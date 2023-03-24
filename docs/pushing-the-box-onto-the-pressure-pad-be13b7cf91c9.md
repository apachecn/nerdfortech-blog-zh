# 将盒子推到压力垫上

> 原文：<https://medium.com/nerd-for-tech/pushing-the-box-onto-the-pressure-pad-be13b7cf91c9?source=collection_archive---------24----------------------->

首先让我们为我们的 Pad 制作一个脚本。

![](img/2e6d5b851cf173f72b9c9d31a948310f.png)

发射台有一个触发碰撞器，我们最初可以用它来检查进入其中的立方体。

![](img/df4a1164634a927b6673dbdde5fdf552.png)

所以我们先试着检测一下吧！像电梯面板一样，我们将使用 OnTriggerStay 方法。

![](img/b77f5271e80377d25f09efcc9d37b0a9.png)

让我们看看它是否能检测到:

![](img/511b0e6c54874bc40a7b3dc7daf329f8.png)

大获成功！接下来，我们想看看使用 Vertex3。距离将帮助我们检测盒子是否在范围内。我们用 1 作为开始。

![](img/5a741b6f85b379ab50727daeda7d4000.png)

让我们看看它是否有效:

![](img/08fbec498636b0a432440662fc7bc8ac.png)

有用！但是范围太宽了。我想让这个框在面板上，让我们把范围改成 0.2 左右

![](img/64855b3d103122b8156ea05b56da0816.png)

好多了！盒子现在实际上在发射台上方。让我们改变垫的颜色。这类似于电梯显示面板，让我们为 pads 'display '对象创建一个 holder 作为一个 MeshRenderer，拖动它，我们将它编码为蓝色。

![](img/457586fa37656b3ca216783bd5893d68.png)![](img/7047e820cce3b3630657650dda964180.png)

厉害！最后，让我们让盒子完全停下来，一旦它在垫子上，你就不能再推它了。这很容易做到，通过改变它的刚体运动学。

![](img/b05c1bceb44b5baa94b1b96497d5a9a7.png)

我们试试吧！

![](img/9d5a95a81ad9f2bf8ea664f2af4efe14.png)

这样我们就有了一个完整的压力垫技工！明天，我们将讨论升级我们的项目。