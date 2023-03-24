# 创作收藏品| Unity

> 原文：<https://medium.com/nerd-for-tech/creating-collectables-unity-ccb59ea0c191?source=collection_archive---------7----------------------->

## 统一指南

## 关于如何使用 Unity 创建收藏的快速指南

![](img/5ef3009ab8ac72996ae639a1c0136ab8.png)

**目标**:创建收藏品，并实施统一收集系统。

在上一篇文章中，我介绍了[如何用 Unity](/nerd-for-tech/implementing-a-double-jump-unity-87a1e55e4e72) 实现双跳。现在，是时候实现一些收藏品和显示我们的平台游戏总数的方法了。

# 收藏品

为了表示收藏品，我们将使用原始资产。正如你在下一张图中看到的，我们在平台上方放置了 3 个球体来代表一件可收藏的物品:

![](img/8631faa2e6e4463054cc21a262f9690e.png)

# 实现收集机制

然后，为了实现收集机制，让我们创建 2 个新脚本:

*   可收集的

来处理这个可收集的技工。

*   UIManager

处理将显示已收集项目总数的用户界面。

![](img/c719a2d858cd99a84a5429fa6573f8f0.png)![](img/b1edd66c235711fbb60f7187dd372f32.png)

现在，让我们确保每个球体:

*   在碰撞器组件中启用了 ***是否触发*** 属性
*   包含一个刚体(不使用重力)
*   会附加相应的脚本

![](img/1ee553e92211f8f440f7b87e55328fcf.png)![](img/b9cb48fd7ab3b7d86e3e38ca6e0915bb.png)

然后，让我们创建一个新的文本元素来显示有多少收藏品(硬币、电源等。)难道玩家已经收集了:

![](img/c0214a8719066c8a2f5e52a41aced5fa.png)![](img/955a543f9b35ed38e9ad4a2a2a018088.png)

同样，让我们将 **UIManager** 脚本附加到画布上，这样我们就可以用脚本处理用户界面:

![](img/ac79b1c67332462a68d6d691b74c5c74.png)![](img/7aa02b76ffbc9e17f529dcd27b40ffcf.png)![](img/8027b0d2a1971f999e315841c8757b58.png)

接下来，让我们打开可收集的脚本并添加下一个名称空间，这样我们就能够使用 [C#动作委托](https://docs.microsoft.com/en-us/dotnet/api/system.action?view=net-5.0)来处理收集机制:

![](img/c27b4a73ad8f9d28400fb0723894eb29.png)

然后，让我们创建一个公共静态动作委托，在收集硬币时在其他脚本中执行相应的动作:

![](img/915abc2b375bd71a3f6c42973bd74005.png)

现在，为了实现收集机制，我们将使用 **OnTriggerEnter** 方法并检查另一个碰撞器是否属于玩家。如果是这种情况，让我们调用动作委托(如果它对于最佳实践来说不为空),然后销毁可收集的对象:

![](img/6629ff2464f318833794e3da85f8a21e.png)

> 注意:玩家必须贴上 ***玩家*** 标签，以便与收藏品发生碰撞时识别。

一旦在 collectable 类中创建了**动作委托**，让我们打开**播放器**脚本并添加:

*   存储硬币数量的私有变量
*   仅返回私有变量的值的公共属性

![](img/7f88cd21f2a30484e2eb74c500001cbb.png)![](img/dad3c24d309721a7b130f37dee329e10.png)

然后，让我们创建一个新方法，它将在每次被调用时添加一个硬币:

![](img/3e9a84bcab0072cee87f94a134e81c00.png)

现在，让我们将该方法添加到来自**collectible**类的**静态动作委托**中，以便能够在每次收集硬币时调用它:

![](img/9b7f40f0e7cd08adb1733647dfd8a380.png)

最后，作为一个好的实践，当玩家使用 **OnDestroy** 方法被破坏时，让我们从动作委托中移除该方法:

![](img/aef3f73e477586f36849c11644992bc1.png)

# 显示总数

现在，为了显示玩家收集的硬币总数，让我们打开 **UIManager** 类并添加 **UnityEngine。UI** 命名空间能够处理来自代码的 UI 元素:

![](img/fbd62e3fae789f3d56d75bd40ccb095e.png)

然后，让我们创建变量来:

*   存储对 UI 文本元素的引用
*   存储对播放器脚本的引用

![](img/1c97c8bbb2a1ba459ccc31dfde662990.png)

> 注意:很可能我会将硬币的数量存储在一个游戏管理器中，而不是存储在玩家本身中。现在我将存储一个对**玩家**脚本的引用。不要忘记使用**【serialize field】**通过检查器设置引用。

接下来，让我们通过检查器获取对 UI Manager 组件的引用:

![](img/0985d13e6074ae008e79d8f3b062f581.png)![](img/c17b00cf1221d42c4a56bbd03995112f.png)

现在，让我们创建一个新方法来更新显示硬币总数的文本元素。这将通过调用**播放器**脚本的属性来修改文本:

![](img/6c73b5ec492ae23678415a595e16c9c9.png)

最后，让我们将各自的方法添加到来自**collectible**类的 **Action Delegate** 中，这样每次收集硬币时文本元素都会更新。另外，不要忘记使用 **OnDestroy** 方法移除该方法:

![](img/1d997221a2e41dd30f222df4314322b6.png)![](img/cac12ebafba8edbe1085ad2b90511314.png)

如果我们用 Unity 运行游戏，我们将能够收集球体，UI 元素将显示收集的总数:

![](img/d7c5ba2c7894daba0a4f7f0d4b90a318.png)

就这样，我们用 Unity 创造了可收藏的物品！:d .我会在下一篇文章中看到你，在那里我将展示如何使用 Unity 为我们的平台游戏移动平台。

> *如果你想了解我更多，欢迎登陆*[***LinkedIn***](https://www.linkedin.com/in/fas444/)**或访问我的* [***网站***](http://fernandoalcasan.com/) *:D**