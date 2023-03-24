# 第二阶段:敌人护盾

> 原文：<https://medium.com/nerd-for-tech/phase-2-enemy-shields-b98c26be09cd?source=collection_archive---------14----------------------->

**目的:**提供让部分敌人拥有护盾的能力。护盾可以让敌人受到一次攻击。

![](img/ddfac28f06549c756fdddde43426cac0.png)

敌人护盾启动

首先要做的是给敌人加一个盾精灵。我将把它命名为敌人之盾。此外，我们需要调整大小，并将其添加到前景改变秩序层，使其显示在敌人

![](img/f050c7a473bac7091eb99100e09a1c94.png)

接下来，我们要调整颜色并保存对预设的修改。

![](img/1f709e2d295fbd906c4e33a7a65e0f1e.png)

现在是更新脚本的时候了，我们将会给敌人的脚本添加一些逻辑。首先，我们需要一个敌人的参照物。

![](img/0f8cf233682fe4cccbc5af8ccb5c59be.png)

在检查器中添加并分配它

![](img/0b1dbcf42d6425af33472280ea701721.png)

分配敌人 _ 盾牌

接下来，我们需要一个变量来让我们知道敌人什么时候激活了护盾。

![](img/2bc865a2c545d08031cf0b2991e5eb93.png)

防护罩启动了吗

当敌人的护盾被激活时，我们将调用一个方法来打开护盾视觉。我将调用方法 ActivateShield。此方法将打开 visual 并将 bool _isEnemyShieldActive 设置为 true。

![](img/137733216989c64bed767407576c4d70.png)

在 OnTriggerEnter2D 方法中，我们将添加一个检查，检查敌人的护盾是否处于活动状态。如果是 active，我们关闭可视屏蔽并将 _isEnemeyShieldActive 设置为 false，然后返回以便跳过方法中的其余代码。

![](img/f9be55ff3116c483859a95dc3fcb3625.png)

为了随机选择拥有护盾的敌人，我使用了 RandomValue 来得到一个介于 0.0f 和 1.0f 之间的数。我将只在值< 0.20f 时调用 ActivateShield 方法。

![](img/9df9348d68e25dc27aa5692cc8b1a903.png)

让我们来看看实际情况

![](img/ddfac28f06549c756fdddde43426cac0.png)

敌人盾牌演示

编码快乐！！