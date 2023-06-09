# 在 Unity 中移动平台

> 原文：<https://medium.com/nerd-for-tech/moving-platforms-in-unity-23612a8aae23?source=collection_archive---------16----------------------->

对于任何平台风格的游戏，我们都需要为平台创造某种类型的运动，这样玩家就可以猜测哪个时间是他们跳跃的最佳时机。首先，我们需要创建一个新的工作平台，并创建我们的平台将在以下地点之间行驶的路径点:

![](img/e5050a3f95ddcabbbaa48d717de46489.png)

随着我们的平台被组织在一个新的空对象中，我们将把我们的航路点放在它们自己的部分中，但是为每个移动的平台贴上标签。这是因为如果我们将它作为移动平台的子平台，平台将永远移动，因为当它移动时，路点也会移动。从这里，我们将创建一个新的脚本，并开始编写运动所需的代码:

![](img/1b9cac5f811a24519a7137bd32faf663.png)

我们在这里所做的是分配我们的平台从第一个位置开始，并从那里分配一个向量 3。平台的 Movetowards 指令。为了不让我们的平台因为想回到 a 点而在 1 点反弹，我们必须创建一个 bool。为了让这个 bool 改变它的值，我们将告诉 Unity，一旦它到达它的目的地，我们希望它改变它的状态，以便我们可以切换回另一个方向。有了这个，我们就可以看到它在我们的游戏中是如何工作的了:

![](img/9427c8d5aab126c30d5d5ddaf7a20a80.png)

现在我们的平台来回移动，让我们看看当它跳到平台上时，它是如何与我们的玩家交互的:

![](img/d6d4e39863c98506f87898a70f36f218.png)

我们的玩家可以正确地着陆并沿着平台移动，但是我们需要自己移动玩家来停留在平台上。在这种情况下，我们将希望让我们的玩家与平台一起移动，而不必这样做，所以让我们看看如何才能解决这个问题。
我们将从创建一个盒子碰撞器开始，它将在我们的平台之上进一步扩展:

![](img/4203e6a9ff0eabe5f72bc41594501ee5.png)

从这里，我们将创建一个 OnTriggerEnter 脚本:

![](img/726508513f46aed932d27d2461bd361c.png)

我们能够对脚本做的是移动我们的玩家，使其成为我们平台的孩子，允许我们以与我们的玩家相同的速度移动:

![](img/5e62d2bf080598754d8bc11f87f36613.png)

但是，正如我们在上面看到的，我们需要创建一个方法，让我们的玩家从作为父代的移动平台断开连接:

![](img/9b8c4d0ed729df8673c5213f94fe8856.png)

有了这个空白，我们将让我们的玩家离开平台，允许我们自己移动，而不是被拖到平台上:

![](img/7b6dc4898f3ab5ce4f7e36601891a409.png)

随着移动平台的完成，我们现在可以期待加入一个生命系统，这样我们就不必在错过一次跳跃时不断重启或替换我们的玩家对象。