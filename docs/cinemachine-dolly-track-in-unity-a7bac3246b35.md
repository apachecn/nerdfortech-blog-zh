# 统一电影放映小车轨道

> 原文：<https://medium.com/nerd-for-tech/cinemachine-dolly-track-in-unity-a7bac3246b35?source=collection_archive---------10----------------------->

本文将展示如何在 Cinemachine for Unity 中使用 Dolly 轨道。在前一篇文章中，我们使用 Cinemachine 为过场动画设置了不同的镜头。我们可以使用几个虚拟相机来实现我们想要的外观，但这不会像小车轨道那样平滑。

![](img/52b3ebbc60bd8d45fb13902f0c0f0f1e.png)

首先，我们从 Cinemachine 创建一个小车轨道，并删除它附带的虚拟摄像机，因为我们已经设置了一个。然后，我们改变虚拟摄像机，使用一个跟踪小车来移动身体。

![](img/dd1b83068d0b957eac93d08abf3e5293.png)

接下来，我们将让虚拟摄像机看一下目标。我们这样做是为了让主角一直在小车轨道上。

![](img/528d8190ee7d719833f881adf07e6593.png)

移动轨迹使用航路点来创建相机将沿其移动的路径。

![](img/d03b25f833aea06aaca76ebe3c4e727e.png)

一旦设置了移动轨迹的路径点，就该沿着路径设置虚拟相机的动画了。

![](img/e77914255f66af657431b5e9d6aa3cd0.png)