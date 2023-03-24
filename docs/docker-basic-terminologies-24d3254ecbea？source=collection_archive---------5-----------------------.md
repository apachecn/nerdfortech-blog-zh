# Docker，基本术语

> 原文：<https://medium.com/nerd-for-tech/docker-basic-terminologies-24d3254ecbea?source=collection_archive---------5----------------------->

Docker 是 Devops 领域中最常用的术语之一，为了深入了解 docker，我们可能需要了解一些基本术语，在本文中，我们将介绍 docker 中三个最常用的术语。

![](img/13ec8449325e865531d7b374f71594f1.png)

由 [OpticalNomad](https://unsplash.com/@opticalnomad?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# **集装箱**

一个*容器*是我们最终想要在 Docker 中运行和托管的。你可以把它想象成一台孤立的机器，或者你愿意的话，一台虚拟机。

从概念的角度来看，一个*容器*运行在 Docker 主机内部，与其他容器甚至主机操作系统相隔离。它看不到其他容器、物理存储或获取传入连接，除非您明确声明它可以。它包含了运行所需的一切:操作系统、包、运行时、文件、环境变量、标准输入和输出。

# **图像**

任何运行的容器都是从一个*映像*创建的。图像描述了创建容器所需的一切；它是容器的模板。您可以根据需要从单个图像创建任意多个容器。

# **注册表**

图像存储在*注册表*中。在上面的例子中， *app2* 图像用于创建两个容器。每个容器都有自己的生命，它们都有一个共同的根:它们在注册表中的映像。

让我知道你对上述文章的想法，或者如果我在评论区错过了什么，谢谢你的来访..