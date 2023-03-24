# Unity 中的简易安全摄像头

> 原文：<https://medium.com/nerd-for-tech/easy-security-cameras-in-unity-ffa02c26bcaa?source=collection_archive---------21----------------------->

这篇文章将展示如何制作一个安全摄像机，当玩家进入摄像机的视野时，它会停止动画并变成红色。

![](img/99737fdb1a191dd670c57415fd46282c.png)

监控摄像头会左右摇摆。

![](img/75a767336e9bb2656fc139ca467810d7.png)

视锥附有触发碰撞器和脚本。

![](img/888179075423ba67a67e68d021ca6ace.png)

SecurityCamera 脚本需要变量来激活过场动画游戏对象、过场动画之前的延迟时间、检测到玩家时要更改的颜色、对父动画制作人停止动画的引用以及对 MeshRenderer 的引用，以便我们可以更改视觉锥体的颜色。

![](img/4f7ae10abc69e6cdde07ba9e7bce38ac.png)

当玩家进入安全摄像头的视觉碰撞器时，颜色会变成警示色。动画制作者被禁用以停止移动，并且为过场动画延迟启动协程。我们正在禁用过场动画的玩家游戏对象，所以它在背景中看不到。

![](img/8c183e71c913b065b06f791806958724.png)

在 Unity 中，我们连接过场动画并设置颜色为 alpha。

![](img/d8baf949a87e44a290ebabdc7683a14d.png)

当安全摄像机发现玩家时，视锥变红并在短暂延迟后开始过场动画。

![](img/ea031f7f5c9898d5e0c35a5a294fec93.png)