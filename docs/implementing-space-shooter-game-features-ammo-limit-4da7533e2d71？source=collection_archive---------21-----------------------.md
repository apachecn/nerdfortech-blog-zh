# 实现太空射击游戏功能-弹药限制

> 原文：<https://medium.com/nerd-for-tech/implementing-space-shooter-game-features-ammo-limit-4da7533e2d71?source=collection_archive---------21----------------------->

## 统一指南

## 快速回顾 Unity 中添加到太空射击游戏的新功能

![](img/ff5e45ccdcc4a5fc734255fa8fb4299f.png)

**目标**:用 Unity 实现一个限制玩家在太空射击游戏中发射弹药的系统。

在之前的帖子中，我[在我的 Unity 太空射击游戏中实现了一个健康增强项目](/nerd-for-tech/implementing-space-shooter-game-features-extra-life-a670cc82f2b9)。现在是时候实现一个系统来限制弹药，显示其统计数据，并提供一种补充弹药的方法。

# 限制弹药

为了限制弹药，让我们从打开玩家脚本开始:

![](img/f8d06183d2f2aa81f2e76abb7725f920.png)

一旦打开，让我们创建一个新的变量来存储玩家能够携带和发射的子弹或激光的数量:

![](img/2037220a531e917b9ceee767c234b01d.png)

然后，在玩家每次射击时执行的方法中，让我们在开始时添加一个条件，以检查是否没有激光可供射击，如果是这样，让我们退出该方法:

![](img/81e29e042c5d691bc6a41e0efd2721d1.png)

现在，如果玩家射击，让我们在方法结束时从激光中减去一个单位:

![](img/c182ff8fb5d0dad613bb47fd15a70d89.png)

# 显示弹药状态

## 带音频

为了让玩家意识到弹药耗尽，让我们添加一个新的音频到没有弹药时播放的玩家脚本数组中:

![](img/10565d70c5d433c2196993ca87fd25c8.png)![](img/8b2024b4fa143e5c53cb2fe01d94fe50.png)

让我们添加相应的调用来播放先前条件中的音频，包括新音频的索引作为参数:

![](img/38d77816527ea98306f620f2975d1660.png)

## 使用 UI 元素

同样，我们可以使用 UI 元素来显示弹药状态，让我们添加一个新的 UI 元素，就像我们在之前[所做的那样:](/nerd-for-tech/creating-ui-elements-in-unity-a778929eacfa)

![](img/bf9947611ed1fb78c26fa3a8fab6351a.png)![](img/5bab1cf3f2dcd5525edf1d0211ac69c4.png)

使用 2 张图片和 1 个文本元素，我们可以确保提供当前弹药的状态:

![](img/b6243e90eedb555ac950b9cca749a0e6.png)![](img/3af6a12e5b4cbecd7947fdc7490f6e57.png)![](img/091e6cc4298b98d7d3be190720aa0596.png)

在顶部，我们使用文本元素来显示可用激光器的数量。在底部，我们用一个图像来显示要射击的弹药类型。

现在让我们打开 UI 管理器脚本，创建一个新的公共方法来更新弹药状态:

![](img/84a382c48f23ea2d690ff8e6c0259f78.png)

该方法将获得剩余的弹药总量。

然后，为了能够更新屏幕上的值，让我们创建一个新变量来存储对画布中文本元素的引用:

![](img/f3d094f85239e3bac640137b1f001e7c.png)

不要忘记使用**【serialize field】**将文本元素拖到检查器中。

现在，将文本元素拖动到画布中的 UI Manager 脚本组件中:

![](img/d8cc94882e4137e7385b5b3d53f8eecf.png)

这样，我们就能够在每次调用该函数时用相应的值修改 text 属性:

![](img/442f74cbee2da0cf22f8c68a5a32acf0.png)

最后，打开玩家脚本，将相应的调用添加到 UI 管理器中，包括玩家每次射击时弹药的当前值:

![](img/62700d17cc62e9d2a7121b79c74930c4.png)

现在，如果我们在 Unity 中运行游戏，我们会看到当玩家射击时弹药值会更新:

![](img/a1ff8c3db368aa67ae69d3e0a607d37a.png)

# 提供补充弹药的方法

最后，但不是最不重要的，让我们添加一个新的电源项目来补充玩家的弹药。如果你不知道如何在你的游戏中实现一个加电物品，你可以访问我以前的一个帖子:

[](/nerd-for-tech/creating-a-power-up-for-your-game-in-unity-6810d73376a1) [## 在 Unity 中为你的游戏创造动力

### 关于如何在你的 Unity 游戏中实现一个增强道具的快速指南

medium.com](/nerd-for-tech/creating-a-power-up-for-your-game-in-unity-6810d73376a1) 

让我们添加新加电项目的精灵，包括其他加电项目具有的各个组件:

![](img/82f96b1bdac2697baf1704064e433d23.png)![](img/47b74ad0baa4dc85731708181fa52d9a.png)![](img/e2aa03271dc9818e658f5bac1468f466.png)

然后，如果我们再次运行游戏，我们将能够在收集能量物品时补充弹药:

![](img/5ee97a761da4f73118fdfe509e008525.png)

就是这样，我们实施了一个限制弹药的系统！:d .我会在下一篇文章中看到你，在那里我会展示更多添加到我的 Unity 太空射击游戏中的功能。

> *如果你想更多地了解我，欢迎登陆*[***LinkedIn***](https://www.linkedin.com/in/fas444/)**或访问我的* [***网站***](http://fernandoalcasan.com/) *:D**