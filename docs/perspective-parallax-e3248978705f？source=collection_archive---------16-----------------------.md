# 透视视差

> 原文：<https://medium.com/nerd-for-tech/perspective-parallax-e3248978705f?source=collection_archive---------16----------------------->

目标:用我们的背景创建一个视差效果。

![](img/c9dd2d4beb504f2a3c9dad0c403597c5.png)

瀑布的视角尤其引人注目。

为了达到视差效果，我们必须确保我们的相机处于*视角*模式。

![](img/d28497b6a06644a5bb3dcfde234c199f.png)

接下来，我们将设置不同图层的 *z 位置*。z 位置的*越大，*离摄像机越远。**

以下是中使用的设置。上面的 GIF:

![](img/dc43202993c678dc216de3bad58eb385.png)

前景的 z 位置为 0。

![](img/d508eb62957c0c19a858918259f35a18.png)

黑色洞穴背景的 z 位置为 1。

![](img/d4c7682c43e62714c55b4c8b89f2e111.png)

瀑布和灰色洞穴背景的 z 位置为 3。