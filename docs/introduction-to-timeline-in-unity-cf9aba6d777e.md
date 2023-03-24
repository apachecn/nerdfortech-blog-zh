# Unity 时间轴简介

> 原文：<https://medium.com/nerd-for-tech/introduction-to-timeline-in-unity-cf9aba6d777e?source=collection_archive---------8----------------------->

## 统一指南

## 开始使用 Unity 时间轴的快速指南

![](img/f54550cb3a2fd027785afb67f64361ec.png)

**目标**:开始使用时间轴处理虚拟摄像机与 Cinemachine in Unity 之间的简单过渡。

在上一篇文章中，我介绍了[如何在 Unity](/nerd-for-tech/composing-a-cutscene-in-unity-330bc8b99d4c) 中构建过场动画。现在，是时候通过使用时间轴窗口来处理 Unity 中摄像机之间的简单过渡，使过场动画栩栩如生了。

# 现场

按照[旧帖](/nerd-for-tech/working-with-previs-elements-unity-89aa8103007c)中 previs 元素的要求，我们有了下一个场景，两个虚拟摄像机指向不同的角度:

![](img/c25565c088f32098de818135f8638d19.png)![](img/c684dd34d9c11e0f5d6aa153a9b0b5de.png)

现在我们有了摄像机，我们需要使用 Unity 编辑器中的时间轴窗口来实现想要的过场动画。

# 使用时间线

要打开时间线窗口，让我们点击*窗口>序列>时间线*:

![](img/4261f2323dd517610a46ffd0db3e453e.png)![](img/1ae9665aca1077822bcf015e43698f92.png)

让我们在一个空的游戏对象中组织虚拟摄像机和过场动画中的演员，以避免层次窗口中的混乱:

![](img/efe2b5cda06357f6207e3351bde44ee6.png)![](img/8b55373b6fc7d0cf7cb58d89c1eb204f.png)

然后，让我们通过选择空的游戏对象并点击时间线窗口的 ***创建*** 按钮来创建一个新的可播放导演组件和时间线资产:

![](img/d5459af8b9c6dc7565963072ed5d0d1d.png)![](img/7554e67a908689fcdd064364956e4f11.png)

由于我们在上一篇文章中用 Cinemachine 包创建了一对虚拟摄像机，我们在场景的主摄像机中创建了一个 **CinemachineBrain** :

![](img/8b78809c2274ba3921c6bcd77b6422f1.png)

现在，为了控制虚拟摄像机，让我们将 **CinemachineBrain** 拖到时间线窗口，创建一个新的 Cinemachine 轨道:

![](img/a61e48e48562586e9c2c6ee5dff1d62e.png)![](img/fb8875bb09f9eba7d179b6cb160ec044.png)

如果愿意，我们可以更改时间轴设置，以显示秒而不是帧作为参数值:

![](img/5b82ac29887588a13ec4ca3e5269ff0a.png)

然后，让我们在 Cinemachine 轨道中创建一个新的 **Cinemachine 镜头**，以在拍摄过程中选择虚拟摄像机:

![](img/5dc5196a6d2fef92b3b0b7709741a82e.png)

要开始过场动画，摄像机在肩膀上，让我们通过检查器将它拖到 Cinemachine 镜头中:

![](img/60ea20df80c06629be86feb37849427b.png)

然后，让我们对指向另一侧卡片的虚拟摄像机做同样的操作:

![](img/454c909619d6d4aadc3858af4a7c507a.png)

这样做可以让我们自定义在过场动画中使用每个虚拟摄像机的时间:

![](img/f8c4d2f70a6f01ba38e47ae1fd55c040.png)

如果我们在时间线窗口中按下 play，我们将能够看到虚拟摄像机是如何处理的:

![](img/9891d1768aadea0fcf0ce65a0bb93b50.png)![](img/e5f4e3cf8326c9cbb3ab76d61b0c8e83.png)

还可以选择混合 Cinemachine 镜头，以改变各自的摄像机，并在混合期间显示它们之间的路径:

![](img/9875995e7f25f14541f0f1b130af7250.png)![](img/7b193b0c4bcecca521b643356fb7cda3.png)

# 下一首曲目

因此，如果我们继续使用时间轴来组合 Cinemachine、动画和激活的不同轨道，我们将能够按照预期控制过场动画，并获得所需的结果:

![](img/e99cf87987e664beb3952e511ab14b17.png)![](img/dccdf78933c222ee43d6cd7044bdcf98.png)

就这样，我们在 Unity 中引入了时间轴窗口！:d .我将在下一篇文章中看到你，在那里我将展示如何在 Unity 中使用移动轨道。

> *如果你想更多地了解我，欢迎登陆*[***LinkedIn***](https://www.linkedin.com/in/fas444/)**或访问我的* [***网站***](http://fernandoalcasan.com/) *:D**