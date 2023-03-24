# py spark——让我们数数产品

> 原文：<https://medium.com/nerd-for-tech/pyspark-lets-count-products-a19e611c582e?source=collection_archive---------18----------------------->

大数据领域日益发展，支持大数据的技术也日益发展。Spark 是另一个有趣的分布式计算平台。Spark 的出现是为了改善 Hadoop MapReduce 的缺点。在这篇简短的文章中，我们将使用 pyspark 解决一个简单的大数据问题。

![](img/e8fc9220baad03c6ec5b48277003b35a.png)

## 问题

我们有一个公司 2009 年 1 月的销售数据集，我们需要计算全国的产品销售额。数据集可在[处获得。](https://github.com/isurunuwanthilaka/pyspark-product-count/blob/main/SalesJan2009.csv)

头的数据集如下。

请注意，我们在第 8 列中有 country。

## 解决办法

我们将创建一个 mapreduce 作业来用 spark 解决这个问题。完全码👇

**让我们一行一行地讨论代码。**

`L0`表示第 0 行。

L11 创建一个 SparkSession

L16 存储输出路径，这里是 s3 存储桶位置。

L17 读取文件并将数据放入 RDD，这里我们解析了上传数据集的 S3 存储桶位置。

L19-L21 定义了映射函数。在这里，我们分割线，得到国家，并将`(K,V)`解析为`(Country,1)`，即`(US,1)`。

L23 是实际的 mapreduce 作业运行程序。它会大量减少。`add`功能取自运营商套餐。

在映射器中我们返回`(K,V)`，在缩减器中我们有`(K, list(V))`。因此`add`功能会减少`list(v)`。举个例子，如果我们得到`key:US`和`values:1,1,1,1,...,1`，那么首先 1，1 将被解析为`add`，结果 2 将是下一次迭代的第一个值。因此，2，1 将被再次解析为`add`和`3,1`、`4,1`、`5,1`，最终`sum`将作为`(US,sum)`返回。

L26 创建一个模式以形成数据帧，从而将结果保存到文件中。这里我们有两个类型为`StringType()`的列。

L30 将数据帧作为 CSV 写入 s3 存储桶位置。

这是一个非常小的 pyspark 工作，演示了我们如何简单地解决大数据问题的工作流程。外面有很多资源。我在 youtube 上创建了一个视频指南。如果你有兴趣，可以去看看。

![](img/90f66d3c696d24012ae033cf97511d1e.png)

注意安全，远离新冠肺炎，快乐编码！

*原载于 2021 年 4 月 24 日*[*https://isurunuwanthilaka . github . io*](https://isurunuwanthilaka.github.io/software-engineering/2021/04/24/spark-product-count.html)*。*