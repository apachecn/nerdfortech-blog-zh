# 空间射手重构:委托

> 原文：<https://medium.com/nerd-for-tech/space-shooter-refactoring-delegates-882218c249a5?source=collection_archive---------15----------------------->

本文将展示如何使用上一篇文章中讨论的 Action 和 Func 委托在 Space Shooter 中实现观察者设计模式。

![](img/58b2039efa9b55ffd7223ded1a3079b0.png)

这是一个笑话，因为委托的添加和移除方法可以被称为订阅和取消订阅。但是严肃地说，前面有很多代码块。

由于实现委托需要修改大量代码，我将展示基本实现的主要例子。如果你想看到所有的变化，你可以看看 GitHub 库。

## 游戏经理

为了访问委托，我们需要使用 System 命名空间。对于 GameManager 脚本，我们将为分数、玩家游戏对象和玩家脚本添加变量。将分数转移到游戏管理器更有意义，因为玩家可能会被禁用或破坏。我们会有任何需要引用玩家游戏对象的脚本，或者从游戏管理器中请求引用的脚本。

![](img/b194fa6365981f68587c28e2de68fde5.png)

当按下 Escape 键时，我们将使用动作来通知 UIManager，并将更新后的分值传递给 UIManager。我们将使用 Func 从玩家那里获取游戏对象。

![](img/5bd4edc281be881df446d7a72183696f.png)

在 Start 中，我们将 score 值发送到 UIManager，将 playerObj 设置为检索到的 GameObject，如果它不为 null，我们将 playerScript 设置为其上的 Player script 组件。

![](img/5ad6bc15a93ee74aca061092019395d0.png)

如果按了 Escape 键，如果有监听器的话，OnPauseMenu 会被调用。

![](img/9e78eb1412126dd0f9121c58333d7a61.png)

第一个方法将返回由 GameManager 脚本获取的玩家游戏对象，并将其传递给该方法正在监听的代理。第二种方法与第一种方法相同，只是针对播放器脚本。最终的方法将根据给定的数量更新分数，然后将其发送给 UIManager。

![](img/a75f0c01dcc1afd3af24295f2fe562ef.png)

在 OnEnable 和 OnDisable 中，我们为其他脚本的委托订阅和取消订阅方法。

![](img/8601c581f7584d11f02c707ff7bf5c14.png)

## 产卵管理器

由于 SpawnManager 脚本使用 System 和 UnityEngine，我们需要定义我们打算使用哪个随机数；我们想要随机的单位引擎。

![](img/51f3147e760563bd7c9aad89fb292c04.png)

SpawnManager 现在将保留一个活跃敌人的列表，供 HomingMissile 脚本抓取并用作目标，并使用 int 跟踪场景中活着的敌人的数量。我们将使用动作来开始下一个波形文本的闪烁，并在 UIManager 上更新波形文本。我们将使用 Funcs 从池管理器中取回正确类型的敌人和力量。

![](img/ea89fcf1963f74e2a1fcd9147dbd9e30.png)

开始时，我们将当前波形和总波形发送给监听 UIManager。

在 Update 中，将它改为使用新的 enemiesActive 变量来检查是否还有敌人活着。

![](img/cf5b7bf8492cd58ad45f7dbc4979ad62.png)

在 StartSpawning 中，我们现在调用 OnUpdateWaves，将当前和总波传递给侦听 UIManager。然后调用 OnNextWave 来告诉 UIManager 闪现下一个 Wave 文本。

![](img/af58af8a8922907ee21bced394dd7b52.png)

第一个方法将把活动敌人的列表返回给该方法正在监听的代理。第二种方法增加了活跃敌人的数量，并将敌人游戏对象添加到活跃敌人列表中。最后一个方法是减少活跃敌人的数量，并从活跃敌人列表中移除敌人游戏对象。

![](img/e410fe9805ee93b97c9d17672fd2895f.png)

在 SpawnWaveRoutine 中，我们将其更改为使用 OnGetEnemy 从池管理器中检索敌人游戏对象，然后将敌人添加到活动敌人列表中。

![](img/7fa0d1e78ee9320e0b6dabd78bc2d7cc.png)

在 OnEnable 和 OnDisable 中，我们为其他脚本的委托订阅和取消订阅方法。

![](img/d6410f8087ef98964329a4355c6cbc81.png)

## 游泳池经理

PoolManager 脚本对所有 get 请求进行注册和注销。

![](img/0bb27aa9e5cc1a6ec16c96606afd20a9.png)

## 敌军

