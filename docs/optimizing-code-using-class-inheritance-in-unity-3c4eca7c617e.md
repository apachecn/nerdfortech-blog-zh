# 在 Unity 中使用类继承优化代码

> 原文：<https://medium.com/nerd-for-tech/optimizing-code-using-class-inheritance-in-unity-3c4eca7c617e?source=collection_archive---------8----------------------->

![](img/b18a2a7e3e5022929e4c58792c02cc8e.png)

在游戏中，我创造了多个敌人，他们有自己的脚本，本质上做着同样的事情。我决定这将是一个使用类继承的好时机，这样如果我决定添加更多的敌人，我就不必一遍又一遍地输入相同的代码。

首先，我必须获得对敌人类中的 animator 和 sprite 渲染器组件的引用来控制动画。我创建了引用并在一个虚拟方法中初始化它们。

![](img/f906dd61606586e3c00284408b88990f.png)

现在创建一个虚拟方法来保存敌人的移动代码，并将代码复制到虚拟方法中。

![](img/503b71415a9df0b58d2602c637e8172e.png)

注意:将 animator 引用替换为敌人类中的那个

用翻转精灵的方法做同样的事情。

![](img/15319ad94d27b70c4eae526a0cc78d23.png)

在敌人类中设置更新方法，使其与在单个敌人脚本中的方法相同，只是替换了 animator 引用。

![](img/f8c6c976909f15d03f7114e687920339.png)

在敌人类中创建一个将调用 init 方法的 start 方法。

![](img/d103723cab9df1f520b01f2a8900572f.png)

现在你可以删除单个敌人脚本中的所有代码。

![](img/29bc84215ccbcb16244ca928453cc872.png)![](img/065a584a72986db240ee40b72a2be9c0.png)

当这些类继承了敌人的类时，它们现在不需要任何代码就可以工作了。

![](img/841992abf8a3f25ee881c8a456e02ce7.png)

这也允许你保留原来的功能，并为每个敌人添加特定的代码。

如果你想增加苔藓巨人的高度，创建高度变量。然后您可以覆盖 init 方法并添加代码。

![](img/a5c5faefd1725ffca965e4fa92648ef9.png)

注:基数。init()；保留基本功能

现在身高变量只会出现在苔藓巨人身上。

![](img/8cb1342b91b59c43732a1d5f36c9b359.png)![](img/0dee99912bfa1536b0ce489cd56ec27c.png)

当我点击 play 时，它会将高度变量设置为 10。

![](img/acd9bb3174abb13574821dbd88d67c3d.png)