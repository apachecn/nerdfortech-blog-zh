# 专题:Boss 敌人—第 3 部分

> 原文：<https://medium.com/nerd-for-tech/feature-boss-enemy-part-3-6aaf256a2845?source=collection_archive---------29----------------------->

这篇文章将从上一篇文章看最后一波后 [Boss 敌人的制造。还有，我们会在玩家打败 Boss 的时候更新 UIManager。](https://kwpowers.medium.com/feature-boss-enemy-part-2-2aec9663711b)

![](img/8c87e7b85d14ccd88c2a6aa8aa9b592a.png)

## UI 管理器

对于 UIManager 脚本，我们将为 Win 文本添加一个文本变量，并在 Start 中禁用它。

![](img/4feb702a194fee14d58bac74a545879a.png)

我们将改变[gameofflickerroutine](https://kwpowers.medium.com/creating-game-over-behavior-7134d8853bf6)以用于任何传入的文本对象。

![](img/76499f9a6f2531946d6535d9abe3d9c4.png)

现在我们需要一个名为的方法，用传入的 Win 文本启动协程，打开重启文本，并告诉游戏管理器游戏结束了。

![](img/ba92033bff4e8bf96f631703de11ad31.png)![](img/dd034060fd2b4d9e8c3f7a9374b30c35.png)

## 产卵管理器

我们需要添加到 SpawnManager 中的变量是一个 Boss 预置的 GameObject，一个 Boss 将在屏幕外生成的 Transform，一个知道这是否是最后一波的 bool，一个告诉 Boss 是否活动的 bool。

![](img/295c28013dfae86cc30fd856fc00660c.png)

这个协程将在延迟后在 Boss 产卵点产卵 Boss 预置，启用能量的产卵，启动能量产卵协程，将 isBossActive bool 设置为 true，并告诉脚本再次计算敌人。

![](img/7a61aec5d80333467542e337c37fa992.png)

在更新中，当检查留在场景中的敌人时，我们也会检查 isFinalWave 是否为 false。当剩下零个敌人和最后一波时，我们启动 spawnboss 例程并将 isFinalWave bool 改为 false。然后，当老板死了，我们将停止计算敌人，停止发电产卵，并激活胜利文本。

![](img/b5cb8aaa8487a8a00529747ae1c16e8a.png)

在 StartSpawning 方法中，我们将检查当前波加 1 是否等于波的总数，并将 isFinalWave 设置为 true。此外，我们将确保当前波形不会超出索引范围。

![](img/b91150e0f8f7dcb4a8e9e95498877074.png)

在 SpawnPowerUpRoutine 中，我们将改变它，这样如果老板是活跃的，罕见的加电次数是不受限制的。

![](img/1c55155954c3d69fd793086dd91e610d.png)

Boss 在最后一波后产卵，当被摧毁时，会出现胜利的文字，这样玩家就可以重新开始游戏。

![](img/db9e384340fe338ff21328018c36c79c.png)

下面这篇文章将是重构空间射手代码的开始。