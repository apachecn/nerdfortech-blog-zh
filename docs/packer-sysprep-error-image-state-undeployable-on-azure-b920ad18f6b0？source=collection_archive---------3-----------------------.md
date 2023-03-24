# Packer / Sysprep 错误-IMAGE _ STATE _ undepable on Azure

> 原文：<https://medium.com/nerd-for-tech/packer-sysprep-error-image-state-undeployable-on-azure-b920ad18f6b0?source=collection_archive---------3----------------------->

![](img/feeb1479a1b886ad267c0d6406a2e36e.png)

# 问题是

打包程序构建过程的一部分是概括准备部署的映像。

在撰写本文时(2021 年 8 月)，遵循来自 [Packer docs](https://www.packer.io/docs/builders/azure/arm) 的指导，它说您应该添加以下配置来实现这一点(我使用的是 HCL，而不是 JSON):

```
# Generalize image
```