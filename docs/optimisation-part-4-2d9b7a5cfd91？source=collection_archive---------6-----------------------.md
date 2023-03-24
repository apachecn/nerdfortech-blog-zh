# 优化，第 4 部分

> 原文：<https://medium.com/nerd-for-tech/optimisation-part-4-2d9b7a5cfd91?source=collection_archive---------6----------------------->

## 论执著的错误

![](img/496ce90f6e3dfdbce44d3baaf000144e.png)

加布里埃尔·索尔曼在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

以前，我曾经写过向 GPXmagic 添加一个简单的四叉树。这使得一些操作在时间上(大致)与点数的对数成正比。

我的基本结构仍然是 GPS 跟踪点列表。为什么不呢，那是骑自行车或者走路的体验。你通过一个点，然后另一个点。

尽管简单，所有证据表明，该程序与几万个点斗争。四叉树只是暂时缓解了症状。这也是一个明确的信号，天真的结构并不总是最好的，也不明显。

我很早就做了一个错误的决定。我给每个点一个数字索引，并把它放入列表中。似乎有道理；给定一个点，我知道它的索引，给定一个索引，我可以找到这个点。我还将`distanceFromStart`放入每个点的数据中。这太棒了，我可以很容易地获得相关信息。我出于良好的动机添加了`directionChange`，因为发现明显的偏差是平滑过程的关键。

这些选择和类似选择的结果是，加载文件需要多次遍历点列表，代码片段如下:

```
firstPassSetsForwardLooking =
        List.map2
            deriveForward
            preFilterDuplicates
            (List.drop 1 preFilterDuplicates)
            ++ List.drop (List.length preFilterDuplicates - 1) preFilterDuplicates
```

这些都是无伤大雅的。这是增长的代价，并且随着名单的增长而越来越大。不明显的是这段代码幕后的内存分配、释放和垃圾收集的数量。甚至那个`List.length`也是整个名单中的一员。

另外值得注意的是`index`和`distanceFromStart`是**坏**。如果我删除了路线中的第二个点，我必须更新所有剩余的点。这明智吗？不。这是可以避免的吗？让我们找出答案。

我用 304000 分的文件做了一个“快速”测试:

> *版本 2.7.13 : <测试坏了 Chrome，无结果>
> 版本 2.8.1:约 4 分钟，内存使用不可用*

(测试破坏工具时很难得到数据。)

最后，本着在 V3 上工作的心态，我决定尝试一些不同的东西。我在考虑使用一些混合的跳过列表或差异列表。一天晚上，躺在黑暗中，我决定尝试一个简单的二叉树，每个节点都增加了从其子节点聚合的关键数据。

```
type alias RoadSection =
    -- A piece of road, in local, conformal, space.
    -- Can be based between two 'fundamental' points from GPX, or an assembly of them.
    { startsAt : LocalPoint
    , endsAt : LocalPoint
    , boundingBox : BoundingBox3d Meters LocalCoords
    , trueLength : Quantity Float Meters
    , skipCount : Int
    } type
    PeteTree
    -- Absurdly simple tree may work.
    = Leaf RoadSection
    | Node
        { nodeContent : RoadSection
        , left : PeteTree
        , right : PeteTree
        }
```

有趣的是。对于一个点没有`index`，但是通过计数`skipCount`我们可以从它的索引中导出一个点，或者一个点的索引。如果我删除一个点，我只需要更新它在树中的祖先节点，而不是所有剩余的点。同样，`trueLength`代替了`distanceFromStart`。

> 当然，皮特，建造这个建筑需要很长时间。您基本上是将叶节点所需的空间增加了一倍(如果您知道如何对一个几何级数求和)。

嗯，答案花了点时间。为了公平测试，我需要能够加载文件，填充树结构，以类似于 v2 的图形呈现它，通过滑块或单击图像提供基本导航。这些证明了我们的索引机制是有效的，并且我们可以在没有四叉树的情况下有效地搜索树。

我们可以使用该结构在任意深度进行渲染，因此从一开始就实现了选择性渲染，给出了当前点周围的更多细节，而没有 v2 中涉及的诡计。

> *版本 3 测试:4.5 秒，265MB 堆*

什么？对，4.5 *秒*，不是分钟。根据 Chrome 开发者工具，其中只有 1.5 秒是我的脚本，所以(根据推断)3 秒只是加载文件内容。我不得不请一个朋友检查窗户。同样的结果:不到五秒钟 333000 分。

当然，这是一个相当根本性的改变，所有的编辑功能都需要重写。我很抱歉吗？不会吧！

不要因为你早期的决定看起来是无辜的，就坚持不放。