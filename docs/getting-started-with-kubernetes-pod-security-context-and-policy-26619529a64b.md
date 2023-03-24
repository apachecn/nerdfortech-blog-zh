# Kubernetes Pod 安全上下文和策略入门

> 原文：<https://medium.com/nerd-for-tech/getting-started-with-kubernetes-pod-security-context-and-policy-26619529a64b?source=collection_archive---------7----------------------->

## 库伯内特斯咖啡馆

![](img/e1b48d316d8989dd5fb9c3458046ad3c.png)

斯科特·韦伯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Kubernetes 最近成为了容器部署的事实上的标准。这使得容器安全性成为 Kubernetes 领域中的一个关键组件。Kubernetes 集群上运行的每个容器都可能有不同的攻击面和漏洞，攻击者可以利用这些攻击面和漏洞。为了处理 Kubernetes 也来了…