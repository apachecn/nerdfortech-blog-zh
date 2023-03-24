# Unity 跨平台输入

> 原文：<https://medium.com/nerd-for-tech/unity-cross-platform-input-dda00b0b93b?source=collection_archive---------15----------------------->

**目的:**使用 Unity 标准资产的跨平台输入系统。

![](img/a2f173919292c39297332d2a29c091f0.png)

到目前为止，我们一直在为 Windows 平台开发游戏。但这是一款手机游戏。我们可以使用标准资产的跨平台输入系统来设置移动控件！

首先，我们必须在我们的*播放器*脚本中包含 *CrossPlatformInput* 库。

![](img/52c2a158ef399407e1e5eabe3fe89401.png)

现在我们将不得不改变我们的运动代码来使用输入系统。跨平台输入系统不会破坏我们之前的代码。

![](img/4d505c8a5008c5c27bc0ac00501663c7.png)

我们将为我们的跳转按钮做同样的事情…

![](img/23894d106e8c2531cf794fd4b95fc85a.png)

…还有攻击按钮。

![](img/c7be0ebb24dcf10a69d8177d5662495f.png)

我们还必须确保按钮的*按钮处理程序*脚本指向正确的轴。在我们的例子中，跳转按钮应该指向*跳转*，而攻击按钮应该指向 *Fire1* 。现在，当我们将游戏导出到手机时，我们将拥有触摸控制！

![](img/dc2f54cc15fb8350862bffbe9ef810f9.png)