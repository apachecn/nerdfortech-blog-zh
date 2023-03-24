# 每日进度——让玩家跳跃

> 原文：<https://medium.com/nerd-for-tech/daily-progression-getting-the-player-to-jump-44ac14b8b65b?source=collection_archive---------19----------------------->

上次我们用角色控制器给玩家编程跳跃。这一次，我们将使用刚体 2D 组件。我们需要检查玩家是否被禁足，但是让我们从简单的开始。让玩家跳起来。

![](img/669da2d4d2f5815605a9e9758c193c69.png)

在做了一些初步的研究后，似乎 AddForce 是正确的选择，AddForce 也有多种模式:加速、力、脉冲和速度变化。冲动似乎最有意义，所以让我们看看我的选择是否正确:

![](img/028ebf686cd9076b5b2ec4f657b0eae8.png)![](img/c57c7e28890935d0adf2656ace5f307c.png)

这只是跳跃的一种方式，另一种方式是改变刚体的速度:

![](img/22fed212542d83d25b95f133dcd67c65.png)![](img/387d177bf1f3f673253812a49dba14e6.png)

看起来不错！但是玩家可以想跳多少次就跳多少次，我们可以限制在一次，但是这给我们带来了下一个挑战:知道我们什么时候被禁足。为此我们需要使用光线投射。

游览 https://docs.unity3d.com/ScriptReference/Physics2D.Raycast.html 应该会给我们需要的信息:

![](img/145a123488daea5a7b07f1af9de73d18.png)![](img/8c8de65d014528f4d0d4853192760d52.png)

所以当使用 RaycastHit2D 时，我们需要一个原点，一个方向，一个距离，以及一个 RaycastHit2D 类型的变量。

我要创建一个新的函数，CheckIfGround，我们也会在更新中调用它。

![](img/110c11f1ba83baef8cbea853225282cb.png)![](img/a54b85e891597a369acfe82b05f9e8bf.png)

我们还会在跳转中添加一个复选标记:

![](img/2d36050a6b8dbaa0ff478fa0cf505508.png)![](img/746c3a38d1cd6254fad605594ebc2ec0.png)

如果我们现在尝试，我们将不能跳跃，因为光线投射很可能探测到玩家。问一下就能看出这一点。

![](img/8e7ef645a136d817d938f45b3c3873af.png)![](img/f029147587efa3c0cbb968c41ed69dbb.png)

果然:

![](img/2302bc8f17a47d582ad0fada43a8fbfe.png)![](img/406867b0148c44d773a8077e91dfbbe7.png)

虽然地面瓷砖被标记为“地面”，但 Raycast 确实利用了层，所以我们可以给地面一个特殊的层名称，以便我们可以告诉 raycast 只检查该层！因此，与'地板'瓷砖选择，我药物下来层菜单创建一个新的，并在第 8 层，创建了一个新的地面层。

![](img/6e3415def815090f10868518f49526ed.png)![](img/d52d814bb084f92132cb474b656b52ec.png)

这里有一个警告，这种特殊的情况并不像它要求的那样容易，你需要使用一种叫做“比特移位”的东西才能正确工作，这实际上是一个高级计算机科学问题，人们可以获得博士学位，而不是一个 Unity 问题。我只想说，接受现实吧。

![](img/b9f660f2afbc466bde008765350350bf.png)![](img/9dfe187502ee1e1efe38088831d26b6a.png)

现在让我们检查一下:

![](img/5fdf5e616012d3bb5de3124106625054.png)![](img/e0b39276c5fa67041a35dd74fd80e33c.png)

这就是我们想要的！

一个可选的方法是为地面层创建一个 holder，我们就说 _groundLayer 作为一个示例变量名；对其使用 SerializeField 以使其在检查器中可用。然后在检查器的下拉菜单中选择图层，完成后。请使用 _groundLayer.value 代替移位后的数字。诚然，这两种方式都有点古怪，但这只是 Unity 的一部分。

顺便说一句，如果我们能看到真实的光线投射就好了。Unity 通过 Debug 再次拯救了我们。DrawRay:

![](img/66706735cd76c3d4d6d7df30f3edf6e5.png)![](img/7a8ee570c10fef00d386d76818f22095.png)

它非常类似于光线投射，所以让我们把 drawray 放在光线投射下面。

![](img/0d536189a19646a73b44414aeca34a3c.png)![](img/1ba5713152fde42854bf7f0374ee8832.png)![](img/9c28572f5c9c4ba08f0059c14cf80b50.png)![](img/90e58b5d2dfaef58eec5df31a6c55ac5.png)

现在我们可以在我们的场景中看到它！你也可以通过点击游戏区上方的小发明按钮在游戏中看到它。

也就是说，现在如果我们在检查器中检查我们的 _isGrounded 变量，我们可以看到光线投射现在正在完美地工作！

![](img/fd5c3f3bd1793baf8ac8def4209bce9d.png)![](img/c46e297a537b6c363c9c4b3c21c811e9.png)

如果你能在这一点上双跳，可能是你的光线投射太长了。随意把它缩短到几乎接触不到地面的程度。这是一个反复试验的过程，每个人都不一样。我最终将 _raycastDistance 变量改为 1.1，这看起来很好。

我们会给我们的射线检查一点喘息的空间，但是现在，我们称它为一篇文章。:)