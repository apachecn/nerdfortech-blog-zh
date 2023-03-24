# Spark 远程作业提交到 AWS EMR 上运行的 Yarn

> 原文：<https://medium.com/nerd-for-tech/spark-remote-job-submission-to-yarn-running-on-aws-emr-5d6ffd8cb373?source=collection_archive---------0----------------------->

## Spark 远程作业提交允许客户端从任何地方向 Yarn 集群提交 Spark 作业，将客户端从 Yarn 集群中分离出来。

![](img/05f7ddd6275c12888fedf0a7079ac7d3.png)

照片由 [Fotis Fotopoulos](https://unsplash.com/@ffstop?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Spark 远程作业提交允许客户从任何地方向 Yarn 集群提交 Spark 作业。这也可以用来将 Spark 作业提交给运行不同版本 Hadoop 和 Spark 的不同 Yarn 集群。

# 问题陈述

我们使用 AWS EMR 集群和 Yarn 作为资源管理器来运行我们的 Spark 作业。我们希望远程向 EMR 集群提交作业，而不需要对主节点使用 ssh，也不需要直接在主节点上运行 spark-submit 命令。

# 解决办法

利用 Spark 的远程作业提交功能，从远程任何地方向 Yarn cluster 提交作业。

## **远程实例的基本要求:**

**1。Java 和 Python 运行时**

**2。Hadoop 和 Spark 二进制**

远程主机上应该有二进制文件(不需要安装)。远程计算机和目标 EMR 群集中的二进制文件版本应该匹配。

二进制文件可以在[这里](https://spark.apache.org/downloads.html)下载，也可以从 EMR 主节点的以下位置***/usr/lib/Hadoop***&***/usr/lib/spark***复制。如果没有从 EMR 集群复制二进制文件，请确保从 EMR 集群复制 emrfs-hadoop-assembly jar。

将二进制文件放在远程实例中，并设置以下环境变量。

*   *导出* HADOOP_HOME= *<* 路径到 HADOOP 二进制文件夹 *>*
*   *导出* SPARK_HOME= *<* 路径到 SPARK 二进制文件夹 *>*

**3。EMR 集群的 Hadoop 和 Spark 配置文件**

从/etc/hadoop/conf 复制 Hadoop 配置文件 yarn-site.xml、core-site.xml、mapred-site.xml，并从/etc/spark/conf 复制 spark 配置文件 hive-site.xml。

这些配置文件将向 Spark 提供关于 EMR 集群的信息，比如在运行 spark-submit 时，哪个是主节点、资源管理器和 hive metastore 要连接到的。将配置文件存储在远程实例的文件夹中，并设置以下环境变量。

*   *导出*HADOOP _ CONF _ 目录= *<* 路径到 hadoop configs 文件夹 *>*
*   *导出*SPARK _ HIVE _ XML _ CONF _ DIR =*<*路径到 spark configs 文件夹 *>*

## **运行火花提交**

如下所示运行 spark-submit 命令，将 spark deploy 模式设置为集群模式。

```
*$*SPARK_HOME/bin/spark-submit --master yarn --deploy-mode cluster --conf spark.yarn.dist.files=*${*SPARK_HIVE_XML_CONF_DIR*}*/hive-site.xml sample-spark-code.py
```

现在，spark-submit 在远程实例上运行，将实际的 spark 作业提交给 EMR 集群。

# 结论

*   通过利用 spark 的远程作业提交特性，我们可以从远程的任何地方向 Yarn 集群提交 Spark 作业，而不需要对主节点使用 SSH，或者直接在主节点上运行 spark-submit。
*   远程实例可以有不同版本的 Hadoop 和 Spark 二进制文件。通过根据运行时的需求为每个会话设置指向特定版本的二进制文件夹的环境变量，我们可以使用多个版本的 Spark & Hadoop 提交 Spark 作业。
*   我们可以在远程实例中存储不同 EMR 集群的 Spark 和 Hadoop 配置文件，并通过为每个会话设置指向特定集群配置文件夹的环境变量，在运行时选择不同的 EMR 集群。
*   远程实例可以是 Docker 容器或 Kubernetes pod，其映像中嵌入了 Hadoop 和 Spark 二进制文件。
*   这样，我们可以将提交 Spark 作业的客户机从 EMR 集群中分离出来。支持客户端使用多个版本的 Spark 和 Hadoop 向多个 EMR 集群提交 Spark 作业。