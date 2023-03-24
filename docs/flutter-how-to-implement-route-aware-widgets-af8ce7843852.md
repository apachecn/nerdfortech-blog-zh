# Flutter:如何实现路由感知小部件

> 原文：<https://medium.com/nerd-for-tech/flutter-how-to-implement-route-aware-widgets-af8ce7843852?source=collection_archive---------1----------------------->

![](img/26879a1211d6a0dbbc3956d8ef38e685.png)

有时你可能想做一些基于路线改变的事情。

例如，在某些页面上，您希望显示全屏视频播放器，或者显示交互式全屏图表。您可能想在进入这些屏幕时调用`SystemChrome.setPreferredOrientations()`来强制水平方向，但是不要忘记在用户离开屏幕时再次调用它来重置方向！