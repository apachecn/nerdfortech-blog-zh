# 使用 Azure DevOps 部署打包虚拟机映像&如何引用 PowerShell 脚本

> 原文：<https://medium.com/nerd-for-tech/deploy-packer-vm-images-with-azure-devops-how-to-reference-powershell-scripts-d3bca8c0f996?source=collection_archive---------8----------------------->

当尝试使用 Azure DevOps 管道部署打包映像时，您需要稍微修改配置以适应 PowerShell 脚本的位置。

在本地运行时，Packer 将能够访问本地目录中与 Packer 配置文件相关的所有内容。没问题！一个简单的`Packer build .`就可以了！如果你想了解如何在 Azure 上构建一个虚拟机映像并在本地运行 packer，请查看这篇文章。