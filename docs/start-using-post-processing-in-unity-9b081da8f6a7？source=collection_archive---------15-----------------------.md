# 开始在 Unity 中使用后处理

> 原文：<https://medium.com/nerd-for-tech/start-using-post-processing-in-unity-9b081da8f6a7?source=collection_archive---------15----------------------->

## 统一指南

## 关于如何开始在 Unity 中使用后处理的快速指南

![](img/886722da7275409c4d96d5a07574416a.png)

**目标**:设置 Unity 开始在你的游戏中使用后期处理特效。

为了开始在 Unity 中使用后处理，我们需要遵循以下步骤:

*   选择顶部的*窗口>程序包管理器*，打开程序包管理器窗口；

![](img/46b9d003c12cfcc817c67f0fac742129.png)

*   打开后，选择 **Unity 注册表**选项，在列表中显示后处理包。然后选择它并点击安装按钮:

![](img/9dba2cdff6fe0f26825e565c337b745d.png)![](img/c373615bc616a49f76320b15fc1d98fc.png)![](img/912e2c68eec3f50ef14fc7ee3f888276.png)

等待软件包安装到您的 Unity 项目中。

*   现在，您将能够在项目视图的包文件夹中显示后处理包:

![](img/79740c9db2216f3ac00f7000a1529d20.png)

*   安装后，创建一个新的空对象，并向其中添加一个后处理卷组件:

![](img/7df075962c3a6da0598f14df8fb421b8.png)![](img/8622206ee67c5696210bc5dbf63f45e5.png)

*   选择您希望后处理效果是全局的还是仅是局部的，并通过单击“新建”按钮创建新的后处理配置文件。概要文件将被创建，您可以通过点击它在项目视图中高亮显示它:

![](img/16c19998e2abd603c2d26da484aec77d.png)![](img/0aae4e0eb3a3b288ea5a66038187f99e.png)

由于我的游戏发生在一个简单的空间环境中，我选择使用全局效果。

*   这样，我们可以开始在游戏中添加不同的后期处理效果:

![](img/10b1725ea4a2998ca357ec586a6b9344.png)

*   但是，为了成功地显示效果，我们需要在相机游戏对象中添加一个后处理层组件:

![](img/8c7b6a4db87bcfad465ee65e9f19bab8.png)![](img/6aea3e41abf222884fe8785ca5845607.png)![](img/ec6229ec8a738a7f928d17172f8a05cf.png)

*   然后，让我们通过点击检查器顶部的**添加层**选项来创建一个新的用户层，以指示后处理层组件的层:

![](img/7c07e6eec72635afa36ca46b62ab7847.png)![](img/37a95b202fa0a5309baa192f2e78bddb.png)

*   此外，确保包含后处理体积组件的空游戏对象设置在同一层以显示效果:

![](img/dba945f3f562695c23574f36c875de08.png)![](img/9839aee9efe3589c01e1543bcbffccef.png)

*   现在，我们可以添加任何后期处理效果，并开始通过修改每个效果的不同属性来改变我们在 Unity 中的游戏外观:

![](img/a3c237f93a80678cdcbdb5b6bf9e36a2.png)

就这样，你可以开始在 Unity 中为你的游戏使用后期处理效果了！:d .我会在下一篇帖子中看到你，在那里我将展示如何使用不同的后期处理效果来使你的游戏进入 Unity 的 AAA 状态。

> *如果你想了解我更多，欢迎登陆*[***LinkedIn***](https://www.linkedin.com/in/fas444/)**或访问我的* [***网站***](http://fernandoalcasan.com/) *:D**