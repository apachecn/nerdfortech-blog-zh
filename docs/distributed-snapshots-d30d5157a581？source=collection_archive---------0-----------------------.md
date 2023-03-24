# 分布式快照

> 原文：<https://medium.com/nerd-for-tech/distributed-snapshots-d30d5157a581?source=collection_archive---------0----------------------->

通过我上一个关于[时钟](https://distributedsystemsmadeeasy.medium.com/clocks-its-not-that-simple-6fa9f59f5dbc) & [版本向量](https://distributedsystemsmadeeasy.medium.com/version-vectors-a9a69e4c34f0)的系列，我的想法是讨论在分布式系统中建立顺序/因果关系的问题以及实现这一点的方法。现实世界中需要这两个概念的一个问题是分布式快照！

## 问题陈述

时间点快照对于捕获系统的“一致”状态至关重要，在系统状态丢失的情况下可以恢复，从而使您的系统具有容错能力。

拍摄特定服务器的快照很容易。你定义一个截止时间，在…