在敌人脚本中，我们使用动作来删除作为目标的敌人和活动敌人列表。我们告诉 AudioManager 播放什么声音，发多少增加分数，碰撞时把伤害量传给玩家。我们使用 Funcs 从 PoolManager 获取正确的激光或武器，从 GameManager 获取玩家脚本。

![](img/e1c40e6d2758ade41b1a19405f51f879.png)

在 Init 中，我们使用 OnGetPlayerScript 来获取播放器脚本。

![](img/bbe10ae87d56ce6eb0d45c1f0f2f760e.png)

在 OnTriggerEnter2D 中，我们检查玩家在场景中是否活跃，然后调用 OnScored 发送金额以增加分数。

![](img/f734f3620170451ecf09918608054599.png)

在 FireLaser 中，我们将其更改为使用 OnGetLaser 从池管理器中检索激光。

![](img/c450da1738260901c8f2634e8963991b.png)

由于代理使用 event 关键字将他们锁定到这个脚本，我们需要为继承的脚本创建方法，以调用它们从 PoolManager 获取激光或武器，并将声音效果发送到 AudioManager。

![](img/29687ba19c2aafd5d99eeaeef9e92d50.png)

在 DisableRoutine 中，我们使用 OnDestroyed 来发送脚本的游戏对象，以将其从目标和活动敌人列表中删除。

![](img/88c487be09f4235449c1995a31b6af83.png)

随着敌人的产生和被消灭，他们会被添加和删除。

![](img/8242584bd5ee483236f9630a6e4f1786.png)

## 运动员

在玩家脚本中，我们将使用动作来告诉相机摇动并更新 UI 中的生命图像、推进器栏、弹药计数和导弹计数。将玩家从敌人的导弹目标上移开，告诉游戏管理器玩家已经死了，并传递一个音频剪辑由音频管理器播放。这些函数将用于从池管理器中获取激光、武器和爆炸。

![](img/3d600442f198b646088c926ace87d33e.png)

在 Init 中，UI 更新被更改为相应的动作。

![](img/9952628b0a506f058f7b0d2ca5608d77.png)

在 Update 中，我们将其更改为使用 OnGetWeapon 从 PoolManager 中检索导弹，并用 OnUpdateMissiles 更新导弹 UI。

![](img/7748ed2d8f3e447034243df88b421b45.png)

在 FireLaser 中，我们将其更改为使用 OnGetLaser 进行三倍拍摄，使用 Laser 从池管理器中检索激光，并使用 OnGetWeapon 进行全向拍摄。

![](img/57620c76e73656aef4689c4807e7b286.png)

在 ChangeLives 中，当玩家受到伤害时，我们将其改为调用 OnCameraShake，并用 OnUpdateLives 更新 UI。

![](img/98f160a68e9ec9ebfc8f6512dfa30a37.png)

我们改变了现有的方法，使用动作来更新 UI，创建一个方法来返回玩家游戏对象，并创建一个方法来在游戏获胜时禁用玩家脚本，这样玩家就不会受到伤害或移动了。

![](img/2e43340a64aef331f6a68259c80621c0.png)

在 OnEnable 和 OnDisable 中，我们为其他脚本的委托订阅和取消订阅方法。

![](img/9a3d05b826d8b49c1394db8a5b140f0a.png)

在 UIManager 和 CameraShake 脚本中添加注册和取消注册之后，游戏就像以前一样工作，但是玩家不会抓取每个人的脚本组件。

![](img/297af47a1895e7f6da4c4ce384634564.png)

## 自动寻的导弹

在 HomingMissile 脚本中，我们会将其更改为请求活动敌人的列表，损坏玩家而不从碰撞的对象中检索玩家脚本，并在被破坏时从池管理器中获得正确的爆炸。

![](img/1003b4790b39bad287bfe6e8e10d59df.png)

在 AssignEnemyTarget 中，我们将其更改为使用 OnGetTargetList 向 SpawnManager 请求活动敌人的列表。

![](img/8689dd3d4662366e5818ec4e5b499b13.png)

在 OnTriggerEnter2D 中，我们将其改为使用 OnDamagePlayer 来伤害播放器。

![](img/8adfc4000e5b8ca849ed980109402ee9.png)

在 OnEnable 和 OnDisable 中，我们为敌人和玩家销毁的代理订阅和取消订阅 RemoveTarget 方法，并将其更改为使用 OnGetExplosion 从池管理器中检索正确的爆炸。

![](img/d1cd8953661b607df8121099636710a4.png)

现在导弹像以前一样工作，但是使用了代理。

![](img/b13c09ecb2aad558aff2c71c6dbbe3b8.png)