# Postgres 真空-问卷深潜

> 原文：<https://medium.com/nerd-for-tech/postgres-vacuum-questionnaire-deep-dive-d4c89a311139?source=collection_archive---------5----------------------->

![](img/ff38d0aaa6e2d5f2eda48eb4572770d7.png)

大象吸尘器

PostgreSQL 的核心概念之一是它的清空过程。我读过一些博客帖子，在这些帖子中，人们谈到了由于事务绕回问题而发生的大规模停机，在某些情况下，停机时间长达几个小时。由于真空过程是非常关键的，因此人们应该了解它的进出。万一我们碰到那些危险的吸尘/缠绕问题，至少我们有更好的装备来处理它。在阅读吸尘概念时，出现了一些问题，我在这里对它们进行了总结。

**问:运行真空作业有并行选项吗？**
**A.** 拖延但不否认。最后，Postgres13 引入了这个非常需要的特性。现在，真空活动可以由并行工作器运行。看了一些博客，我知道只有索引清空/清理是以并行方式发生的，而且每个索引只有一个工作人员。有了这个特性，在具有多个索引的大表上，总的真空运行时间将会有巨大的改进。

```
VACUUM (PARALLEL 3) TESTTABLE;
```

谁知道后来的 Postgres 版本引入了偶数堆和单索引真空/清除并行工作线程(Postgres11 已经引入了并行索引扫描)

**问:如果我终止一个正在运行的真空作业会怎样？会恢复吗？**
**A.** Postgres 在可见性映射中跟踪增量块变化(死元组和冻结块)以避免全表扫描。我们可以通过 pg_visibility 扩展看到这个可见性地图信息。在真空运行期间，这种可见性图检查点经常出现。

```
create extension pg_visibility;
CREATE EXTENSIONcreate table test (age int);
CREATE TABLEinsert into test select generate_Series(1,1000000);
INSERT 0 1000000select * from pg_visibility_map_summary('test');
 all_visible | all_frozen
-------------+------------
           0 |          0
vacuum test; --> Killed from another session
VACUUM
select * from pg_visibility_map_summary('test');
 all_visible | all_frozen
-------------+------------
        8850 |          0
```

在反环绕真空的情况下，要前进`*relfrozenxid,*`，真空必须在一次成功通过中扫描可见性图中的所有页面。

