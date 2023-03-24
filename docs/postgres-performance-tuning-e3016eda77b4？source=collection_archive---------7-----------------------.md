# Postgres —查询性能调优

> 原文：<https://medium.com/nerd-for-tech/postgres-performance-tuning-e3016eda77b4?source=collection_archive---------7----------------------->

![](img/3c985035055598b3b2b3f56513272a87.png)

**场景:**

我们有一个 Prod 数据库运行在 Amazon Aurora PostgreSQL 上。在使用 CloudWatch Logs Insights 进行慢速查询分析期间(这是另一篇文章)，我在大约 10 秒钟内运行了一个查询。(如果所有数据块都在缓存中，则磁盘读取需要 5 分钟以上的时间)。

是的，这里的一个选项是使用 **pg_prewarm** 扩展简单地缓存数据(索引)。

**查询:**

```
explain (analyze, buffers)
SELECT tabalias.rid,
       tabalias.col1,
       tabalias.col2,
       tabalias.col3,
       tabalias.col4,
       tabalias.dis_id,
       tabalias.col5,
       tabalias.processed_date,
       tabalias.col6,
       tabalias.col7,
       tabalias.col8,
       tabalias.col8,
       tabalias.col9,
       tabalias.col10,
       tabalias.col11,
       tabalias.o_id,
       tabalias.col12
FROM   testtable tabalias
WHERE tabalias.processed_date >= '18-dec-2020'
AND   tabalias.processed_date <= '18-jan-2021'
AND   (tabalias.o_id IN ('US'))
AND   (tabalias.dis_id IN ('VAL1','VAL2','VAL3','VAL4','VAL5'))
ORDER BY tabalias.rid;
```

**注意:**我已经更改了这篇博文的表格和列名。

**当前查询计划:**

```
Sort  (cost=508512.99..508600.74 rows=35097 width=124) (actual time=9197.968..9198.468 rows=4707 loops=1)
   Sort Key: rid
   Sort Method: quicksort  Memory: 1443kB
   Buffers: shared hit=470537
   ->  Bitmap Heap Scan on testtable tabalias  (cost=462676.21..505863.34 rows=35097 width=124) (actual time=9187.705..9195.001 rows=4707 loops=1)
         Recheck Cond: (((dis_id)::text = ANY ('{VAL1,VAL2,VAL3,VAL4,VAL5}'::text[])) AND (processed_date >= '2020-12-18 00:00:00'::timestamp without time zone) AND (processed_date <= '2021-01-18 00:00:00'::timestamp without time zone))
         Filter: ((o_id)::text = 'US'::text)
         Rows Removed by Filter: 3
         Heap Blocks: exact=4589
         Buffers: shared hit=470537
         ->  **BitmapAnd**  (cost=462676.21..462676.21 rows=39906 width=0) (actual time=9186.433..9186.433 rows=0 loops=1)
               Buffers: shared hit=465948
               ->  Bitmap Index Scan on dis_id_indx  (cost=0.00..9451.54 rows=733641 width=0) (actual time=60.723..60.723 rows=229752 loops=1)
                     Index Cond: ((dis_id)::text = ANY ('{VAL1,VAL2,VAL3,VAL4,VAL5}'::text[]))
                     **Buffers: shared hit=1256**
               ->  Bitmap Index Scan on vr_acct_dist_leid_idx  (cost=0.00..453206.87 rows=22201730 width=0) (actual time=9038.687..9038.687 rows=22853543 loops=1)
                     Index Cond: ((processed_date >= '2020-12-18 00:00:00'::timestamp without time zone) AND (processed_date <= '2021-01-18 00:00:00'::timestamp without time zone))
                     **Buffers: shared hit=464692**
 Planning time: 0.383 ms
 Execution time: 9199.731 ms
```

查看查询计划，我们可以看到 Planner 对两个筛选列使用了两个索引— **dis_id** 和 **processed_date** ，然后对这两个索引执行位图索引连接。

> **位图索引连接:**位图索引连接在谓词列上有单独索引而没有复合索引的情况下很有用。它就像两个索引的内部连接。在连接索引后，如果得到的行数较少，那么表堆访问的开销会更小，因为只需要对少数元组进行堆扫描。

现在来看一下**缓冲区:共享命中**，这里成本较高的部分是 processed_date 上的第二次索引扫描，因为这是由于查询中的 1 年过滤条件而扫描大量数据。让我们获得两个过滤器列的计数/基数细节。

```
select count(*), count(distinct dis_id), count(distinct processed_date) from testtable;count   | count  |  count
-----------+--------+---------
 408243990 | 492622 | 9517490
```

在 **pg_stats** 表格中查找统计数据。

```
select tablename,attname,n_distinct from pg_stats where tablename='testtable' and attname in ('dis_id','processed_date'); tablename        |    attname     | n_distinct
------------------------+----------------+------------
 testtable | dis_id |       **4516**
 testtable | processed_date |      **16433**
```

