# 在 Unity 中移动平台

> 原文：<https://medium.com/nerd-for-tech/moving-platforms-in-unity-14930769bd50?source=collection_archive---------22----------------------->

![](img/d262d37aa96568e9282086e3d81a128a.png)

啊，好老的移动平台！这是我们游戏记忆中如此美好的一部分，现在轮到我们来制作一些了，让我们开始吧！

首先，让我们抓取一个当前的平台，并将其重命名为 Moving_Platform，复制几次，并将副本命名为 Point_A 和 Point_B，并将 Point_B 移动到您希望平台移动的位置。

![](img/6ecfaa91073e9227eeaaf3940abe0ba9.png)

因为我们实际上只需要点 A 和点 B 的变换位置，所以继续删除除变换以外的所有内容。不幸的是，选项框被切断了记录屏幕，但只是'删除组件'在每个项目。

![](img/91c46d9d22baff30efc7b81ceb0083aa.png)

太好了！我们准备好让事情动起来了！创建一个名为 MovingPlatform 的脚本，并将其拖动到移动平台游戏对象上。

![](img/d4f78f4428f012387dc904e2e07810bc.png)

我们要做的第一件事是为 PointA 和 PointB 创建容器。

![](img/07bb602f6e77cf6f109fb3fdff3b57f0.png)

接下来，将 Point_A 拖动到 targetA，将 PointB 拖动到 _targetB。

![](img/2fd5f801d196cc0ac131ecf8bbbd086a.png)

我们有很多方法可以解决这个挑战，但对于这一个，我们将使用名为 MoveTowards 的 Vector3 方法

![](img/a7c67756b5e9d4b4c7303c619cd933d0.png)

这很简单，你有当前位置、目标位置和 maxDistanceDelta，这是一个很好的词，表示“我们想走多大的一步”或…速度。

所以我们先做一个速度变量。

![](img/3ef54f395b01c2611369f24bb028c7f6.png)

太好了！让我们将 target_b 作为我们的目标，并将这个命令放在 update 方法中。

![](img/f88b566fa2a70d6e3e8666ff3775005d.png)

所以我们取当前的 transform.position，然后指定 Vector3。使我们从当前位置移动到 now _targetB 乘以 _speed * Time.deltaTime 的位置的 move origing 方法。

我们去看看吧！

![](img/97e80beeac455d2ae230a7b5905495c9.png)

它还活着！嗯，它在动…现在我们需要一些乒乓球动作。

经过一番思考，我发现让平台从一个点切换到另一个点的最直接的方法是使用 _currentTarget 变量，我们不断地用它切换 targetA 和 targetB，并不断地向 _currentTarget 移动。

![](img/b210720d7d85b4966d624cd203129d3e.png)

我们在 Start()中将它设置为 PointB。

![](img/167a835e0ef576107a1b660f0d0eb888.png)

让我们将 Update 中的当前代码移到一个名为 MoveTowardsCurrentTarget()的新函数中。

![](img/49113185369f4270c4e2c575906ce44d.png)

我们将把 _targetB.position 改为 _currentTarget.position。

![](img/d7a71545cd255b62f64b700b9587d6cc.png)

让我们在 Update()中调用 MoveTowardsCurrentTarget，这样它就会一直发生。

![](img/179f45f3351133cdd421a32d7c5c8491.png)

如果我们现在运行它，它将完全按照以前的方式运行，只是我们已经为切换打下了基础。让我们创建一个名为 CheckIfTargetReached()的新函数

![](img/530b81f8965d46f4b46b0e8c4d5f2765.png)

这是我们编程中最难的部分。你准备好了吗？先用伪代码吧！

![](img/6a9bf04b9c1af2693e143c94b5d0112d.png)

相当简单！让我们一行一行地做:

![](img/9b1619eb2a9667bbf78fe130f6673f44.png)

我们需要改变当前位置，我们有一个名为 _currentTarget 的变量…

![](img/3190d34d2643a68ee34c5ae505c107bf.png)

就是这样！由于 Update 总是在运行，所以代码会自行处理，一旦它到达 targetB 的位置，当前的目标将会改变，平台将自动开始移动到 targetA！但现在我们需要对目标 A 进行检查？你说得对！

![](img/e175b8d422b0f317bdee56c6205fecb9.png)

如果你现在运行游戏，它不会工作，因为我们需要这个功能在更新，所以它也在每一帧运行！

![](img/012dd70472c1d052ca2fc8f26f2206d7.png)![](img/ea7483bfd812a7543edc53bfa8652df0.png)

相当干净和可读的更新，不是吗？总是努力让你的代码易于理解。你这样做不仅是为了别人，也是为了你自己。

![](img/7586015071e895b4391664290612eb82.png)

让我们跑到月台上车吧！玩家死了，因为他只是呆在一个地方，没有“骑”上平台。这是我们明天要解决的问题！到时候见！