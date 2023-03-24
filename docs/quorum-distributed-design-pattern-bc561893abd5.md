# 仲裁—分布式设计模式

> 原文：<https://medium.com/nerd-for-tech/quorum-distributed-design-pattern-bc561893abd5?source=collection_archive---------5----------------------->

![](img/a29f79f7dd8e3e21cf1b70d79ba16ab0.png)

法定人数

任何分布式系统都有一个不变的事实，那就是失败。我们构建的系统能够抵御失败。让我们继续我们之前关于墙的讨论，为了确保持久性，我们的操作被存储在墙上，从那里它们可以被恢复。假设我们想要将 WAL 复制到集群中的不同节点，以实现高可用性和容错。我们需要问的下一个问题是-