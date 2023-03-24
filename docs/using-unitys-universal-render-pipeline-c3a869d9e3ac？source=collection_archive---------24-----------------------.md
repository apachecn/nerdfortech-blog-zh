# 使用 Unity 的通用渲染管道

> 原文：<https://medium.com/nerd-for-tech/using-unitys-universal-render-pipeline-c3a869d9e3ac?source=collection_archive---------24----------------------->

**目标:**升级我们的项目以利用*通用渲染管道*。

![](img/f0dcb7b327d8c0c84944ca44b1b1ab8e.png)

在 URP 之前…

我们在新的 Unity 项目中引入了一些看起来很酷的资产。但是当我们在我们的场景中弹出它们的时候，它们是…嗯，粉红色的。不是我们想要的那种坚韧不拔的精神。这些资产旨在与*通用渲染管道*一起使用。下面是如何安装 *URP:*

在菜单栏上，进入*窗口* → *包管理器*。在*包管理器*中，搜索*通用 RP* 并*安装*它。

![](img/d5fbe6af0b8a0c807bc01bbf4563eb4a.png)

我们将通过右键单击项目资产并转到*创建→渲染→通用渲染管道→管道资产(正向渲染器)*来创建一个新资产。默认情况下，这实际上会创建两个名为*UniversalRenderPipelineAsset*和*UniversalRenderPipelineAsset _ Render*的资产。

![](img/44ba7198331154d7148711ae6b02fc1b.png)![](img/2e7803f2d23f4fe1011271fd773245e6.png)![](img/8967a26acdc455285ceef31c94fefcaa.png)

现在，我们将返回菜单栏*编辑→项目设置→图形*并选择我们的新资产。

Unity 会自动更新你的物体上的材质来使用渲染管道！

![](img/a86a39c1801801499068400740fd117b.png)

…在 URP 之后！

如果材质没有自动更新，或者如果后来引入了新的资源，请转到*编辑→渲染管道→通用渲染管道→将项目材质更新为通用材质*以强制更新。

![](img/3108d8e53aa96493ca1dada0bd62f269.png)