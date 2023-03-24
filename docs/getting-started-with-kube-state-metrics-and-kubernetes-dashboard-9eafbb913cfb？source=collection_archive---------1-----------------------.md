# Kube-State-Metrics 和 Kubernetes 仪表板入门

> 原文：<https://medium.com/nerd-for-tech/getting-started-with-kube-state-metrics-and-kubernetes-dashboard-9eafbb913cfb?source=collection_archive---------1----------------------->

## 库伯内特斯咖啡馆

![](img/76d7a7f0cd65e8e75981945a57345190.png)

卢克·切瑟在 Unsplash[上的照片](https://unsplash.com?utm_source=medium&utm_medium=referral)

# Kube 状态度量简介

`kube-state-metrics`是一个开源项目，用于生成关于 Kubernetes 集群对象状态的指标。这是一个监听 Kubernetes API 服务器的服务。它不对 Kubernetes API 进行任何修改，只是读取…