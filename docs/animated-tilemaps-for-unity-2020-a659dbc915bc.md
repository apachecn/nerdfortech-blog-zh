# 适用于 Unity 2020+的动画磁贴地图

> 原文：<https://medium.com/nerd-for-tech/animated-tilemaps-for-unity-2020-a659dbc915bc?source=collection_archive---------4----------------------->

过去，如果你想在 Unity 中制作动画 Tilemap，你必须进入 Unity 的 github，手动下载“Tilemap Extras”并安装。现在您可以直接从软件包管理器安装它，尽管需要一些额外的步骤:

首先你需要进入编辑/项目设置/包管理器并勾选“启用预览包”

![](img/f5393921d9f611c30316889a7c7b6c0f.png)

你会得到一个警告，告诉你这些是全新的选项，Unity 不会对任何着火的电脑负责，放弃你的第一个为老神而生的孩子，等等，等等，点击“我明白”。

![](img/cebfbdd88b14e7441e98eafc015877e2.png)

现在当你进入包管理器时，tilemap extras 将会出现！让我们点击安装！

![](img/2397b6786c4cfff160ef93fec1d1055b.png)

几秒钟后，该包将成为您的 Unity 开发包的一部分，您可以开始工作了！实际上还没有。您可能需要退出 Unity 并重新加载它，以便软件包初始化。

如果由于某种原因这不起作用，我们可以用老方法去 https://github.com/Unity-Technologies/2d-extras 的 Unitys Github 下载代码。

![](img/cf922a43e89877275a31895a403d4694.png)

接下来，解压文件，打开 unity 项目的 packages 文件夹，将解压后的文件夹复制到其中。

![](img/affa096720dfb4f94e2ac055fbe04bc4.png)

接下来打开 Readme.md 并复制 json 行

![](img/5a9e64ef0ac4ad0d673b61f186427845.png)

并将该行粘贴到 manifest.json:

![](img/d4a6629322c0fa1acc6d5d4572b1b12f.png)

非常感谢 [Gamenomoly youtube 频道](https://www.youtube.com/channel/UC-t7hMnrRf2nwwrr9sZ-dzA)提供的信息！

也就是说，我在让 tile extras 工作时仍然有问题，我最终只是删除了所有内容，并从零开始了一个新的干净的项目，然后使用包管理器方法安装了它。这个故事的寓意是，在项目开始的早期，尝试获得所有计划的扩展。

现在一切都准备好了，我们得到了一个瀑布的 spritesheet，我们把它分割成 256x256 的部分

![](img/49766f852b26c0240b3a1acbe58e5ea3.png)![](img/d8286b25d8d64b26cd48ccacaf4440c5.png)

现在，如果我们真的浏览我们的瀑布精灵，你可以看到每一帧是如何制作动画的。

![](img/c3d283c0cffd524ae195475a379159e8.png)

准备好所有 3 个瀑布后，瀑布中心，左侧和右侧。我们将它们放在图块编辑器中，但不是每一帧，我们将创建一个包含所有帧的动画图块，并用它进行绘制。

首先在 Sprites/Tilemap/Tiles 中，我们创建一个瀑布目录，然后右键单击创建名为瀑布 _ 左的动画 2D 瓷砖。

![](img/d560d6a68351d44a1d595caa4210ebf1.png)

以前，你必须将每一帧拖动到动画瓷砖中，但幸运的是，现在你只需要将切片的精灵拖动到其中。

![](img/091fe0d107433e0013765edf728fa8ed.png)

然后，我们对瀑布中心和瀑布右侧重复这个过程。

接下来，我们为瀑布瓷砖创建一个新的调色板，并将瓷砖放入其中。

![](img/36506e6c3c18ce1d986b8024951b959f.png)

接下来保存场景以确保调色板设置也被保存。

我新建了一个名为瀑布的 tilemap 图层，将排序图层改为 3，然后在上面绘制瀑布。

![](img/b1b77186655d29da8d9bdb1b19a697b5.png)

点击播放，我们看到瀑布动画，但只有 1 fps。让我们加快速度。

![](img/f814bbfa213056d6f0d89c79027115aa.png)

如果我们选择瀑布瓷砖，在检查器的底部你会看到最小和最大速度。只需将所有 3 个瀑布资产的最小速度更改为 30。

![](img/c5ed1bf2e209463c9c9f01448e541fdc.png)![](img/9fffb2c528e2806d4708dd080b3433ca.png)

好多了！瀑布现在以每秒 30 帧的速度运动！

再补充几个吧。

![](img/d24e256f32f9812f07145176ff7baf9c.png)

明天，我们将讨论预制笔刷和碰撞器。