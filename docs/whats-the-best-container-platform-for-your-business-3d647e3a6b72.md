# 对您的企业来说，最好的容器平台是什么？

> 原文：<https://medium.com/nerd-for-tech/whats-the-best-container-platform-for-your-business-3d647e3a6b72?source=collection_archive---------11----------------------->

## 落后于容器平台意味着存在于更加敏捷的竞争的阴影中。

![](img/59c5fc2ae7634e4a87f4d43de542923f.png)

图片由 iStock 提供

如果您是 IT 管理员或开发人员，那么您很可能听说过容器。事实上，在过去几年中，容器已经席卷了企业业务。此外，根据 Gartner 的数据，到 2024 年，全球集装箱管理收入将增长至 9 . 44 亿美元。在同一份报告中，Gartner 预测“到 2022 年，超过 75%的全球组织将在生产中运行容器化应用程序，而目前这一比例不到 30%。”

这很重要。因此，如果您的公司还没有使用容器，那么是时候考虑这个选择了。

# 什么是容器？

简而言之，容器是一种打包应用程序及其所有依赖项的方式，因此应用程序(或服务)可以快速部署在任何支持平台上。容器可以从一台机器迁移到另一台机器，并且仍然完全按照预期运行。

容器的主要优点是:

*   它们是轻量级应用程序，使用的资源比标准部署少得多。
*   它们是可移植的，因此几乎可以部署在任何平台上。
*   他们可以很容易地扩大和缩小，以满足需求。
*   他们可以帮助您的业务更加敏捷。
*   他们享受更快的启动时间。
*   它们比标准应用程序更容易管理。
*   它们比标准应用程序更具成本效益。

容器的流行使得任何开发人员或外包软件开发团队都应该非常有能力使用这项技术。我们应该知道——在 [BairesDev](https://www.bairesdev.com/software-development-services/software-outsourcing/) ，我们越来越多地使用容器来更好地满足客户需求，因为我们知道它们是一种现代标准。

尽管我们已经说过容器可以部署在任何平台上，但是有一点需要注意:这个平台必须包括所谓的容器引擎。容器引擎是一个允许用户使用容器的软件。这些引擎包括用于下拉图像(容器将基于这些图像)和管理容器部署的终端命令。

您可以使用许多不同的容器引擎:

*   [Docker](https://www.docker.com/) 是首批容器引擎之一，与一些流行的容器编排器合作。对 Docker 的一个警告是，它不再是最流行的容器编排器 Kubernetes 的默认引擎。
*   [CRI-O](https://cri-o.io/) 是容器运行时接口的初始实现。这个引擎是轻量级和开源的。
*   [containerd](https://containerd.io/) ，一个[云本地计算基金会](https://www.cncf.io/)的项目，正迅速成为 Kubernetes orchestrator 最受欢迎的引擎之一。

Docker 被 Kubernetes 淘汰是有原因的。为了达到容器部署所需的扩展水平，Hyper、CoreOS、Google 和 Kubernetes orchestrator 的其他赞助商一起为容器运行时创建了一个高级规范。这就是众所周知的容器运行时接口(CRI)。为了让 Docker 与 CRI 一起工作，必须创建一个 shim(称为 dockershim)。

然而，最终，containerd 从 Docker 核心中分离出来，包含了 Docker 的所有必要功能，这样就不需要 shim 了。这意味着 Kubernetes 可能会反对 dockershim，这(实际上)意味着 Docker 支持也会被反对。

在这一点上，你可能想知道为什么 Kubernetes 有这么多要说的。让我们来回答这个问题。

# 什么是容器编排器？

一个集装箱对你的生意没什么好处。当然，你可以通过一个容器来部署一个应用或服务，但你不会有太多的运气来扩展它。为什么？因为扩展需要部署多个克隆容器来处理增加的负载。

但是，如果您只需要在您的网络中部署一个快速的应用程序或服务，并且不需要它进行扩展，您可以使用 Docker 这样的工具来快速轻松地部署它。事实上，到目前为止，Docker 容器引擎是部署单个容器最简单的方法。

但是，当您需要该容器处理更大的负载，或者您需要它与其他容器协同工作时，您将需要一个 orchestrator，它可以管理一个服务器集群，并根据需要将容器部署到这些服务器上。

就管弦乐队而言，你不会找到比 Kubernetes 更合适、更有能力的工具了。这个解决方案是由 Google 开发的，是迄今为止世界上最流行的容器编排器。

除了是最受欢迎和最强大的容器编排器之一(能够处理您的伸缩、负载平衡和故障转移需求)，Kubernetes 还受到 Amazon AWS、Google Cloud Engine 和 Azure 的支持。或者，如果您有胜任这项任务的开发人员和管理员，您可以随时在您的本地数据中心安装 Kubernetes。

Kubernetes 的优势包括:

*   容器的自动部署。
*   容器回滚。
*   容器的隔离。
*   监控服务健康的能力。
*   服务发现。
*   负载平衡。
*   支持 CI/CD。

大多数主要的云提供商都有自己的 Kubernetes 版本，例如:

*   [亚马逊弹性 Kubernetes 服务](https://aws.amazon.com/eks/)
*   [蔚蓝库伯内特服务](https://azure.microsoft.com/en-us/services/kubernetes-service/)
*   [IBM Cloud Kubernetes 服务](https://www.ibm.com/cloud/kubernetes-service)

[Amazon 弹性容器服务](https://aws.amazon.com/ecs/)是一个管理 Amazon EC2 实例集群的容器编制器。这个平台实际上为亚马逊的许多服务提供了动力(例如亚马逊推荐引擎)。

ECS 的主要功能包括:

*   易于部署和管理 Docker 容器。
*   与许多 AWS 服务集成。
*   支持第三方图像库。
*   通过亚马逊 VPC 支持 Docker 网络。

[Azure Service Fabric](https://azure.microsoft.com/en-us/services/service-fabric/) 是一个分布式服务框架，它使管理容器成为可能，并且可以部署在 Azure 云或内部。ASF 的功能包括:

*   自动升级。
*   容器的自我修复。
*   自动缩放。
*   多语言框架支持。
*   低延迟。
*   支持有状态和无状态服务。
*   支持 CI/CD。

[Docker Swarm](https://docs.docker.com/engine/swarm/) 使服务器集群化成为可能，因此您可以从单一入口点控制 Docker 容器的部署和扩展。虽然 Docker Swarm 可能无法为您的企业提供上述任何工具中的扩展和控制水平，但它的部署和管理要容易得多。如果你的开发人员和管理员已经熟悉了 Docker 和 Docker Compose 命令，他们使用 Docker Swarm 不会有任何问题。

# 结论

如果您计划为您的公司外包软件开发，并且您希望应用程序和服务能够扩展以满足您不断增长的需求，那么您应该认真考虑采用这些容器平台之一。如果你在这方面落后了，你会发现你的企业存在于你更敏捷的竞争的阴影中。