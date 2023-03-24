# Postgres 填充因子—深潜

> 原文：<https://medium.com/nerd-for-tech/postgres-fillfactor-baf3117aca0a?source=collection_archive---------1----------------------->

![](img/b3affb7099c0d33a13eaf1d4207916fa.png)

在上一篇[帖子](https://virender-cse.medium.com/moving-oracle-to-postgres-be-aware-of-fillfactor-and-indexing-implications-fcec6596827a)中，我们了解了 Oracle 与 Postgres 在 MVCC 性质上的差异，以及如何在 Postgres 中设置适当的填充因子值是有益的。让我们对此进行更深入的探讨。

完全紧凑的表(FILLFACTOR = 100)将导致糟糕的更新性能，还可能导致索引碎片。而低填充因子值会导致表上的顺序扫描变慢，因为它必须读取更多部分填充的块。

**FILLFACTOR 依赖因素:**

为了获得最佳的 FILLFACTOR 值，应该知道表上完整的更新模式，以及一个块可能需要多少空间来存储新版本的元组。

*   更新是批量行更新还是单行更新？批量更新意味着可以在一个块中更新所有行。
*   新的更新数据扩展了行的大小？如果 row 的新版本比旧版本大，那么块将需要更多的空间来容纳它。
*   一行在其生命周期中被更新一次还是多次？如果我们期望一行被多次更新，VACUUM 应该频繁运行以清除死元组，从而为新的/更新的行腾出空间。
*   长时间运行的 SQL 延迟真空清理死元组。如果 VACUUM 在一个表上延迟，死元组将不会被清除。

为简单起见，我们以 OLTP 系统中最常见的例子为例，在该系统中，更新查询通常只影响几行。让我们获取数据库中更新最多的表。

```
select
   schemaname,
   relname,
   pg_size_pretty(pg_total_relation_size (relname::regclass)) as full_size,
   pg_size_pretty(pg_relation_size(relname::regclass)) as table_size,
   pg_size_pretty(pg_total_relation_size (relname::regclass) - pg_relation_size(relname::regclass)) as index_size,
   n_tup_upd,
   n_tup_hot_upd 
from
   pg_stat_user_tables 
order by
   n_tup_upd desc limit 10;schemaname | relname | full_size | table_size | index_size | n_tup_upd | n_tup_hot_upd
------------+----------+-----------+------------+------------+------
 public     | test     | 297 MB    | 182 MB     | 115 MB     |  46171448 |         13382
 public     | testtd   | 334 MB    | 244 MB     | 90 MB      |    260900 |             0
```

**表 FILACTLFOR:**

表填充因子默认值为 100。我们将采用两个用例:

1.  如果 UPDATE query 没有更新属于表上相应索引一部分的任何列。

```
postgres=> create table test(id bigint) with (fillfactor = 90);
CREATE TABLEpostgres=> ALTER TABLE test SET (autovacuum_enabled = false);
ALTER TABLEpostgres=> insert into test SELECT temp.id from generate_series(1, 10000000) AS temp (id);
INSERT 0 10000000postgres=> \dt+ test
Schema | Name | Type  |  Owner   |  Size  |--------+------+-------+----------+--------+
public | test | table | postgres | 383 MB |postgres=> update test set id=id where id % 11 = 0;UPDATE 909090postgres=> \dt+ test
Schema | Name | Type  |  Owner   |  Size  |--------+------+-------+----------+--------+
public | test | table | postgres | 383 MB |postgres=> update test set id=id where id % 11 = 0;
UPDATE 909090postgres=> \dt+ test
Schema | Name | Type  |  Owner   |  Size  | Description
--------+------+-------+----------+--------+-------------
public | test | table | postgres | 383 MB |postgres=> update test set id=id where id % 11 = 0;
UPDATE 909090postgres=> \dt+ test
Schema | Name | Type  |  Owner   |  Size  | Description
--------+------+-------+----------+--------+-------------
public | test | table | postgres | 383 MB |
```

在这种情况下，如果 UPDATE query 正在更新一些记录，那么我们可以将 FILLFACTOR 值保持在较高的值，比如 90–95。这种更新方式适用于 HOT，即使 ctid 改变也不会更新索引条目。随后，由于该块上的空间压力，小型块级真空将自动处理死元组，从而为块中新更新的行腾出空间。

2.如果 UPDATE query 正在更新任何索引列，那么索引条目当然需要进行更改。因此，这里的低 FILLFACTOR 只对使 UPADTE 查询稍微快一点有用，因为它不需要寻找另一个空闲块并将新的行版本迁移到那里。

在这样的表上设置填充因子的权衡是:

> **更快的更新与更慢的顺序扫描和浪费的空间(部分填充的块)**

这种表的 FILLFACTOR 可以从一个值开始，该值是我们怀疑要更新的表中的行的百分比计算值(只是一个大概的数字)。

**For Ex** —应用程序被设计为每一行都需要在创建后不久更新。在这种情况下，FILLFACTOR 值可以设置为 50。

这里的另一个方面是，我们需要确保 VACUUM 经常在这些表上运行，以便它删除死元组，并为块中新的更新行腾出空间。

**注意:**表格创建后，填充因子值可以更改(增加或减少)。如果我们增加 FILLFACTOR 值，它也将被添加到空闲空间映射(FSM)中，因此有资格获得新的行。虽然稍后减少填充因子值，但不会影响已经填充超过新的填充因子值的块。

**B 树索引填充因子:**

Postgres 索引填充因子默认值为 90。索引填充因子的功能与表填充因子不同。这里的区别在于新行是如何产生的——直接插入还是由更新引起的插入(删除然后插入)。

在表的情况下，新行插入服从 FILLFACTOR 值，Postgres 不允许新行进入 FILLFACTOR 值以上的块，而在由更新引起的插入的情况下，它也允许那些新的版本化的行进入 fill factor 值以上的块(仅在可能的情况下在同一块内)。

在使用索引 FILLFACTOR 的另一侧，两种类型的插入以相同的方式处理，即使设置 FILLFACTOR 值为 90，新行也将适合 FILLFACTOR 值以上的块(单调增加的列除外)。

让我们举一个新数据进入索引叶块的例子

1.  **单调递增插入:**

如果新列值与现有键相比是最大的，则它不会填充 FILLFACTOR 值以上的块。发生块分割，新条目将进入新块。

在下面的示例中，我们看到了两个索引之间的大小差异，因为 FILLFACTOR 值为 90 的索引将在块中保留 10%的空间用于进一步的更新，因此当更新发生在该索引上时，其大小保持不变。

```
postgres=> create table test(id bigint);
CREATE TABLEpostgres=> CREATE INDEX idx1_test ON test (id) with (fillfactor = 100);
CREATE INDEXpostgres=> CREATE INDEX idx2_test ON test (id); --fillfactor = 90 
CREATE INDEXpostgres=> insert into test SELECT temp.id from generate_series(1, 10000000) AS temp (id) ;
INSERT 0 10000000postgres=> \di+ idx1_test
Schema |   Name    | Type  |  Owner   | Table |  Size  |--------+-----------+-------+----------+-------+--------+
public | idx1_test | index | postgres | test  | 193 MB |postgres=> \di+ idx2_test
Schema |   Name    | Type  |  Owner   | Table |  Size  |
--------+-----------+-------+----------+-------+--------+
public | idx2_test | index | postgres | test  | 214 MB |postgres=> update test set id = id+1 where id%100=0;
UPDATE 100000postgres=> \di+ idx1_test
Schema |   Name    | Type  |  Owner   | Table |  Size  |
--------+-----------+-------+----------+-------+--------+
public | idx1_test | index | postgres | test  | 386 MB |postgres=> \di+ idx2_test
Schema |   Name    | Type  |  Owner   | Table |  Size  |
--------+-----------+-------+----------+-------+--------+
public | idx2_test | index | postgres | test  | 214 MB |
```

**2。随机插入:**

如果一个新的列值以随机顺序出现，那么它将填充块直到 100%,然后发生 50-50 分割。

在下面的例子中，我们看到，填充因子值为 90 和 100 的索引大小几乎相同。

这里的索引大小与上一个示例中的 100%压缩索引相比更大，因为 50–50 分割，因此许多块被部分填充。

```
postgres=> insert into test SELECT ceil(random() * 10000000) from generate_series(1, 10000000) AS temp (id) ;
INSERT 0 10000000postgres=> \di+ idx1_test
Schema |   Name    | Type  |  Owner   | Table |  Size --------+-----------+-------+----------+-------+--------+
public | idx1_test | index | postgres | test  | 278 MB |postgres=> \di+ idx2_test
Schema |   Name    | Type  |  Owner   | Table |  Size  |
--------+-----------+-------+----------+-------+--------+
public | idx2_test | index | postgres | test  | 280 MB |postgres=> update test set id = id+1 where id%100=0;
UPDATE 99671postgres=> \di+ idx1_test
Schema |   Name    | Type  |  Owner   | Table |  Size  |--------+-----------+-------+----------+-------+--------+
public | idx1_test | index | postgres | test  | 281 MB |postgres=> \di+ idx2_test
Schema |   Name    | Type  |  Owner   | Table |  Size  |
--------+-----------+-------+----------+-------+--------+
public | idx2_test | index | postgres | test  | 282 MB |
```

现在，如果我们重新索引这些索引，它们将服从 FILLFACTOR 值。这就像在数据单调增加的表上创建一个新索引。

```
postgres=> Reindex index idx2_test;
REINDEXpostgres=> \di+ idx2_test     --fillfactor 90
Schema |   Name    | Type  |  Owner   | Table |  Size  |--------+-----------+-------+----------+-------+--------+
public | idx2_test | index | postgres | test  | 214 MB |postgres=> Reindex index idx1_test;
REINDEXpostgres=> \di+ idx1_test    --fillfactor 100
Schema |   Name    | Type  |  Owner   | Table |  Size  |
--------+-----------+-------+----------+-------+--------+
public | idx1_test | index | postgres | test  | 193 MB |
```

通过以上解释，我们看到索引填充因子在以下情况下是有用的:

*   存在一个预填充的表，我们需要创建一个新的索引，然后期望对表索引列进行大量更新(即使更新的列是其他索引的一部分，因为 ctid 的更改将传播到所有索引)。
*   索引在右侧扩展的表(最大的键值)，我们希望对表索引列进行许多更新。

**结束语:**我看到 Oracle 对表和索引的 PCTFREE 默认值分别是 90 和 100，而 Postgres 对表和索引分别选择 FILLFACTOR 值为 100 和 90。

对于需要在许多表上使用 DML 的 OLTP 用例，您认为表和索引的默认值 90 和 100 更有意义吗？**欢迎在评论区提出建议、反馈和体验。**