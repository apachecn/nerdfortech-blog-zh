# 如何优化数据库性能并扩展它？数据库着色前要做的事情。

> 原文：<https://medium.com/nerd-for-tech/how-to-optimise-the-database-performance-and-scale-it-d98bdb78a45d?source=collection_archive---------0----------------------->

有几种方法可以优化数据库以提高性能并使其更具可伸缩性。您可能听说过的最流行的概念之一是数据库分片，它基本上是向外扩展数据库。但是 [**数据库阴影**](/nerd-for-tech/all-about-database-sharding-scaling-up-the-database-3b6172491cd) 可能是你最后一个优化数据库的选择。那么，让我们看看还有哪些优化数据库的选项。

![](img/a1cd4cd363027bd29ceed3a131ab9b81.png)

扬·安东宁·科拉尔在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 优化查询语句

![](img/d06f60e507eaf798d5f40b340236bc59.png)

卡斯帕·卡米尔·鲁宾在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

这是优化数据库的第一步。如果我们编写一个使用更多数据库处理能力的查询，那么它将影响整体性能。现在问题来了，如何优化查询？

*   **只选择那些必需的字段:**在选择查询中尽量避免`*`。如果查询是针对不需要的字段，那么它将在 RAM 中分配更多的内存来处理，这会影响整体性能。
*   **限制数据输出:**避免编写查询来一次获取所有数据。假设您有一个包含大量数据的表，并试图一次获取所有数据，那么 CPU 内核将被一个进程阻塞很长时间。您可以使用`limit`、`offset`来获取有限数量的数据。基本上，我们对数据进行分页。
*   **避免不必要的连接操作:**连接操作根据特定条件合并两个表数据。因此，要确保数据库体系结构是以这样一种方式设计的:只需要执行最少的连接操作，并在必要时执行连接操作。

# 索引栏

![](img/9ee9c24c6518910610cce4e14d3045b8.png)

由 [Maksym Kaharlytskyi](https://unsplash.com/@qwitka?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

DBMS 中的索引是一种数据结构，用于快速定位和访问数据库表中的数据。

想象一下，你有大量的数据，想要搜索一个数据，那么你需要搜索整个数据堆。如果您可以根据您知道需要在哪个类别上搜索数据的类别，将数据分类，那会怎么样？你进入那个类别，在有限的数据上搜索。

这就是索引的工作方式，我们用于搜索的列可以被索引，这在上面的例子中被认为是类别排列。索引用于优化数据库的性能，方法是在处理查询时最大限度地减少所需的磁盘访问次数。

但是并不总是有优化的性能，它也有其局限性。如果数据是公平分布的，或者为某个特定值分配了大量数据，那么性能不会有太大变化。

# 主从架构

![](img/108d9e4b3cc8bdd35719c5eed3003855.png)

[KOBU 机构](https://unsplash.com/@kobuagency?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

如果所有的方法都应用在数据库中，那么我们就可以实现主从架构。那么什么是主从架构呢？

主从架构是一种创建数据库副本的方法。上的将是主副本，其他副本是主副本的从副本。主机处理写操作，从机处理读操作。

基本上，我们只是根据任务分配了工作量。对于写操作，我们称之为主节点，对于读操作，我们称之为从节点。如果主设备发生任何变化，从设备会跟随主设备并与之保持同步。

通常，与写操作相比，要执行更多的读操作，因此我们可以通过分配任务来创建多个从设备以提高性能。

# 多个母版

![](img/0aae1b535feae82c547b3dc59198d612.png)

照片由[艾米·赫希](https://unsplash.com/@amyhirschi?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

这种方法是主从架构的扩展。假设现在您的写操作工作量增加了很多，在这种情况下，我们可以创建多个主节点。这些主节点可以基于某些类别来创建，例如地理特定的类别。并且两个主节点将彼此同步。

以 Instagram 为例，世界各地都有很多人上传他们的内容。因此，为了处理该负载，在它们的地理位置附近可以有一个主节点，在那里内容将首先被更新。然后，它将与其他主节点同步。

通过这种方式，更新内容或从世界任何地方访问内容的延迟将会很低。

# 存档数据/数据分区

![](img/9d5bfe6e4172086d1aecad363c324919.png)

照片由 [Giammarco](https://unsplash.com/@giamboscaro?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

一旦应用了上述所有方法，并且表的数据变得太大并且需要时间来查询。那么我们可以采用以下方法:

*   归档很可能不经常需要的旧数据(例如通知数据)。
*   数据分区，根据类别对数据进行分区。这听起来可能类似于数据库分片，但事实并非如此。分片是基于某种条件将数据划分到多个数据库中。分区就是根据某种条件将数据划分到多个表中。

# 结论

[**数据库分片**](/nerd-for-tech/all-about-database-sharding-scaling-up-the-database-3b6172491cd) 任重道远。很有可能我们永远不需要实现数据库分片。数据库分片是提高数据库性能的最后一个选择。很少需要对数据库进行分片。你可以在我的 [**数据库分片**](/nerd-for-tech/all-about-database-sharding-scaling-up-the-database-3b6172491cd) 博客中阅读更多关于数据库分片的内容。