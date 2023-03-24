# SSH 协议工作—授权和加密。

> 原文：<https://medium.com/nerd-for-tech/ssh-protocol-working-authorization-encryption-737ec08f7564?source=collection_archive---------0----------------------->

![](img/b1615b63c02f456df24bf0c2c67fd55e.png)

# 介绍

SSH 是继 SSL 之后最重要的网络加密协议。它允许您安全地连接和访问远程机器的外壳，以便在其上执行命令。ssh 基于客户机-服务器模型工作，其中一台机器上的 ssh 客户机可以连接到另一台机器上的 SSH 服务器。它通过加密所有数据，在两台主机(客户机和服务器)之间创建一个安全的隧道