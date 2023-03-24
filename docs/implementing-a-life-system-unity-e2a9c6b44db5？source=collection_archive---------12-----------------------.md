# 实施生命系统| Unity

> 原文：<https://medium.com/nerd-for-tech/implementing-a-life-system-unity-e2a9c6b44db5?source=collection_archive---------12----------------------->

## 统一指南

## 如何用 Unity 实现生命系统的快速指南

![](img/7f6d173b53586d419ed3d72385e85da6.png)

**目标**:实现一个系统来统一处理玩家的生活。

在上一篇文章中，我介绍了[如何用 Unity](/nerd-for-tech/dynamic-platforms-unity-28cfe83af90) 创建动态平台。现在，是时候实现一种方法来处理玩家在我们平台游戏中的生活了。

# 处理坠落

如果你看过[我的上一篇帖子](https://fas444.medium.com/)，你会记得我们正在使用原始资产开始创建一个平台游戏。目前，在下一个场景中，我们有一个带有一些平台和收藏品的播放器:

![](img/c9bde72e34aed3e362408633d7085437.png)

因此，为了处理玩家摔倒的情况，让我们从创建一个空的游戏对象开始，它代表场景中让玩家重生的点:

![](img/d3b7b003efd020ee7317d11c7db851bc.png)![](img/cff83888fcce792c0b43b349d3c8a5d0.png)

然后，让我们创建一个新的游戏对象，它包括一个盒子碰撞器组件，并启用了 ***Is Trigger*** 属性来表示玩家将失去一条生命的区域:

![](img/fd7c0e6ff7ace32c9f76fc73b36eb5f1.png)![](img/77abdf20c2ea8f1889cfdec637cea703.png)![](img/1b40a1e485ffd4f31a226c8a3b24a00e.png)

一旦碰撞器实现了我们的目的，让我们将游戏对象移动到期望的区域:

![](img/90b3d09214671e4686d3fd995365cb0b.png)

当它掉落并触发碰撞器时，玩家将失去一条生命。

现在，让我们创建一个新的脚本并附加到游戏对象上，以便处理与玩家的触发冲突:

![](img/6526210e248ddf849002ed8fbb2f56d8.png)

然后，让我们打开脚本并在顶部包含**系统**名称空间:

![](img/32400d4027e900b2c34774762c74723a.png)

接下来，让我们创建一个新的公共静态动作委托，它将代表玩家的每次摔倒:

![](img/4bde84c4b98a641f8df12f660daeaaaa.png)

如果您想了解有关动作代表的更多信息，可以访问 Microsoft 文档:

[](https://docs.microsoft.com/en-us/dotnet/api/system.action?view=net-5.0) [## 动作委托(系统)

### 封装没有参数且不返回值的方法。公共委托 void Action()；公共…

docs.microsoft.com](https://docs.microsoft.com/en-us/dotnet/api/system.action?view=net-5.0) 

现在，让我们使用 **OnTriggerEnter** 方法来触发与其他碰撞器的碰撞。在这个方法中，我们将通过比较另一个碰撞器的标签来检查碰撞是否发生在玩家身上。如果碰撞器属于玩家，我们将调用动作委托(如果不为空)在玩家摔倒时采取相应的动作:

![](img/de180500e6dc1fb4105df75a3efffab4.png)

# 生命系统

现在，为了处理生命系统，让我们打开玩家脚本并包含**场景管理**名称空间，因为我们将在玩家生命耗尽时重启关卡:

![](img/51edfdca4edd6ac4ee8b3534b9fe6ae3.png)

然后，让我们创建:

*   一个私有变量存储玩家的总寿命。
*   返回私有变量的值的公共属性。
*   存储重生点引用的私有变量。

![](img/ad32bcdb7aa11cbe1c3eb02b82e7cd35.png)

现在，让我们创建一个新的方法来处理玩家的伤害。当该方法被调用时，它将:

*   从相应的变量中减去一个生命。
*   如果玩家生命耗尽，恢复等级。
*   禁用角色控制器组件以平滑重生。
*   改变玩家的位置到重生点。
*   在一定时间后启动一个协程来启用字符控制器。

![](img/39bddc0348ae73134a5df17f065cb1ae.png)

启用角色控制器的协程是下一个:

![](img/42c18bc4f625f7782cd626e7f1b8d41b.png)

这样，我们将等待半秒钟，直到控制器再次启用。

最后，让我们将该方法添加到从死区开始的动作委托中的 **Start** 方法中，以便在玩家摔倒时调用该方法。同样，作为一个好的实践，让我们使用 **OnDestroy** 方法删除它，以避免在播放器被破坏时试图执行它:

![](img/77517efe7b461eb09541fe1baa5824f5.png)![](img/34cb70b56f735ef1d28475b0f946b5c8.png)

保存脚本后，让我们将重生点拖到检查器中:

![](img/3fb67c921165d95bd5e9bf05cb64634e.png)

# 展示生命

现在，为了显示剩余的生命，让我们在画布中使用一个文本元素，就像我们用来显示硬币的元素一样:

![](img/fc529782f1434e8a527ebe3e56bd122f.png)![](img/6d9b14a3e8fbef4ca20a97fc9a5fbcda.png)

一旦我们有了文本元素，让我们打开 UI Manager 脚本并创建一个新变量来存储对它的引用:

![](img/85edfc5a69b985387acd44a670a30814.png)

然后，让我们创建一个新方法，通过更改 text 属性并从 player 类调用 lifes 属性来更新 lifes:

![](img/3845ffdd91a9bce065011c7f88ec15be.png)

最后，让我们添加和删除动作委托中的方法，就像我们处理接收玩家类伤害的方法一样:

![](img/6a3ce6848298a3f91d17fdef03cdd868.png)![](img/e14bc3311541eb3c24e8035bb09ca84d.png)

不要忘记通过检查器将文本元素拖到相应的变量中:

![](img/a09dbb3f43536463e50c56e86e0ee187.png)

如果我们在 Unity 中运行游戏，我们会看到玩家每次倒下都会重生并失去一条生命:

![](img/4ff3367cae0ee2f77979bf254571d1a5.png)

就这样，我们用 Unity 为我们的平台游戏实现了一个生命系统！:d .下一篇文章再见，我将展示如何用 Unity 创建电梯。

> *如果你想了解我更多，欢迎登陆*[***LinkedIn***](https://www.linkedin.com/in/fas444/)**或访问我的* [***网站***](http://fernandoalcasan.com/) *:D**