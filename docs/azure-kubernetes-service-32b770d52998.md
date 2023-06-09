# 蓝色库伯内特服务

> 原文：<https://medium.com/nerd-for-tech/azure-kubernetes-service-32b770d52998?source=collection_archive---------28----------------------->

# 关于 Kubernetes

Kubernetes 是一个可移植、可扩展的开源平台，用于管理容器化的工作负载和服务，有助于声明式配置和自动化。它有一个庞大的、快速增长的生态系统。Kubernetes 的服务、支持和工具随处可见。

Kubernetes 为您提供了一个灵活运行分布式系统的框架。它负责应用程序的伸缩和故障转移，提供部署模式等等。

Kubernetes 提供的功能:

*   **服务发现和负载平衡** Kubernetes 可以使用 DNS 名称或自己的 IP 地址公开容器。如果容器的流量很高，Kubernetes 能够对网络流量进行负载平衡和分配，从而使部署稳定。
*   **存储协调** Kubernetes 允许您自动挂载自己选择的存储系统，例如本地存储、公共云提供商等等。
*   **自动化部署和回滚**您可以使用 Kubernetes 描述您部署的容器的期望状态，它可以以可控的速度将实际状态更改为期望状态。
*   你为 Kubernetes 提供了一个节点集群，它可以使用这个集群来运行容器化的任务。您告诉 Kubernetes 每个容器需要多少 CPU 和内存(RAM)。Kubernetes 可以在节点上安装容器，以充分利用资源。
*   **自我修复** Kubernetes 重新启动失败的容器，替换容器，终止对用户定义的健康检查没有响应的容器，并且在它们准备好服务之前不向客户端通告它们。
*   **秘密和配置管理** Kubernetes 允许您存储和管理敏感信息，比如密码、OAuth 令牌和 SSH 密钥。您可以部署和更新机密和应用程序配置，而无需重新构建容器映像，也无需在堆栈配置中暴露机密

然而， *Kubernetes 并不是一个传统的包罗万象的 PaaS* (平台即服务)系统。Kubernetes 在容器级别而不是硬件级别运行，它提供了一些 PaaS 产品通用的普遍适用的功能，如部署、扩展、负载平衡，并允许用户集成他们的日志记录、监控和警报解决方案。因此，Kubernetes 不是单一的，这些默认解决方案是可选的和可插拔的。

# 关于微软 Azure

微软 Azure 是面向中小型企业和大规模企业的世界知名云平台。Azure 是唯一一致的混合云，提供无与伦比的开发人员生产力，提供全面、多层的安全性，包括任何云提供商中最大的合规性覆盖范围，并且您将为 Azure 支付更少的费用，因为 AWS 比用于 Windows Server 和 SQL Server 的 Azure 贵五倍。

由于 Kubernetes 是一种现代方法，正在迅速成为在生产环境中管理云原生应用程序的常规方法，Azure 提供了一种称为 Azure Kubernetes Service (AKS)的服务，以更轻松地部署和管理容器化的应用程序。

# 关于 Azure Kubernetes 服务

![](img/6e44cc1cdfd9472556fcda4bdd77a50a.png)

微软 AKS

Azure Kubernetes Service (AKS)是一个高度可用、安全且完全托管的 Kubernetes 服务。它是一种开源的容器编排服务，于 2018 年 6 月推出，在微软 Azure 公共云上提供，可用于在集群环境中部署、扩展和管理 Docker 容器和基于容器的应用程序。

> AKS 最好的一点是，管理 AKS 不需要容器编排方面的高深知识和专业技能。

AKS 提供无服务器 Kubernetes、集成式持续集成和持续交付(CI/CD)体验以及企业级安全性和治理。在单一平台上整合您的开发和运营团队，满怀信心地快速构建、交付和扩展应用。

# AKS 客户案例

1.  Hafslund 将集装箱软件用于公用事业计划和改善客户服务

![](img/d6e1f8dbf86899ae0f768179ba852d0b.png)

