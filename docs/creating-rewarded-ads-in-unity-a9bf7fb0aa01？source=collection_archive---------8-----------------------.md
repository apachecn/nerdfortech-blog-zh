# 在 Unity 中创建奖励广告

> 原文：<https://medium.com/nerd-for-tech/creating-rewarded-ads-in-unity-a9bf7fb0aa01?source=collection_archive---------8----------------------->

![](img/ec5b4a3ba265a97e8c8940fd236e9973.png)

要创建奖励广告，您可以做的第一件事是添加一个播放广告的按钮。

为此，我复制了购买按钮，并将其移动到商店的右上角。然后我把它改成了商店的文本，并改变了颜色。

![](img/3676189617655ea9933bab75b3fa65c3.png)

现在，要启用广告，您需要进入**窗口= >常规= >服务**打开服务窗口。

![](img/6466781fb2fc0a1451dd9e2024f7c83f.png)

现在点击广告按钮，它会为你打开一个窗口。你需要选择你的组织，然后你可以打开广告。

![](img/bd6e3937f56151ef0f44782624302259.png)

现在，您需要转到 unity 仪表盘。前往**货币化= >广告单元。**在项目设置的屏幕上，为您正在使用的平台启用测试模式。

![](img/d9c212aafd742a0fa0612a212c5c83cc.png)

在 unity 中创建一个新的空游戏对象，附带一个名为 AdsManager 的脚本。

![](img/5ee67bfa060f7c38b61c7cf464a95d8d.png)

在脚本中为按钮、奖励宝石数量、游戏 ID、位置 ID 和玩家脚本创建变量。另外，实现 IUnityAdsListener 接口。确保使用统一的引擎。广告命名空间。在 start 方法和 AddListener 方法中初始化游戏广告。

![](img/febda05f626a43d10130b74609e3d10a.png)

创建一个显示广告的方法。在界面中，方法添加了允许广告在被观看后给出奖励的逻辑。

![](img/4b6ce55d7d65cd004f31b8819cc2e7ac.png)

在 unity 中，为游戏添加正确的 id。这些可在 unity 仪表盘的广告单元部分找到。

![](img/8c3a70766925a3d74f7e9b79dcd1c2af.png)

确保为广告管理器设置正确的点击方式。

![](img/baeac8148d47f6abb16edf744ecd0d66.png)

这会让你的游戏播放广告。