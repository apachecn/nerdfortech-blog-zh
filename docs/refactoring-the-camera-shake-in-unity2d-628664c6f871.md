# 重构 Unity2D 中的相机抖动

> 原文：<https://medium.com/nerd-for-tech/refactoring-the-camera-shake-in-unity2d-628664c6f871?source=collection_archive---------14----------------------->

![](img/b914719112e42471caf7c568ad923898.png)

在我的上一篇文章中，我展示了如何在玩家受到伤害时让相机抖动，而不使用任何插件。我不喜欢我目前的相机抖动设置的一点是，它是在 **void Update** 中运行的，这意味着，游戏会检查每一帧，看看相机是否应该抖动。那可能是一个**昂贵的**操作，所以我更喜欢有一个**公共方法**，它可以运行到它在…之外完成