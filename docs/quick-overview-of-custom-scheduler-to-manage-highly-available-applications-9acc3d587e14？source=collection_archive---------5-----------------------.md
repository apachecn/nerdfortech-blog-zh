# 管理高可用性应用程序的定制调度程序的最佳概述

> 原文：<https://medium.com/nerd-for-tech/quick-overview-of-custom-scheduler-to-manage-highly-available-applications-9acc3d587e14?source=collection_archive---------5----------------------->

***TL:DR
Kubernetes 有几个用于管理应用程序的工具，但定制调度程序是为了管理具有强大集群和用户群的大型应用程序而构建的。最重要的是，它在新创建节点时将 pod 分配给节点，并确保应用程序在没有任何停机时间的情况下保持运行，并且所有资源预期都得到处理。***

![](img/2714e37384e91dcf73c9c440f48ba18f.png)

**自定义调度程序高亮显示**
1。调度程序负责将持久卷映射到集群中先前已调度的 pod。

2.与调度程序一起工作，它有一个名为 Kube-scheduler 的插件，这是控制平面的重要组成部分，也是调度程序如何运行的核心。

3.调度器的主要目的是将 pod 分配给等级最高的节点，插件使这成为可能。

4.定制的 Kubernetes 调度程序有开源插件，可以在所有的 Kubernetes CockroachCloud 集群中运行。

5.在运行高流量应用程序时，定制 Kubernetes 调度程序可以确保由容器管理的资源得到实时处理，从而优化 Kubernetes 操作。

6.自定义 Kubernetes 调度程序依赖于一些行为；策略和配置文件，因为它们有助于正确配置调度程序的功能。

> *根据定义，定制 Kubernetes 调度程序旨在监视系统中的新 pod，并确保它们被分配到可以运行的最佳节点。它的操作大部分是由插件完成的，它对节点的决定受到为时间表划分的策略和配置文件的影响。*

定制 Kubernetes 调度程序的操作通过两个主要操作获得了成功；**过滤评分**。虽然**过滤**涉及查看可用节点并确认其满足新创建的待调度的 pod 的要求的能力，但是**评分**是其采取的下一步，并且其涉及将过滤过程产生的分数分配给每个节点，然后 pod 将被分配给具有最高分数的节点。在一个或多个节点之间的分数存在某种形式的平局的情况下，随机选择获胜的节点。

此外，基于策略和配置文件配置调度程序的过滤和评分功能，策略和配置文件分别涵盖评分和插件配置阶段的优先级。其中一些阶段包括评分、绑定、过滤和许多其他阶段。此外，可以调整调度程序的性能以适应业务需求，尤其是对于相对较大的集群，调度程序的行为在延迟和准确性之间取得了平衡，这涉及如何快速放置 pod 以及确保调度程序不会做出糟糕的决策。此外，在尝试测试调度程序时，有为此准备的文档。

# 用法—更进一步

Kelsey Hightower 使用定制的调度程序，创建了一个有趣的回购协议，我们可以使用它作为参考。你可以在这里 看一下这个回购 [**中的调度器代码。使用注释器命令注释每个节点:**](https://kubernetes.io/docs/tasks/extend-kubernetes/configure-multiple-schedulers/)

```
kubectl proxyStarting to serve on 127.0.0.1:8001go run annotator/main.gogke-k0-default-pool-728d327f-00lq 1.60
gke-k0-default-pool-728d327f-3vzg 0.20
gke-k0-default-pool-728d327f-nmz7 0.80
gke-k0-default-pool-728d327f-pxee 0.05
gke-k0-default-pool-728d327f-xm4i 0.05
gke-k0-default-pool-728d327f-zynj 0.20
```

# 创建部署

```
kubectl create -f deployments/nginx.yamldeployment "nginx" created
```

nginx pod 应该处于挂起状态:

```
kubectl get podsNAME                     READY     STATUS    RESTARTS   AGE
nginx-1431970305-mwghf   0/1       Pending   0          27s
```

# 运行调度程序

列出节点并记下每个节点的价格。

```
annotator -lgke-k0-default-pool-728d327f-00lq 0.80
gke-k0-default-pool-728d327f-3vzg 0.40
gke-k0-default-pool-728d327f-nmz7 0.40
gke-k0-default-pool-728d327f-pxee 0.05
gke-k0-default-pool-728d327f-xm4i 1.60
gke-k0-default-pool-728d327f-zynj 0.40
```

运行最佳价格计划程序:

```
scheduler2016/08/19 11:16:25 Starting custom scheduler...
2016/08/19 11:16:28 Successfully assigned nginx-1431970305-mwghf to gke-k0-default-pool-728d327f-pxee
2016/08/19 11:16:35 Shutdown signal received, exiting...
2016/08/19 11:16:35 Stopped reconciliation loop.
2016/08/19 11:16:35 Stopped scheduler.
```

> *请注意，挂起的 nginx pod 部署到了成本最低的节点上。*

# 在 Kubernetes 上运行调度程序

```
kubectl create -f deployments/scheduler.yamldeployment "scheduler" created
```

现在，请记住，这些过程中的许多都可以使用 CI/CD 毫无困难地自动化，只要您的管道能够协调该过程，任何工具都适合该工作。在这一点上，任何形式的系统变化都可以立即展开。你可以在这个[博客](https://blog.usejournal.com/aws-codebuild-vs-github-actions-vs-circleci-3fe20a182013?source=your_stories_page-------------------------------------)上找到我对 CI/CD 的偏好。最后，您应该能够访问原始的[文档](https://kubernetes.io/docs/tasks/extend-kubernetes/configure-multiple-schedulers/)，并观察调度程序的一些广泛应用。

总之，定制的 Kubernetes 调度程序用于编排和管理高使用率的应用程序，并确保在 pods 被正确分配和运行时没有停机时间，除了配置调度程序独立运行之外，没有来自开发人员的任何输入。现在，请记住，这篇文章不仅仅是为软件领域的专家准备的，即使是新手也可以加入并学到很多东西，这就是为什么我试图用外行和专业的术语把一切都讲清楚，所以如果你有任何问题，可以通过 [**Twitter**](https://twitter.com/SamuelArogbonlo) **联系我，或者通过**[**GitHub**](https://github.com/samuelarogbonlo)**找到我。**

**感谢阅读❤️**

如果你对这个话题有任何想法，请留下评论——我乐于学习和探索知识。

# 我可以想象这个帖子有多有用，请留下掌声👏下面几次以示对作者的支持！