为 150 万挪威人提供服务的电网运营商 Hafslund Nett (Hafslund)认为，用于读取电表数据的传统系统需要更高的容量，并且外部开发的软件难以管理。为了解决这个问题，Hafslund 选择开发自己的计量系统软件，使用 Microsoft Azure 作为其云平台，使用 Azure Kubernetes Service (AKS)管理软件容器，使用 Azure Monitor for containers 优化容器性能。Hafslund 的 IT 人员将很快节省管理其改进系统的时间，客户也将从更高的可靠性中受益。

> *“我们需要一个平台来加速开发和测试，但要安全地进行，不失去对安全性和性能的控制。这就是 Azure 和 AKS 最适合我们的原因。”*
> 
> stle heit Mann，Hafslund Nett 首席技术官

根据其他公司部门的大型项目经验，Hafslund 认识到了容器化应用程序的价值以及它们提供的规模效率。通过与微软合作伙伴网络成员 Aurum 和 Computas 合作，Hafslund 求助于[微软 Azure](https://azure.microsoft.com/en-us/) 和[Azure Kubernetes Service(AKS)](https://azure.microsoft.com/en-us/services/kubernetes-service/)来创建自己的智能电表软件，该软件由高效、行业认可的工具提供支持。

2.**博世**利用地图匹配算法和 Azure Kubernetes 服务提高车辆安全性

![](img/98e2207075cd149dcf3a6a0cdc68ff0c.png)

当罗伯特·博世公司(Robert Bosch GmbH)着手解决司机在高速公路上逆行的问题时，目标是拯救生命。类似的服务在德国也有，但是精度和速度不能妥协。博世能实时获得足够精确的位置数据来做到这一点吗？该公司知道它必须尝试。

结果是错误方向驾驶员警告(WDW)服务和软件开发套件(SDK)。该架构专为应用程序开发人员和原始设备制造商(OEM)设计，以创新的地图匹配算法和微软 Azure Kubernetes 服务(AKS)的可扩展性为基础，结合与 Apache Kafka 流媒体平台集成的 Azure HDInsight 工具。

> *“我们需要一个平台来加速开发和测试，但要安全地进行，同时不失去对安全性和性能的控制。这就是 Azure 和 AKS 最适合我们的原因。”*
> 
> stle heit Mann，Hafslund Nett 首席技术官

3. **Finastra** 选择 AKS 作为其下一代金融技术开发生态系统

![](img/595c8d3b667317f9517147afba25ccf3.png)

Finastra 在 60 多个国家设有办事处，拥有 10，000 名员工，收入超过 20 亿美元，是一支重要的金融科技力量。已经是金融软件和云解决方案领域的公认领导者，其首个平台产品 FusionFabric.cloud 于 2018 年 6 月发布到公共云。

> *“AKS 给了我们一个纯粹的 Kubernetes 和 Docker 成像环境，我们不用自己管理。我们的团队重新获得了加速部署和最大化我们的 PaaS 产品的资源。”*
> 
> Félix Grévy，Finastra 全球产品管理总监

Kubernetes 是 FusionFabric.cloud 平台的核心，允许编排 Docker 容器。Fintech 应用可以在 Azure Kubernetes 服务(AKS)上轻松运行和扩展，这是基于 Azure 容器服务引擎(ACS)的下一代服务。Finastra 目前使用 ACS 引擎，计划迁移到 AKS。

4. **Siemens Healthineers** 将更多的计算转移到云中，以支持基于价值的护理发展

![](img/8b898df3f8e9560783dcfaf866894e0d.png)

Siemens Healthineers 通过其数字生态系统引领医疗保健的数字化，帮助医疗保健提供商和解决方案开发商为医疗保健服务带来更多价值，最终提高从医疗保健数据中获得的洞察力的质量。Siemens Healthineers 使用 Microsoft Azure 使解决方案更易于访问，并使用 Azure Kubernetes Service (AKS)和其他工具实现快速、高效和有竞争力的开发渠道。

> 服务使我们不仅能够在 Docker 容器中部署我们的业务逻辑(包括编排),还能够轻松管理公开、控制和计量访问。
> Thomas Gossler:数字生态系统平台首席架构师
> 
> -西门子医疗保健公司

*资料来源:azure.microsoft.com*