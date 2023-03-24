# 在 SQL Server 中实现表分区和分区视图(实践)

> 原文：<https://medium.com/nerd-for-tech/implementing-table-partitioning-partitioned-views-in-sql-server-dc1528d1dd6e?source=collection_archive---------0----------------------->

在关于分区的早期文章中，我们试图在一个已经存在的表中创建分区。本文提供了一个在创建表时实现分区的实践练习。

要在新创建的表上实现表分区，需要执行以下操作

创建一个分区方案

创建了一个分区函数

分区方案作为“Create Table”语句的一部分应用。