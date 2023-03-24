# Docker 映像最佳实践—最少映像

> 原文：<https://medium.com/nerd-for-tech/create-your-own-minimal-docker-base-image-55db9f37a419?source=collection_archive---------0----------------------->

Dockerfile 的最佳实践是**保持图像最小化**。创建一个分布式映像，避免包含不必要的包或暴露端口，以减少攻击面。

# 分布式图像

这篇文章包含了如何建立一个最小的 Docker 镜像的说明，这个镜像只有一个`tmp`文件夹，它是一个“分布式镜像”

[**dis loses**](https://github.com/GoogleContainerTools/distroless)images 只包含你的应用程序及其运行时的依赖项。它们不包含包管理器、shells 或任何其他您期望的程序…