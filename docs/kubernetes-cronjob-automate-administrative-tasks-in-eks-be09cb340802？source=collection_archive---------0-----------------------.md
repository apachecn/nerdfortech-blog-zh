# Kubernetes CronJob:自动化 EKS 的管理任务

> 原文：<https://medium.com/nerd-for-tech/kubernetes-cronjob-automate-administrative-tasks-in-eks-be09cb340802?source=collection_archive---------0----------------------->

## 使用 Kubernetes Cron job 在 EKS 自动化管理任务的简单用例

![](img/ec54022f9c4a85e4d4b651a83cbbe830.png)

劳拉·奥克尔在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

Kubernetes 是使用最广泛的开源容器编排平台之一。对于大多数 DevOps 工程师来说，在 Unix 中实现任务自动化的简单、快速和方便的方法是使用 Unix crontab 安排一个脚本。

类似地，Kubernetes 提供了一种按照固定时间表安排任务的本地方式。这篇博客解释了一个使用 Kubernetes Cron 作业自动化管理任务的简单用例。

## **需求:**

我们在库伯内特斯(EKS)运行阿帕奇气流。在开发环境中，每个开发人员都被提供了一个专用的 Airflow 实例，作为 Kubernetes 部署运行，在专用的 Kubernetes 名称空间中有一个专用的调度程序和 Web 服务器。这些部署在周末(非工作日)将处于非活动状态，其中一些部署并不经常使用。

我们不希望在周末为调度程序和 Web 服务器运行任何不做任何事情(不主动调度作业)的 pod，这将在周末为我们节省大量计算成本，因为中型团队中的每个开发人员都有专门的气流部署。应该有一种简单的方法在周末禁用/休眠气流部署，并在用户需要时重新启用它们。

## **解决方案:**

解决方案是在周末将所有气流部署缩减至 0 个副本。

```
kubectl scale deployment -n my-airflow-namespace --all --replicas=0
```

在工作日，如果用户需要恢复气流部署，他们只需使用副本 1(副本的原始值)来扩展部署。

```
kubectl scale deployment -n my-airflow-namespace --all --replicas=1
```

通过这种方式，周末不会运行任何 pod，并且所有不常用的气流部署不会运行任何不必要的 pod，直到用户按需需要它。

现在，为了在周末安排这个任务，我们可以使用一个 Kubernetes Cron 作业来执行这个管理任务。

**实现:**

*   创建了一个安装了 kubectl 的 Docker 映像，以便映像中的脚本可以在 kubectl 命令的帮助下与 AWS EKS 集群进行交互。

*   创建了一个 Kubernetes 名称空间，自动化 Cronjob 将在其中运行。通过 Kubernetes RBAC，对该名称空间的访问仅限于管理员用户。
*   创建了一个附加到 AWS IAM 角色的 Kubernetes 服务帐户，该帐户为 cron 作业提供必要的访问权限，以便在 EKS 集群上运行必要的 kubectl 命令。参考:[EKS 服务帐户的 IAM 角色](https://docs.aws.amazon.com/eks/latest/userguide/iam-roles-for-service-accounts.html)
*   创建了一个 Kubernetes cronjob，计划在 UTC 时间每周六上午 7 点运行。下面是 cron 作业的定义。参考:[创建 Kubernetes Cron 作业](https://kubernetes.io/docs/concepts/workloads/controllers/cron-jobs/)

这只是一个使用 Kubernetes Cron job 在 EKS 实现行政自动化的简单例子。同样，我们可以用 Kubernetes ConJob 自动化任何任务。

自动化快乐！