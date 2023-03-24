# 在 Unity 中创建主菜单

> 原文：<https://medium.com/nerd-for-tech/creating-the-main-menu-in-unity-6876e0cc65cc?source=collection_archive---------13----------------------->

![](img/505f6a32e792266857b2dff6c171210a.png)

在这篇文章中，我将讲述我是如何为我的游戏创建主菜单的。

首先，我创造了一个新的场景。

![](img/8e8e6f4608e862be4b08ae9994dfcf57.png)

在新场景中，我创建了一个新的画布来保存主菜单。

![](img/1a94f6872fa3813d5ce224fbee5cc5f7.png)

确保将 UI 缩放模式设置为**与屏幕尺寸**一起缩放，并将分辨率设置为 1920 x 1080。

![](img/21fa1427456680be89460ac608b7848e.png)

我已经有了我需要的图像和菜单的按钮图像，所以我添加了 UI 图像和按钮，并把它们放在正确的位置。

![](img/807d85ab927ecb68c7e2bcea597d8d23.png)![](img/bff86e1413f486fa0f0c4d99d5c635bf.png)

现在创建一个名为 MainMenu 的新 C#脚本，并将其附加到画布上。

![](img/b6016a5e46790923b16a82ecb1d5c921.png)

在新的脚本中使用了 **UnityEngine。SceneManagement** 命名空间，为你的开始按钮和退出按钮创建两个方法。

![](img/12e93ef3f48a0d909d5b93c7eb9c1749.png)

省省吧，去 unity。在点击事件的按钮上，将画布添加到事件中，并选择每个按钮的方法。

![](img/89f44b99b88d77ccd6e44a59c7c64ad1.png)![](img/1f1a384007d47dfec2a4160050a6193b.png)

这将给你一个功能性的主菜单。

![](img/3f445593e31da607f2770d59a031d59c.png)