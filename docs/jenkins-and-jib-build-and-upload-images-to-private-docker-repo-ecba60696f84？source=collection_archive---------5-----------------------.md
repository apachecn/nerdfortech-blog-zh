# Jenkins 和 Jib |构建图像并上传到私人 Docker Repo

> 原文：<https://medium.com/nerd-for-tech/jenkins-and-jib-build-and-upload-images-to-private-docker-repo-ecba60696f84?source=collection_archive---------5----------------------->

![](img/9d44db4ea417c25aa78319bfd1378a1a.png)

照片由 [JJ 英](https://unsplash.com/@jjying?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

几天前，我需要从头开始创建一个没有任何模板的新项目，该项目应该能够将 Micronaut 用作无服务器框架。我的申请是用科特林语写的。最后，它应该像网络服务器一样工作，部署在 Kubernetes 上。首先，有三个关键组件。第一个是应用程序本身。此应用程序需要转换为 docker 图像。下一部分是管道。它需要运行一些自动化测试，如果测试成功，就构建整个映像。它还应该将图像推送到 artifactory 或任何其他私有 docker repo。最后一部分是 Kubernetes 集群，它部署了舵图。在本教程中，我将只看前两步。稍后我将创建一篇关于此主题的文章。

# 从应用程序创建 docker 图像

首先，我们需要有一个经过测试并正在运行的应用程序。下一步是将 [google jib](https://github.com/GoogleContainerTools/jib/tree/master/jib-gradle-plugin#build-your-image) 插件添加到你的插件中。

我还添加了来自网飞的[星云发布插件](https://github.com/nebula-plugins/nebula-release-plugin)，用于这些图像的语义发布。

在充分发挥 jib 插件的潜力之前，您需要对它进行配置。

第一部分与 Dockerfile 文件中的相同。您还可以从 [openjdk:14-alpine](https://hub.docker.com/_/openjdk) 构建它，这是一个超轻量级的 java 映像。第二部分是为了出版。它将您的图像上传到您定义的目的地，并用项目版本对其进行标记。对于私人回购，您需要用用户名和密码定义 auth 部分。

现在，您可以运行 jib 命令来创建新图像。

正如您在这里看到的，有两种方法可以释放并构建这样一个映像。第一行创建一个名为的图像，格式为:

*<专业>。<小调>。<补丁> -dev。#+ <哈希>*

第二行使用 nebula 插件的 final 关键字。看起来是这样的。

*v <专业>。<小调>。<贴片> -rc。#*

其中`#`是到目前为止该版本的候选发布版本的数量。

# 创建您的构建管道

我创建了一个新的詹金斯管道，但你也可以使用任何其他管道系统。重要的部分在詹金斯的档案里。让我们来看看吧。

实际上，这个文件最重要的部分是发布-发布阶段。它使用私有 docker repo 的凭据。这些凭证需要在 Jenkins 中定义。如果您想了解如何在 Jenkins 中创建新凭证，请点击[此处](https://www.jenkins.io/doc/book/using/using-credentials/)。如果我们仔细看看这几行，我们可以看到我们有一个 If 子句，它要么在 git commit 上没有以“v”开头的标记时创建一个 devSnapshot，要么用最后一个 git 标记创建一个最终版本。

最后，如果一切正常，您现在应该能够在您的私有 docker repo 中看到一些映像，无论是 devSnapshots 还是最终版本。如果是这样，…恭喜你！你刚刚把你的第一张照片推给了码头工人。

# 反射

## 什么进展顺利？

码头工人形象的建立非常成功。Google Jib 插件在创建这些图像方面做得非常好。这些图像比预期的要快，并且易于创建。

## 有哪些需要改进的地方？

首先，我在 docker 图像的语义版本方面遇到了一些问题。读了一段时间的文件后，我发现我做错了什么。我漏掉了 release.useLastTag 前面的-P。我遇到的第二个问题是，我忘记在 Jib 插件的配置中添加一个认证条款。我想知道为什么我的图像不能被推送到回购，但这是清楚的，因为它需要认证。

我希望你能学到一些东西。😊