> `*VACUUM*`通常只扫描自上次清空后修改过的页面，但只有当扫描完表格中可能包含未冻结 xid 的每一页时，`*relfrozenxid*`才能前进。当`*relfrozenxid*`比`*vacuum_freeze_table_age*`事务更老时，当`*VACUUM*`的`*FREEZE*`选项被使用时，或者当所有还没有完全冻结的页面碰巧需要清空来移除死行版本时，就会发生这种情况。
> 
> 我还在社区中发起了一个讨论，看看 Postgres 是否可以为每个元组拥有事务 id(可能是一个愚蠢的想法？).社区中关于进一步改进的更多讨论， [1](https://www.postgresql-archive.org/vacuum-freeze-possible-improvements-td6193674.html) 和 [2](https://www.postgresql-archive.org/vacuum-freeze-possible-improvements-td6193674.html) 。

**问:分配更多的维护 _ 工作 _ 内存，VACUUM 可以运行得更快吗？如果我在系统级别(动态参数)更改它，是否会影响已经运行的真空作业？**
**A.** 我做了一些增加 maintenance_work_mem 参数的测试，但是我没有看到已经运行的真空作业有任何改进。当然，新的真空作业会受益。注意 vacuum 可以使用的最大内存为 1 GB。

**问:正常真空(未满)是否会独占锁定工作台？**
**A.** 这可能需要，这取决于表的末尾是否有空块。然后真空试图释放那些块。如果有任何查询正在访问 writer 节点上的表，它会跳过排他锁，但是如果查询正在 Reader 节点上运行，它不会等待/确认(即使 hot_standby_feedback 为 on)。这意味着正常的真空会导致读取器节点上的查询取消。这里，另一个参数 max_standby_streaming_delay 来拯救(折衷副本同步)。

Postgres12 引入了一个特性来禁用这种截断行为。我们可以利用这一点来避免查询取消/延迟问题。

```
alter table testtable set (vacuum_truncate=false);
```

**问:真空作业总是清理死元组吗？**
**A.** 不总是，数据库中长时间运行的事务块通过真空进程删除死元组。在这种情况下，VACUUM job 运行良好，没有任何抱怨，但它不会清除死元组。如果发生这种情况，VACUUM verbose 选项会给出详细信息。

```
postgres=> vacuum verbose testtable;INFO:  "testtable": found 224972 removable, 1255345 nonremovable row versions in 367291 out of 8370336 pages
DETAIL:  1255191 dead row versions cannot be removed yet.
```

> 这种行为可能会在非常繁忙的系统中产生问题。我已经在另一篇[帖子](/nerd-for-tech/postgres-good-for-queuing-implementation-8b7980b6409b)中详细解释了这个场景。我也在社区里提出了[问题](https://www.postgresql.org/message-id/19474.1572022017%40sss.pgh.pa.us)来理解这种行为。

问:只插入表(没有更新/删除)也需要真空吗？
**答:**是的，只插入表格需要真空有两个原因——

*   冻结元组以避免事务绕回问题。
*   为了避免仅索引扫描的性能问题(因为仅索引扫描需要检查可见性映射以查看页面是否全部可见)。

Postgres13 引入了插入触发自动真空的特性。

**问:如何降低桌子上的真空频率？**
**A.** 在更新查询的情况下，Postgres 有一个称为 HOT(仅堆元组)的特性，它在其中添加了一个指针来定位新的行版本。这使得迷你块级真空对表本身。为了利用这个特性，我们应该适当地设置表格填充因子值。我已经在另一篇[帖子](/nerd-for-tech/postgres-fillfactor-baf3117aca0a)中详细解释了 FILLFACTOR。

**问:真空冷冻只存在吗？**
**答:**当我们即将面临交易回绕问题时，仅冻结真空会很有用。那时候我们只担心冻结元组，死元组可以以后再清理。我看到过一些讨论这个话题的帖子，但没有发现这个功能(如果我错了，请纠正我)。这意味着在“真空冻结”操作期间，它还会清除表中的死元组。然而，相反的情况不成立，可能存在仅清理死元组但不冻结元组的真空作业。

**更新:**通过 Postgres12 中的 index_cleanup defer 特性，vacuum 延迟了步进清理阶段，因此对于防缠绕 vacuum 来说非常快。

```
VACUUM (INDEX_CLEANUP False, VERBOSE) testtable;
```

**问:如何加快真空作业的速度？**
**A.** 有自动真空参数，可以根据需要进行调谐/调整。For Ex —自动真空 _ 真空 _ 成本 _ 延迟、自动真空 _ 真空 _ 成本 _ 限制、自动真空 _ 维护 _ 工作 _ 记忆

删除表上未使用的索引有助于加速真空作业。因此，如果事务回绕就在附近，那么为了整个数据库正常运行，可以删除/截断索引或甚至表(对于日志/归档表)。

Postgre12 引入了不清理索引条目的真空操作。Postgres13 引入了一个与并行工作人员一起运行的真空作业。

参数 vacuum_freeze_min_age 可以增加，因此正常的真空作业将做更少的工作，因为它必须冻结更少的元组。

**问:我们如何跟踪真空吸尘器的运行进度？**
a .Postgres 中有检查真空进度的目录对象——pg _ stat _ progress _ vacuum。在这些元数据对象(www.google.com)的基础上构建了一些很好的查询。

有时，我还会检查表中的总块数(pg_freespace ),然后检查 all_visible 块数(pg_visibility_map_summary)是如何随着运行真空而增加的。这给出了还剩多少块的大致想法。

```
select count(*) from pg_freespace(‘testtable’); --total blocks
 count
 — — — — -
1706691--see how many all_visible blocks are there.select a.*,current_timestamp from  pg_visibility_map_summary(‘testtable’) a
 all_visible | all_frozen | now
 — — — — — — -+ — — — — — — + — — — — — — — — — — — — — — — -
 1706691 | 727685 | 2021–04–13 12:25:09.908857+00
```