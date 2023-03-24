# 了解索引如何提高查询性能

> 原文：<https://medium.com/nerd-for-tech/understanding-how-indexes-improve-query-performance-c5de2eae4772?source=collection_archive---------21----------------------->

开始动手之前的一些先决条件是-

1.  下载并安装 SQL Server 2019 开发人员版。
2.  从[https://docs . Microsoft . com/en-us/SQL/samples/wide-world-importers-dw-install-configure 加载教育数据库“WideWorldImporters ”?view=sql-server-ver15](https://docs.microsoft.com/en-us/sql/samples/wide-world-importers-dw-install-configure?view=sql-server-ver15) 进入已安装的 SQL Server developer edition。

让我们开始吧。

**第一步:**

登录 SSMS 并启用“包括实际执行计划”选项。