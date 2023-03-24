# 在 Unity 中创建暂停菜单

> 原文：<https://medium.com/nerd-for-tech/creating-the-pause-menu-in-unity-e48277abdf8e?source=collection_archive---------0----------------------->

![](img/e0ab54c6fe618fa049b30803a7aea256.png)

是时候为我的游戏创建暂停菜单了。

首先，为 HUD 面板创建一个新按钮。您可以禁用此按钮的文本。对于图片，我在 unity asset store 免费找到了一个漂亮的暂停图标。把这个按钮放在屏幕的右上角。

暂停图标链接:[https://asset store . unity . com/packages/2d/GUI/icons/basic-icons-139575](https://assetstore.unity.com/packages/2d/gui/icons/basic-icons-139575)

![](img/4a9af0027b3d0db79bccc148b20cf6e7.png)

在现有画布上创建一个新面板，并将其命名为暂停菜单。可以复制暂停按钮，放在暂停菜单里。将名称更改为 resume 按钮。然后添加菜单和退出游戏两个按钮。

![](img/7b3711a86cf073b78004c56c15f95077.png)![](img/63ce28b0fe14cd1d3cbf9a96854c2792.png)

将主菜单脚本添加到画布。您可以对“退出”按钮使用 quit 方法。

![](img/8879e8e3716dce13c759807500fc4d06.png)

在脚本中，获得暂停菜单和暂停按钮的两个引用。然后为暂停按钮、继续按钮和菜单按钮创建新方法。

![](img/62f3b7e6def0a5e19e673ed4213aebb1.png)

将引用添加到脚本中。

![](img/6fc01d310ffa633c3d75b2ebd83c4393.png)

对于主菜单中的引用，您可以将暂停图标拖到层次结构中并禁用它。然后将它添加到引用中。

![](img/e028dc45a5e190d18c044921a5cc2f10.png)

现在将这些方法添加到正确的按钮上。

![](img/653aa77f14bca2f92a37d964c41ad834.png)![](img/9ce27f3af3ab79f502b6612add300591.png)

默认情况下禁用暂停菜单。

![](img/ce2cc0aafc544911b8667b0001968acf.png)

现在你应该有一个工作暂停菜单。

![](img/c4d0ece593204c92ebce29f6e1e84d3c.png)