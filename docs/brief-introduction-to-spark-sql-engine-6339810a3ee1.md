# Spark SQL 引擎简介

> 原文：<https://medium.com/nerd-for-tech/brief-introduction-to-spark-sql-engine-6339810a3ee1?source=collection_archive---------5----------------------->

Spark SQL 允许开发人员/管理员以编程方式对带有模式的结构化数据发出 ANSI SQL:2003 兼容的查询。Spark SQL 是在 1.3 版本中引入的。从那时起，已经在它的基础上建立了几个更高级别的功能

![](img/0f7ded913522cc27b5e3ff890fb27fb2.png)

照片由 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的 [Sunder Muthukumaran](https://unsplash.com/@sunder_2k25?utm_source=medium&utm_medium=referral) 拍摄

## Spark SQL 功能概述

*   生成优化的查询计划和精简 JVM 代码的最终执行。
*   作为使用数据库 ODBC/JDBC 连接器的外部工具的桥梁。
*   增加了读写 JSON、CSV 或 Avro 等各种格式的结构化文件并将它们转换成临时表的能力。
*   连接到 Apache 配置单元 metastore 和表。
*   引入了一个交互式 Spark SQL shell，用于即席和快速数据浏览。
*   统一了 Spark 的各种组件，并允许用 Spark 支持的语言(Java、Scala、Python 和 R)创建数据帧/数据集抽象。

# Spark SQL 的两个主要组件

Spark SQL 的核心是以下两个组件:

## **钨项目**

由几项旨在提高 Spark 应用程序的内存和 CPU 效率的工作组成。它的一个特性是使用快速紧凑的定制格式将数据帧和数据集存储在内存中，而不是使用占用更多内存且操作缓慢的 JVM 对象。

## **催化剂优化器**:

这提供了一个计算查询，并为它输出一个执行计划。我们将在下面简要讨论四个转型阶段:

1.  **分析**:该步骤解析 SQL/DataFrame 查询中引用的列和表名，并为查询生成抽象语法树(AST)。对于列名和表名的解析，将参考内部目录。catalog 是 Spark SQL 的编程接口，它包含列、数据类型、函数、表、数据库等的名称列表。
2.  **逻辑优化**:在这个阶段，通过应用基于规则的优化方法生成一组计划，每个计划都有一个指定的成本。这些计划包括优化，如常数折叠、谓词下推、投影修剪或布尔表达式简化的过程。逻辑计划是下一阶段的输入。
3.  **物理规划**:该阶段为选择的逻辑规划生成物理规划。
4.  **代码生成**:在最后阶段，生成 Java 字节码，以便在集群中的每台机器上执行。Spark SQL 还可以对已经加载到内存中的数据集进行操作，并可以利用先进的编译器技术来生成代码，以加快执行速度。在这个阶段的上下文中，您会经常听说 ***整个代码生成*** 的概念，这是一个物理查询优化阶段，它将整个查询折叠成一个函数，同时删除虚函数调用并使用 CPU 寄存器来获得中间结果。Spark 2.0 附带的第二代钨引擎使用了这种方法，这种简化极大地提高了 CPU 性能和效率。

我们已经对 Spark SQL 引擎及其两个核心组件 Catalyst Optimizer 和钨进行了高度概述，我们将在接下来的博客中涉及更多更深入的 Spark 主题，下次再见！