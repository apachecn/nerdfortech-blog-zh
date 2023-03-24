# Pyspark + Spark 运营商+亚马逊 EKS =类固醇大数据！

> 原文：<https://medium.com/nerd-for-tech/pyspark-spark-operator-amazon-eks-big-data-on-steroids-7d1ccedb765b?source=collection_archive---------1----------------------->

![](img/2f24bcee2537f304d4acda1b4ab78749.png)

本文结束时，你将能够:

*   使用 AWS 推荐的工具创建一个 EKS 集群作为代码
*   通过正确使用 Spot 实例降低成本并提高可用性
*   通过在 EKS 集群上安装一些功能强大的附加组件，进一步降低成本并提高可扩展性
*   在 EKS 执行任何星火任务
*   创建一个简单但功能强大的 PySpark 应用程序来读取/写入弹性对象存储，并应用一些很酷的技术来准备和优化您的数据湖。

# 介绍

在这篇文章中，我们将向您展示如何在 kubernetes 集群中安装和运行 Spark 作业，更准确地说，我们将使用亚马逊 EKS 作为我们的 Kubernetes“风味”。

此外，我们将创建一个非常简单的 python 作业( [pyspark](https://spark.apache.org/docs/latest/api/python/) )，它将用于从 [S3 桶](https://aws.amazon.com/s3/?nc1=h_ls)中读取 [JSON](https://www.json.org/json-en.html) 事件，按年/月/日/小时重新划分这些事件，并将它们转换为一个名为 [Apache Parquet](https://parquet.apache.org/) 的优化列格式，最后将这些事件写回到另一个 S3 桶中。

作为我们的火星车，我们将使用 K8s 操作器上的[火星车。例如，我们可以在 EKS](https://github.com/GoogleCloudPlatform/spark-on-k8s-operator) 上使用 [EMR，但是对于本教程，我们选择了 spark 操作符，因为它提供了对 Spark 作业、重启策略、保存执行日志的能力的良好控制，并且是免费的=)。](https://docs.aws.amazon.com/emr/latest/EMR-on-EKS-DevelopmentGuide/emr-eks.html)

在我们动手之前，让我们稍微谈一下这两项令人惊叹的技术(Spark 和亚马逊 EKS)。

[Apache Spark](http://spark.apache.org/) ，通常被称为 MapReduce 的*继承者*，是一个并行分布式数据处理的框架，能够执行并发作业(支持编程语言:Python、Java、Scala、SQL 和 R)，轻松处理海量数据，具有弹性、速度和可扩展性。Spark 主要用于批处理用例(有界数据，如 [ETL](https://en.wikipedia.org/wiki/Extract,_transform,_load) 作业、分析、数据集成等。)，但它也提供了对流用例的支持(无界数据，比如使用来自 Apache Kafka 主题的消息来训练 ML 模型，执行流 ETL 等。).

[亚马逊 EKS](https://aws.amazon.com/eks) 是亚马逊网络服务提供的托管 Kubernetes，它提供了一个完全托管的 Kubernetes 控制平面，具有许多安全和管理功能，还提供了与其他 AWS 服务的轻松集成。亚马逊 EKS 也可以作为开源发行版获得，称为 [EKS 开放发行版](https://aws.amazon.com/blogs/opensource/introducing-amazon-eks-distro/)。

[Helm](https://helm.sh/) 是 [Kubernetes](https://kubernetes.io) 的一个包管理器，提供对 Kubernetes 工作负载的管理，比如共享应用、模板化、执行升级、安装第三方软件等。没有 Helm，我们需要手动创建清单，并通过复制粘贴的方式与其他团队/环境共享，有了 Helm，我们可以拥有不同的存储库，其中包含安全且经过验证的应用程序版本。

以下是跟帖的要求:

*   AWS 账户
*   具有足够权限的有效访问密钥/秘密密钥(对于本教程，管理员权限可以做到这一点，但是对于真实世界的场景，我们强烈建议遵循最小特权原则)。
*   [AWS Cli 已安装](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html)和[已配置](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html)
*   安装了 Eksctl Cli】
*   [已安装的 Kubectl Cli](https://kubernetes.io/docs/tasks/tools/)
*   [舵 Cli 安装完毕](https://helm.sh/docs/intro/install/)

> 不幸的是，亚马逊不提供 EKS 的免费版本，所以请注意，创建一个 EKS 集群将产生你的 AWS 帐户费用！

# 创建 EKS 集群

为了创建我们的集群，我们将使用真正方便的 [eksctl](https://eksctl.io/) 工具，这是提供 EKS 集群的官方 CLI(命令行界面)。

Eksctl 支持创建/配置集群的命令式和声明式方式，因此让我们定义配置文件，该文件将指定新集群的每个重要配置:

sedsed

关于我们的配置文件，需要考虑一些事情/技巧:

*   我们使用的是 EKS 版本 1.18
*   我们的集群和所有必要的资源(VPC、子网、NAT 网关等。)将在美国东部-2(俄亥俄州)地区创建，因为那里的现货保险甚至更便宜。
*   我们使 OIDC 能够将 [IRSA](https://aws.amazon.com/blogs/opensource/introducing-fine-grained-iam-roles-service-accounts/) 与[集群自动缩放器](https://github.com/kubernetes/autoscaler/tree/master/cluster-autoscaler)组件一起使用。集群自动缩放器将使我们的集群能够自动缩放: )
*   因为我们将运行弹性工作负载，所以我们可以使用混合池的 Spot 实例，以便在成本和可用性之间取得良好的平衡。
*   我们相应地标记所有内容，以便[集群自动缩放器](https://github.com/kubernetes/autoscaler/tree/master/cluster-autoscaler)能够根据需要缩放我们的集群。
*   请注意，我们只使用一个可用性区域，但是请等一下，让我解释一下，我们的集群被设计为短暂的，或者换句话说，被创建、运行我们的作业和被销毁，从而降低了在单个可用性区域中运行的风险。通过在一个可用性区域中运行，我们可以避免 az 之间的网络传输成本。此外，我们建议使用 VPC S3 端点，以避免不必要的 NAT 网关流量成本。
*   我们在我们的 worker 节点中安装了 [SSM 代理](https://docs.aws.amazon.com/systems-manager/latest/userguide/ssm-agent.html)，以便能够在没有 SSH 密钥对的情况下访问它们！
*   请注意，我们为我们的工作组附加了一些托管策略，其中一个类似于*arn:aws:iam::XXXXXXXXXXX:policy/back end-analytics policy-XXXXXXX*，显然这是一个假的策略 arn，应该替换为有效策略的 ARN，该策略授予访问我们希望 spark 作业访问的任何 AWS 服务的权限，例如，从/向 AWS S3 读取/写入文件，或者将其用作 [Spark 检查点](https://spark.apache.org/docs/latest/streaming-programming-guide.html#checkpointing)位置。但这是为什么呢？因为这(仍然)是为 EKS 上的 Spark 操作员管理的 Spark 作业提供对 AWS 资源的访问的最简单和安全的方式，因为 Hadoop(由 Spark s3a 文件系统使用)附带 AWS SDK 的捆绑版本，我们不能使用 [IRSA](https://aws.amazon.com/blogs/opensource/introducing-fine-grained-iam-roles-service-accounts/) 来启用 WebIdentity 凭据提供者，并且使用固定访问/秘密密钥从来都不是一个好主意。

要创建我们的集群(我假设您已经检查了前面提到的所有需求)，请执行以下命令:

```
eksctl create cluster -f eks-cluster.yaml
```

如果一切正常，恭喜你！现在，您已经拥有了一个运行良好(且经济高效)的 EKS 集群！

# 安装火花操作器

现在，我们可以使用以下命令在全新的集群中安装 Spark Operator:

```
helm repo add spark-operator [https://googlecloudplatform.github.io/spark-on-k8s-operator](https://googlecloudplatform.github.io/spark-on-k8s-operator)helm repo updatehelm install spark-operator spark-operator/spark-operator --namespace spark-operator --create-namespace --set sparkJobNamespace=default
```

所以在这里我们只是:

*   使用 helm CLI 添加新的公共存储库
*   更新了我们所有本地配置的存储库
*   再次使用 helm CLI 在 Kubernetes 名称空间 **spark-operator** 上安装 spark 操作符图表(认为该图表是一个应用程序，在本例中是 Spark 操作符本身)，使用最新版本的图表(如果我们没有指定版本，这是默认行为)并在名称空间 **default** 上查看我们的 Spark 作业(我们将很快创建)。

几分钟后，运行以下命令检查 Spark Operator 是否安装成功(您应该看到列出的 spark-operator 窗格):

```
kubectl get po -n spark-operator
```

# 安装集群自动缩放器

让我们安装集群自动缩放器，这样我们的集群就可以扩展以满足我们的大数据需求！

就像我们之前做的那样，使用 helm 按照命令安装 CA:

```
helm repo add autoscaler [https://kubernetes.github.io/autoscaler](https://kubernetes.github.io/autoscaler)helm repo updatehelm install cluster-autoscaler autoscaler/cluster-autoscaler --namespace kube-system --set 'autoDiscovery.clusterName'=analytics-k8s --version 1.1.1
```

# 安装并运行 Python Spark 作业

现在我们应该已经准备好安装和运行我们的第一个 Spark 作业了！所以让我们开始吧。

还记得我们提到过让我们的 Spark 作业能够访问 AWS 服务的政策吗？因此，对于我们的示例，假设我们希望我们的 Spark 作业从 S3 存储桶读取文件并将文件发布到另一个 S3 存储桶，让我们假设我们创建了以下 IAM 托管策略并将其附加到我们的 EKS 工作节点(这只是来自 [Cloudformation](https://aws.amazon.com/cloudformation/?nc1=h_ls) 的一个简单片段):

该策略授予对桶 **taxi-events** (我们的源桶)和 **taxi-stage** (我们的目的桶)的完全访问权。

我们的 spark 作业将用于按年、日、月和小时重新划分来自出租车司机的 [JSON](https://www.json.org/json-en.html) 格式的事件。此外，这项工作将转换这个事件，以使用一个更优化的列格式，称为 [Apache Parquet](https://parquet.apache.org/) 。

下面是我们 JSON 事件的一个例子:

我们的 pyspark 重新分区工作的源代码如下所示:

为了让 Spark Operator 可以使用这个作业，我们将把文件复制到一个 S3 存储桶中(对于本教程，我们使用一个假的存储桶，用您的存储库存储桶替换它):

```
aws s3 cp repartition-job.py s3://my-awesome-jobs/
```

以下是我们的作业定义，因此 Spark Operator 可以运行我们的作业:

这个清单几乎是不言自明的，但是这里有一些事情/技巧需要考虑:

*   我们选择了 InstanceProfile 凭据提供程序，因此将使用我们附加的 IAM 托管策略。
*   我们的作业将在集群模式下运行，有 6 个实例，3 个 CPU 和 10 GB 内存。
*   Spark 使用 ivy 缓存下载依赖项(包)时有一些技巧。
*   这项工作将使用 Spark 版本 3.0.1 的预建映像，可在我们的 [Docker Hub](https://hub.docker.com/u/3bittechs) 中获得。如果您想生成自己的图像(推荐)，请遵循本指南。

要提交此作业并查看结果，请执行以下命令:

```
kubectl apply -f repartition-job.yamlkubectl get sparkapplication -w -n default
```

我们的工作需要一段时间才能可用和运行，因为工作节点将不得不提取 Spark Docker 映像，并且集群自动缩放器可能需要横向扩展工作节点的数量来满足需求。

伙计们，在现实世界中，我们会添加监控/日志记录、良好的持续集成管道、安全性和其他一些东西。

# 清理

请不要忘记删除本教程创建的资源！按照命令删除 eksctl 创建的所有资源:

```
eksctl delete cluster -w -f eks-cluster.yaml
```

我们希望你和我们一样喜欢这篇文章，如果你有任何疑问，请随时联系我们！

想了解更多？别忘了去 https://3bit.com.br/[拜访我们](https://3bit.com.br/)