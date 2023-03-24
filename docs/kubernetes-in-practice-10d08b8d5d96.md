# 实践中的 Kubernetes

> 原文：<https://medium.com/nerd-for-tech/kubernetes-in-practice-10d08b8d5d96?source=collection_archive---------2----------------------->

![](img/270197cdd4bfc1533c36826f092e25b9.png)

# 释文

*   Kubernetes 为我们提供了带有标签的注释特征。
*   注释基本上是对某事进行解释或评论的文字。
*   例如，注释可以用来写创建者的名字或联系方式，或者关于它运行的应用程序。
*   标签意味着保存有限的信息，而我们可以对任何资源进行更大的注释。
*   注释是向资源添加一些额外信息的方法，但它不能像标签一样用于分组或过滤。

# 重叠标签

*   有了标签，很容易对资源进行分组。
*   但是，如果有任何标签重叠，例如
*   type = frontend 分配给几个带有 app_name_web 和 env_development 的 pod
*   type = frontend 分配给几个带有 app_name_web 和 env_development 的 pod
*   特定资源的名称总是唯一的，如何在开发、QA 阶段设置与生产类似的环境。
*   但是，当您想要将对象分割成独立、不重叠的组时，该怎么办呢？我们可能希望一次只在一个组内操作。

# 命名空间

*   [Kubernetes](https://www.technologiesinindustry4.com/2020/09/Docker-Engine-What-is-Kubernetes.html) 也将对象分组到名称空间中。
*   名称空间是一种虚拟盒子，它将自身包含的资源与其他名称空间隔离开来。
*   我们可以很容易地使用名称空间来划分资源的范围。例如，开发阶段的名称空间中的资源不能损害生产阶段的名称空间中的资源。
*   类似地，我们可以将它们分成多个名称空间，这也允许我们多次使用相同的资源名称(跨不同的名称空间)

# 正在创建名称空间

*   Kubectl 创建名称空间产品
*   Kubectl 获取 ns
*   Kubectl 创建 ns 开发
*   Kubectl 获取 ns

# 命名空间内的 Pod

*   nano myfirstpod.yaml
*   Kubectl create -f myfirstpod.yaml
*   ku bectl get pods–名称空间=生产
*   kubectl run NSE example–image = ahmedmansoor/hello world–port = 80–restart = Never–namespace = development
*   Kubectl 获取豆荚
*   Kubectl 获得 pods -n 产品
*   Kubectl get pods -n 开发

**列出所有名称空间的 pod**

*   Kubectl get pods —所有名称空间

# 删除资源

*   Kubectl 删除 pod myfirstpod
*   kubectl get pods——所有名称空间
*   ku bectl delete pod myfirstpod–命名空间=生产
*   Kubectl 删除 ns 开发
*   kubectl 获取 ns
*   Kubectl get pod —显示-标签
*   Kubectl 删除 pod-l type = back end
*   Kubectl 删除窗格–全部

# 副本集

*   副本集也是 Kubernetes 中的资源之一，就像 pods 和其他资源一样。它通常用于保证指定数量的相同 pod 的可用性。
*   副本集是帮助在 Kubernetes 中创建和管理应用程序(副本)的多个副本的资源。
*   当我们通过副本集创建任何 pod 时，它确保其 pod 始终保持运行。
*   如果 pod 因任何原因移除，例如
*   如果工作者节点从集群中消失
*   如果 pod 被逐出工作者节点
*   副本集记录丢失的 pod 并创建一个替换 pod。

更多详情请访问:[https://www . technologiesinindustry 4 . com/2020/10/kubernetes-in-practice . html](https://www.technologiesinindustry4.com/2020/10/kubernetes-in-practice.html)