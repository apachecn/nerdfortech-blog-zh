# Postgres —使用动态日期过滤器的部分索引

> 原文：<https://medium.com/nerd-for-tech/postgres-performance-tuning-with-partial-index-cffc411f719a?source=collection_archive---------6----------------------->

![](img/6dfd51236c033bbffb2bbe8828d777a1.png)

最近，我在 Postgres 中微调了一个运行缓慢的查询。像往常一样，我检查了为该查询传递的不同绑定变量以及表中这些值的计数，最后提出了一个额外的索引来加速查询。

现在的问题是，Prod 表的大小是 900GB(不幸的是，这个表上没有实现分区)，在这样一个巨大的表上创建索引花费了大约 9 个小时(在从快照恢复的数据库上测试，当然是在进行 EBS 初始化之后)。我们希望在 4 个小时的低峰值时间窗口内完成索引创建，并将此活动对整体数据库性能的影响降至最低。

**查询:**实际的查询有点复杂，有多个表连接在一起，但是为了这篇文章，我们将重点关注部分索引，所以下面只是一个虚拟查询。

```
SELECT * FROM TESTTABLE WHERE ((asn, vsid ) IN ('B09JKJK', 'Unad45F')) and last_updated > current_Date — 360;
```

正如我们所看到的，谓词列 **last_updated** 需要最近一年的数据，那么为什么要在完整的表数据上创建索引呢(表中有 20 年的数据)。因此，我们想到在 WHERE 子句中创建一个带有硬编码日期的部分索引(包含最近 1 年的数据)。这个部分索引的大小很紧凑，创建时间也更短(在我们的例子中，索引创建时间减少到 4 小时)。

```
create index CONCURRENTLY idx_tab_new  ON testtable USING btree (vsid,asn,last_updated,last_updated) where last_updated  > '01-01-2020';
```

**快速笔记:**

1.  很明显，我们不能用动态过滤器创建部分索引。

```
create index idx on testtable (last_updated) where last_updated > current_date — 360;ERROR: functions in index predicate must be marked IMMUTABLE
```

2.我在索引定义中添加了 last_updated 列以及 vsid 和 asn，因为将来该索引将保持增长并包含一年以上的数据，而索引中的 time last_updated 列将有助于进一步过滤数据。

我最初的想法是，现在 query 将选择部分索引并在更短的时间内运行，但事实并非如此，它仍然是 seq scan。

**为什么？**查询具有动态过滤器**last _ updated>current _ Date—360**，因此 planner 无法识别部分索引仍然适用，因为它具有上一年的数据(截至 2021 年)。计划者期望在查询中给出与我们在索引中给出的相同的 WHERE 子句。那我们就这么做吧。

在应用程序中部署了新代码，用变更号作为提示(这样将来任何人都可以理解我们为什么添加了另一个谓词)。乍一看，这两个过滤条件看起来很幼稚，但这就是我如何让它工作。

```
SELECT * FROM TABLE WHERE 
last_updated > current_Date — 360
and last_updated  >= '01-01-2020' --CHANGE NUMBER 'NNNNNN'
;
```

现在，查询可以选择索引并运行得更快。

另一方面，部分索引非常有用，我们可以给出一个 SELECT 查询 **where 子句**，因为它在部分索引中。DMLs(插入/更新)会有一些开销，因为需要为每个元组插入/更新计算 where 子句。

我们有另一个查询，其中对 status_flags 列执行“**逐位&** ”操作，并且该查询在一天内执行了数千次。所以我们用完全相同的 WHERE 子句创建了一个部分索引来加速查询(**让我知道是否有更好的方法来处理其他类型的索引**

```
create index idx on test (lid,vcode,isname) where
( (
Cast(status_flags AS BIGINT) & 128) > 0 )
AND ( (
Cast(status_flags AS BIGINT) & 2) = 0
AND (
Cast(status_flags AS BIGINT) & 32) = 0
AND (
Cast(status_flags AS BIGINT) & 64) = 0 );
```