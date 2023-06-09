# 用 Kubernetes 抓住他们:口袋妖怪 Go 案例研究

> 原文：<https://medium.com/nerd-for-tech/catch-em-all-with-kubernetes-pokemon-go-case-study-37a43c5dce1b?source=collection_archive---------3----------------------->

![](img/1acb4d3c0bddf2e0d2ef7e19f7b4df06.png)

对于这里所有的千禧一代来说，口袋妖怪从小就在我们的心中。由于口袋妖怪是由任天堂公司制作的，口袋妖怪游戏只在任天堂 switch 上提供。任天堂是掌机之王。但目前，Niantics 与任天堂合作发布了一款适用于 Android 和 iOS 设备的口袋妖怪游戏。POKEMON 去吧。这是第一个在手持设备以外的设备上玩的口袋妖怪游戏。

![](img/741db62f2f3762324d34d41d1cc489e3.png)

POKEMON 围棋

这款手机游戏还没上市就在市场上引起了轩然大波。游戏一经推出，对该公司的评价就大大提高了。毕竟是口袋妖怪。在澳大利亚和新西兰推出后的 15 分钟内，玩家流量飙升，远远超出了 Niantic 的预期。这对 Niantic 的产品和工程团队来说是第一次表明他们手上有真正特别的东西。

![](img/21d6841a42d0cac01f6d42084f5f8229.png)

《口袋妖怪 Go》超出预期

Pokemon Go 是通过谷歌云推出的。球队的目标是 1 倍的球员流量，最坏的情况估计大约是 5X 这个目标。Pokémon GO 的受欢迎程度迅速将玩家流量飙升至最初目标的 50 倍，是最坏情况估计的 10 倍。在全球发行的短短几周内，这款游戏在**的下载量就达到了 5 亿多次**，在**平均每天有 2000 多万活跃用户**。作为回应，Google CRE 代表 Niantic 无缝地提供了额外的容量，以远远领先于他们创纪录的增长。

# 他们是如何在如此短的时间内实现横向扩展的？

这个问题的答案是集装箱化。Pokemon Go 是一款基于容器的应用。游戏的应用逻辑运行在由开源软件 Kubernetes 项目驱动的谷歌容器引擎(GKE)上。

![](img/9314861921e7153b3cd71c8359aa33d8.png)

码头工人

**容器:**容器是一个标准的软件单元，它封装了代码及其所有依赖项，因此应用程序可以快速可靠地从一个计算环境运行到另一个计算环境。这意味着我们可以通过运行这些容器来运行应用程序，它们有自己的依赖关系，并且是隔离的，因此容器之间的冲突不会产生错误。此外，容器是轻量级的，只是为了运行一些进程而创建的，因此我们可以在几秒钟内提供和终止容器。尽管隔离是最好的方法，但是我们需要多个容器相互通信，因为单个容器只有一个进程。例如，带有 web 应用程序的容器需要与带有数据库的容器通信。这里，我们需要一个容器管理服务

![](img/90a5385d58e59efd635efe02d5fdb6cc.png)

库伯内特斯

Kubernetes: Kubernetes 是一个开源的容器编排系统，用于自动化计算机应用程序的部署、扩展和管理。它最初是由谷歌设计的，现在由云原生计算基金会维护。Kubernetes 是目前容器编排的最佳工具。Kubernetes 还提供角色和 IAM 等功能，以及适合我们需求的各种网络解决方案。

Kubernetes 服务由谷歌云提供，即谷歌 Kubernetes 引擎。Google Kubernetes Engine(**GKE**)为使用 Google 基础设施部署、管理和扩展您的容器化应用程序提供了一个托管环境。 **GKE** 环境由组合在一起形成一个集群的多台机器(计算引擎实例)组成。Pokemon Go 使用这项服务和十几项服务。Pokémon GO 在谷歌云上使用了十几项服务。事实上，这是 Kubernetes 在 Google Container Engine 上最大的一次部署。

Kubernetes 部署用于告诉 Kubernetes 如何创建或修改包含容器化应用程序的 pod 实例。**部署**可以扩展副本单元的数量，以可控的方式部署更新的代码，或者在必要时回滚到更早的**部署**版本。

部署人员会密切关注吊舱，如果任何吊舱因任何故障而关闭，它会尝试让吊舱重新联机。这意味着我们不必一直监视运行应用程序的每个节点上的每个容器。在 Kubernetes 的帮助下，Niantics 和 GoogleCloud 能够横向扩展节点，从而扩展 pod 的数量，进而扩展容器的数量。部署等资源有助于非常轻松地自动化和管理应用程序群集，从而减少停机时间。

这就是 Niantics 和 Google Cloud 能够管理如此巨大的游戏流量的原因。

谢谢大家！

# vimaldaga # right education # education redefine # right mentor # world recordholder # Linux world # makingindiafutureready # righ education # arthbylw # ansibleglaxy # rightansibleconcepts # k8s #用例#自动化# kubernetes #微服务