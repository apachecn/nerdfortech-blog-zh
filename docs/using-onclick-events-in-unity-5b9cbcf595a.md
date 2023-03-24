# 在 Unity 中使用 OnClick 事件

> 原文：<https://medium.com/nerd-for-tech/using-onclick-events-in-unity-5b9cbcf595a?source=collection_archive---------1----------------------->

![](img/142526b627865109ce98aa5fddbf7cef.png)

我一直在为我的游戏开商店。在本文中，我将介绍如何使用 unity 的 UI 系统和 OnClick 事件来设置商店的功能。

首先，事件需要函数来调用。在打开商店 UI 的脚本中，创建一个方法，该方法接受一个 int 值，并根据该 int 值和一个 switch 语句确定您选择的商品。

![](img/4802571027d55079be7fd33111fb227b.png)

保存并转到 unity 将事件添加到按钮。对于每个按钮，将脚本所在的商家游戏对象添加到 OnClick 事件中。选择正确的方法，并将正确的数字添加到方法中。

![](img/fffdd4871516114746be95d3a22b78d7.png)![](img/1b0f95e1a9e54d9ec0b3e1b1126583ae.png)![](img/17aa62428fe821a1fc1caeee58821c71.png)

现在，项目的按钮点击将实际上是正确的。

现在，为了使用 buy 按钮，我们将为它创建一个使用方法。首先，创建一个保存所选项目的值的变量。

![](img/3a0cfaf7dc7717169da2488a9100f8fc.png)

buy buttons 方法将使用所选项目的值，并确定给玩家的价格。你需要创建一个变量来保存物品的价格成本，以及一个方法来确保玩家有足够的宝石来购买物品。

![](img/4ae1032f16326ea08465bffd2f4c5a1f.png)![](img/82e90903798020c049b1073f8946d0c3.png)

注意:项目成本在项目按钮的 switch 语句中设置

这将允许玩家购买物品，如果他们有足够的宝石。

我还添加了一个显示所选项目的栏。

![](img/33d59f08b97376956d136fedc7c850b9.png)

注意:在 UIManager 中

![](img/afc8d585813c6c465f222d738d1328f9.png)

这给了商店更好的感觉。

![](img/61ecac9f29a64ef1c5c4fdd009d4b0af.png)