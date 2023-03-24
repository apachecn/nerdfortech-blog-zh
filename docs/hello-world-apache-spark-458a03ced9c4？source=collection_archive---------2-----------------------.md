# 嘿，阿帕奇火花

> 原文：<https://medium.com/nerd-for-tech/hello-world-apache-spark-458a03ced9c4?source=collection_archive---------2----------------------->

如果你熟悉大数据，那么你可能听说过 Apache Spark。Spark 是一个强大的开源数据处理引擎，可以轻松分析大型数据集。在本帖中，我们将看看为什么 Spark 如此受欢迎的一些原因，并探索它的一些关键特性

Apache Spark 是一个计算引擎，Apache Spark 是一个计算引擎和一系列大数据工具。它具有流式传输、查询数据集、机器学习(Spark MLlib)和图形处理(GraphX)等功能。在本帖中，我们将回顾什么是 Apache spark 及其用例。Spark 是用 Scala 开发的，但是也有 Python、Java、SQL 和 R 的绑定。Spark 完全依赖于内存处理，这使得它比相应的 Hadoop 功能的性能快几倍。

![](img/6126682b2c46ed9d0a377643a551aef4.png)

照片由[达维德·扎维亚](https://unsplash.com/@davealmine?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 概观

Spark 的核心是一个弹性分布式数据集(RDD)，这是一个分布在一个机器集群上的只读多数据项集，跨操作维护在内存中。这种基本的抽象使 Spark 能够支持批处理式计算，例如 map-reduce 作业，以及交互式查询。rdd 是从 HDFS 或任何 Hadoop 支持的文件系统中的一个或多个外部数据集开始构建的，并使用并行操作(如 map、reduce、join 和 cogroup)来转换这些数据。

作为一个独立的集群管理器，Spark 支持自动化常见的集群资源管理任务(例如，监控、调度)，利用内置的原语，这些原语可以很容易地组合到更高级别的应用程序逻辑中。该库可以在标准 Hadoop 发行版上获得，也可以单独下载。

Spark 是一款多用途发动机。它可用于批处理，对存储在 HDFS 或其他文件系统中的数据集进行交互式查询。为了处理来自不同来源的流数据，Apache Spark 通过 Spark Streaming 提供了实时流处理功能，可以分析和处理内存中或通过磁盘持久性的数据。在 Apache Spark 的未来版本中，它将支持新的前端语言，如 R 和 Python，以提供针对大型数据集的交互式分析，而无需我们编写任何代码。

# MapReduce 和 Spark 比较

随着 Spark 的出现，MapReduce 框架退居二线，原因如下:

*   **迭代作业:**某些机器学习算法对数据集进行多次遍历以计算结果。每一次传递都可以表示为一个不同的 MapReduce 作业。但是，每个作业从磁盘读取其输入数据，然后将其输出转储到磁盘，供下一个作业读取。当涉及磁盘 I/O 时，与从主存储器访问相同的数据相比，作业执行时间增加了许多倍。
*   **交互分析:**用户可以使用 Hive 或 Pig 等工具对大型数据集运行特定的 SQL 查询。如果用户发出针对同一个数据集的多个查询，每个查询可以转换为一个 MapReduce 作业，从磁盘读取同一个数据集，并对其进行操作。让多个 MapReduce 作业从磁盘读取同一个数据集是低效的，并且增加了查询执行延迟。
*   **丰富的 API:**Spark 通过提供各种丰富的 API，可以简洁地表达一个操作，否则在 MapReduce 中表达时会包含许多行代码。与 MapReduce 相比，使用 Spark 时，用户和开发人员的体验相对简单。

# 弹性分布式数据集

*   **弹性:**这意味着 RDD 是容错的，能够重新计算由于节点故障而丢失或损坏的分区。这种自我修复是通过使用 RDD 谱系图实现的，我们将在后面介绍。RDD 记得它是如何达到当前状态的，并且可以追溯到重新计算任何丢失分区的步骤。**分布式:**当组成 RDD 的数据分布在一个机器集群上时。数据集:这是指我们处理的数据记录的表示。可以通过 JDBC 使用各种来源加载外部数据，如 JSON 文件、CSV 文件、文本文件或数据库。

# RDD 的属性

*   **弹性:**这意味着 RDD 是容错的，能够重新计算由于节点故障而丢失或损坏的分区。这种自我修复是通过使用 RDD 谱系图实现的，我们将在后面介绍。RDD 记得它是如何达到当前状态的，并且可以追溯到重新计算任何丢失分区的步骤。
*   **分布式:**当组成 RDD 的数据分布在一群机器上时。
*   **数据集:**这是指我们处理的数据记录的表示。可以通过 JDBC 使用各种来源加载外部数据，如 JSON 文件、CSV 文件、文本文件或数据库。

# 为什么要从 Hadoop 转型

如果您正在使用 Hadoop，并希望提升您的数据处理水平，迁移到 Spark 可能是一个不错的选择。

然而，所有的迁移都需要花费工时。以下是一些迁移的好理由:

*   Spark 在内存中进行处理，比 Hadoop MapReduce 快很多倍。

Spark 声称比 Hadoop MapReduce 快 100 倍。

项目的采用带来了高质量的文档、课程和书籍，可以帮助您正确地完成它。相反，在专门处理文本流的 MapReduce 上，Spark 中有各种高级抽象，最常见的是 RDD。

*   几乎所有我们需要处理的数据都可以通过 Spark 来完成。如上所述，Spark 可以执行流式活动、机器学习、查询数据和图形处理。当然，我们可以用 RDD 级别做任何数据操作。
*   用于交互式数据争论的 Spark shell。就像我们在 Python shell 中构建 Python 代码的原型一样，我们也可以在 spark-shell 中用我们的 spark 作业做同样的事情。

# MLlib 与 Tensorflow/Pytorch

围绕机器学习有许多超级流行和有据可查的框架，如 Tensorflow 和 Pytorch。所以用 MLlib 出现？

使用 Spark 生态系统中任何工具的一个重要原因是它的分布式性质。除了执行内存计算，Spark 还可以在分布式文件系统上执行。

这有助于扩展流程，并且您不必学习可能与 Spark 不兼容的第二种技术。

还记得上节课的网球例子吗？我们可以写一个模型来预测下一个球的颜色。由于我们已经在 HDFS 有了数据，我们可以利用与 HDFS 的 Spark 集成，并在那里运行我们的机器学习模型。

# Spark 和 Hadoop MapReduce

Hadoop 问世时是大数据处理的突破。虽然它仍然是一个受欢迎的工具，但 Spark 在许多方面都优于它，比如性能和实时需求。

Spark 依靠内存运行，仅此一点就能改变游戏规则。

# 火花的应用

*   趋势计算。
*   商业智能。
*   使用 GraphX 的 TextRank 等图形算法对语料库进行汇总。
*   使用 Spark 流和 MLlib 实时检测欺诈性支付。
*   用 Spark 流实现 ETL 管道。

要了解 Spark 如何工作的更多信息，请访问:[https://Spark . Apache . org/docs/latest/index . html # how-it-works](https://spark.apache.org/docs/latest/index.html#how-it-works)

Apache Spark 是一个开源集群计算系统，可用于对大型数据集进行分析。你对它的工作原理了解得越多，你就能更好地分析大数据，并通过机器学习技术利用它的优势，下次再见