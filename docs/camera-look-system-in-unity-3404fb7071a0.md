# Unity 中的摄像机观察系统

> 原文：<https://medium.com/nerd-for-tech/camera-look-system-in-unity-3404fb7071a0?source=collection_archive---------8----------------------->

![](img/c247f68ba89f5da67b41c49774bc7436.png)

我让我的球员在移动，现在我需要摄像机跟踪他们。

你能做的第一件事是把你的照相机设置到你想要的地方，并把它交给播放器。这将使摄像机跟随玩家。

![](img/a340db63c38cad944d2262a129a4e1f4.png)

在玩家脚本中，您将需要设置对主摄像机的引用。

![](img/4e80d683399addcbe71c83d76538ffe8.png)

创建一个新方法来保存相机控件的代码。创建两个变量来保存鼠标在 x 和 y 轴上的移动值。

![](img/80ee6213c30fba477b69d4a29d7c1148.png)

要让玩家左看右看，创建一个新的向量 3，它将是玩家的 transform.localEulerAngles。将向量 3 的 y 值设置为正，等于 mouseX 输入乘以一个灵敏度值。使用**四元数设置玩家的旋转。AngleAxis(currentRotation.y，Vector3.up)** 。

![](img/b786b467aadc5b674e1edad34f3a850c.png)

注意:你可以在脚本的顶部设置一个敏感度变量，就像玩家速度变量一样

![](img/34dcc32b839ca15807bee492ee555f7a.png)

对于垂直相机运动，你做一些非常类似的事情。唯一的两个区别是，你将减去 moveY 输入，你将钳制 x 值，这样它们只能向上和向下看。

![](img/23f8e7f7910eeb07873c6c77dd65f242.png)![](img/0744eb190870b77da4273934ea5d5f2b.png)

这样的话，你也会注意到玩家并没有朝着摄像机面对的方向移动。

![](img/df4917f59d71424846aac4091a1e7286.png)

这是因为你需要将玩家的速度从局部空间转换到世界空间。这是通过使用 transform 完成的。TransformDirection()。

![](img/690aa01f3879757f026c692b46fee467.png)

这将解决该问题。

![](img/8e15d0324c093283aec30d310ed22eeb.png)

你可以做的最后一件事是像大多数射击游戏一样将光标锁定在屏幕中央。

这可以通过使用 CursorLockMode 在游戏开始时锁定光标，并通过点击 escape 键来解锁光标来实现。

![](img/6591632d7fcbddc53ec3aaeba5d268e4.png)![](img/4f3a2437b2b500fab4e6a854f00eeba1.png)