我们可以看到 dis_id 列有大约. 5M 个不同的值，还不错。我们可以猜测，如果计划程序可以只在 **dis_id** 列上使用索引，然后在堆扫描之后过滤 processed_date 上的值，这可能是一个更好的计划。与其他许可的 RDBMS 不同，Postgres 没有提供太多的选项来修改执行计划(是的，我们必须付出巨大的代价来获得这些特性)。让我们试着改变计划。

**工作 1:** 由于 default_statistics_target 默认为 100 块，我把这个增加到 300 以增加样本量，并对表进行了分析。我们可以看到， **n_distinct** 两列的值都增加了，而计划两列保持不变。

```
set default_statistics_target=300;
SET
analyze vr_distributor_returns;
ANALYZEselect tablename,attname,n_distinct from pg_stats where tablename='testtable' and attname in ('dis_id','processed_date');
       tablename        |    attname     | n_distinct
------------------------+----------------+------------
 testtable | dis_id |      **11130**
 testtable | processed_date |      **44499**
```

**工作 2:** 我只为列 **dis_id** 设置了统计参数，但是在这种情况下，这两个列的 n_distinct 也增加了，而 plan 保持不变。

```
alter table testtable alter column dis_id set STATISTICS 300;
ALTER TABLE
analyze testtable;
ANALYZEselect tablename,attname,n_distinct from pg_stats where tablename='testtable' and attname in ('dis_id','processed_date');
       tablename        |    attname     | n_distinct
------------------------+----------------+------------
 testtable | dis_id |      **11130**
 testtable | processed_date |      **44499**
```

**工作 3:** 使用 n_distinct 参数对 dis_id 列的 n_distinct 值进行硬编码。它起作用了，我们可以看到只有 dis_id 列的 n_distinct 值增加了，现在计划只在这个列上选择索引。新的计划查询在不到一秒钟的时间内运行。

```
alter table testtable alter column dis_id set (n_distinct = 500000);
ALTER TABLE
analyze testtable;
ANALYZEselect tablename,attname,n_distinct from pg_stats where tablename='testtable' and attname in ('dis_id','processed_date');
       tablename        |    attname     | n_distinct
------------------------+----------------+------------
 testtable | dis_id |     **500000**
 testtable | processed_date |      **44499**
```

**新查询计划:**

```
Sort  (cost=5369.54..5370.29 rows=303 width=124) (actual time=222.474..223.554 rows=4707 loops=1)
   Sort Key: rid
   Sort Method: quicksort  Memory: 1443kB
   Buffers: shared hit=1295
   ->  Index Scan using dis_id_indx on testtable dbdistribu0_  (cost=0.57..5357.05 rows=303 width=124) (actual time=2.665..219.316 rows=4707 loops=1)
         Index Cond: ((dis_id)::text = ANY ('{VAL1,VAL2,VAL3,VAL4,VAL5}'::text[]))
         Filter: ((processed_date >= '2020-12-18 00:00:00'::timestamp without time zone) AND (processed_date <= '2021-01-18 00:00:00'::timestamp without time zone) AND ((o_id)::text = 'US'::text))
         Rows Removed by Filter: 224579
         **Buffers: shared hit=1295**
 Planning time: 0.574 ms
 Execution time: 223.996 ms
```

**工作 4:** 如果 n_distinct 也不起作用，我们仍然可以使用查询重写选项来实施新的计划。基本上，首先用子句在**的 dis_id 列过滤表，然后在主查询中过滤 processed_date 的数据。**

```
WITH TEMPTABLE AS 
(SELECT tabalias.rid,
       tabalias.col1,
       tabalias.col2,
       tabalias.col3,
       tabalias.col4,
       tabalias.dis_id,
       tabalias.col5,
       tabalias.processed_date,
       tabalias.col6,
       tabalias.col7,
       tabalias.col8,
       tabalias.col8,
       tabalias.col9,
       tabalias.col10,
       tabalias.col11,
       tabalias.o_id,
       tabalias.col12
FROM   testtable tabalias
where    (tabalias.o_id IN ('US'))
AND   (tabalias.dis_id IN ('VAL1','VAL2','VAL3','VAL4','VAL5')
)
SELECT * FROM temptable 
WHERE tabalias.processed_date >= '18-dec-2020'
AND   tabalias.processed_date <= '18-jan-2021'
ORDER BY tabalias.rid;
```

**闭幕词:**

我们还可以在( **dis_id，processed_date** )上创建一个新的复合索引，以获得该查询的最小延迟。尽管表的大小超过了 500GB，这也是我们希望避免在生产数据库中使用新索引的原因。

**提示:** Postgres 的另一个特性是使用“CREATE STATISTICS”命令创建相关的统计数据。这可用于向规划者提供更多关于列相关性的信息。