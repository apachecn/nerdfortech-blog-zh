# 适时优化，第 2 部分

> 原文：<https://medium.com/nerd-for-tech/timely-optimization-part-2-ce5f3bdf6636?source=collection_archive---------8----------------------->

## 扰流板:7x 加速，工作两小时

![](img/d0d099ab4e4eeed8919d1f96853b1fa7.png)

照片由 [Marc-Olivier Jodoin](https://unsplash.com/@marcojodoin?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

在上一篇文章中，我谈到了切换到一种数据结构，这种数据结构旨在为最常见的访问提供更好的支持。在这种情况下，是一个四叉树，用于使用 XY 坐标中的边界框对二维对象进行索引。

我们从 O[n]列表搜索到 O[log n]树查找。通过将一些查询工作转移到数据结构遍历中，而不是将一个集合返回给调用者并在那里进行工作，这实现了一个有价值的改进。

通常情况下，这引发了一种认识，即类似的技术可能会加速“地形”的创建，我们使用“地形”在更实际的环境中指示道路。

地形生成的工作原理是递归地对 XY 空间进行细分，并在每一个阶段推导出该区域的最低点，这样我们就可以渲染一直位于道路下方的地形。我花了一些时间思考是否可以将现有的树结构转换成景观，但这不够灵活，因为地形生成会“适应”道路的实际位置。

尽管如此，我们仍会为每个分部发出查询。很像前面的例子，它返回一个点的列表，只为我们计算最低的海拔和聚合边界框；总的来说，这些驱动地形。我们可以将这个计算推入数据结构中，以避免传递回结果集吗？

是的，我们当然可以，但这是`fold`而不是`filter`。

```
**queryWithFold** :
    **SpatialNode contentType units coords** -> **BoundingBox2d.BoundingBox2d units coords** -> (**SpatialContent contentType units coords** -> **accum** -> **accum**)
    -> **accum** -> **accum
queryWithFold** current queryArea folder accumulator =
    case current of
        **Blank** ->
            accumulator

        **SpatialNode** node ->
            let
                **fromThisNode** : **List** (**SpatialContent contentType units coords**)
                **fromThisNode** =
                    node.contents |> List.filter (*.box* >> BoundingBox2d.intersects queryArea)
            in
            if node.box |> BoundingBox2d.intersects queryArea then
                List.foldl folder accumulator fromThisNode
                    |> queryWithFold node.nw queryArea folder
                    |> queryWithFold node.ne queryArea folder
                    |> queryWithFold node.se queryArea folder
                    |> queryWithFold node.sw queryArea folder
            else
                accumulator
```

我们用调用方提供的`folder`函数扩展了`query`调用，将查询结果“动态”聚合到最终的`accumulator`参数中。

这是否让我们的调用代码变得很模糊？我想没有。

```
**initialFoldState** : **TerrainFoldState
initialFoldState** =
    { minAltitude = Quantity.positiveInfinity
    , resultBox = BoundingBox2d.singleton centre
    , count = 0
    }

**queryFoldFunction** :
    **SpatialContent IndexEntry Meters LocalCoords** -> **TerrainFoldState** -> **TerrainFoldState
queryFoldFunction** entry accum =
    { minAltitude = Quantity.min accum.minAltitude entry.content.elevation
    , resultBox = BoundingBox2d.union accum.resultBox entry.box
    , count = accum.count + 1
    }

{ **minAltitude**, **resultBox**, **count** } =
    SpatialIndex.queryWithFold index myBox queryFoldFunction initialFoldState
```

这三件事是什么？首先，一个用于积累结果的小数据结构。其次，做积累的功能；为每个结果调用。第三，对`queryWithFold`的实际调用。我认为这是对工作的一种相当简洁的表达，一种令人愉悦的分工，以及容易获得的结果。几乎不需要任何其他的改变。

值得吗？早餐后，我花了大约一个小时编写`queryWithFold`函数，晚上，我花了大约一个小时编写调用代码，同时观看烘烤决赛。我的测试用例有 23000 个跟踪点，渲染地形需要大约 7 秒钟。这下降到只有一秒钟左右。

这就是我喜欢的优化方式。有针对性，及时，有效。