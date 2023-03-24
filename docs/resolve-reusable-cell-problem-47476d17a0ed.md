# 解决可重用单元问题

> 原文：<https://medium.com/nerd-for-tech/resolve-reusable-cell-problem-47476d17a0ed?source=collection_archive---------0----------------------->

![](img/27e8cafff3802392903b0ff521726c5e.png)

你好，读者！

今天我想讨论一个大家都很熟悉的问题…那就是在一个 **UITableView** 或 **UICollectionView 中重用不同类型的 **UITableViewCell** 或 **UICollectionViewCell** 。**

我见过多种方法，从差的(我认为它们是差的)依赖于 ***if*** 或 ***switch*** 语句，并检查实际上应该重用什么索引，然后**根据该索引将**转换为类型，到更好的方法，其中出列被很好地分开。

糟糕的方法是这样的:

![](img/1f9420b49e5d24dfc78cbd5da343cf24.png)

每当我看到上面这样的代码时，我的表情

我有了一个想法:“这不可能是实现目标的唯一方法…”。

所以我开始在网上搜索不同的解决方案。我发现了非常好的解决方案，所以我不明白为什么开发者仍然使用这种非常不好的方法？我想到的唯一答案是因为这是**简单**和**快写**！
你知道什么指标会是具体的那种类型那么为什么不这样做呢？

我觉得这有点 ***【懒惰做法】***

这段代码的问题是它不容易修改，所以维护它是一件痛苦的事情……它很难维护，并且不允许我们重用一些特性。

在本文中，我想向您展示一种基于简单协议将不同种类的单元出队的方法。我想向您展示这种方法的主要概念，也许它会帮助您解决项目中的一些问题。

让我们开始吧:

## **首先:**

创建一个解决上述情况的方案:

## **第二:**

声明我们的 **UICollectionView** 类:

到目前为止，我们已经有了 **CollectionView** ，当 indexPath.item 为偶数时显示 **FirstAmazingCell** ，否则显示 **SecondAmazingCell** 。

是时候使用我们的**collectionviewdequeue protocol**来解开这个条件了。让我们添加一个方法来注入它，并将出列单元方法和 numberOfItemsInSection 的主体替换为 CollectionView 模型，该模型将包含我们的协议的实例:

我们现在需要一些数据来检验它是否有效。为此，我将使用我最喜欢的迷因角色之一[佩佩悲伤青蛙。](https://en.wikipedia.org/wiki/Pepe_the_Frog)

![](img/b153708c31e655b33d45a76dbfbc112d.png)

我已经在资产中添加了三个不同的 Pepe 图像，并模仿一些服务层来获取包含这些图像的数据以及应该显示多少元素。

**我的模拟:**

我将在接下来的步骤中做什么:

*   创建三个 **UICollectionViewCell** 类来显示正确的佩佩图像，
*   创建一个构建器，该构建器将初始化三种不同类型的实例**集合视图出列协议**，
*   将这些实例传递给 **CollectionView。**

**建造者:**

现在将所有这些收集在一起并展示:

现在，如果我们运行上面的代码，模拟器应该显示佩佩。一个例子:

![](img/2d1fdd0b3b5c34ad2d4beecc7e1e2fa8.png)

我们还可以做更多的事情:

在`[func collectionView(UICollectionView, cellForItemAt: IndexPath) -> UICollectionViewCell](https://developer.apple.com/documentation/uikit/uicollectionviewdatasource/1618029-collectionview)`中，我们需要做的只是尽快返回出列的单元。设置 **CollectionView** 项将在 UICollectionViewDelegate 方法中进行:

`[func collectionView(UICollectionView, willDisplay: UICollectionViewCell, forItemAt: IndexPath)](https://developer.apple.com/documentation/uikit/uicollectionviewdelegate/1618087-collectionview)`。这一变化使得我们的单元在出队和显示时性能更好。

让我们为此创建另一个协议:

更新我们的课程:

**集合视图:**

**建造者:**

仅此而已。搞定了。

# 结论:

*   很好地将出队和项目设置与数据源分开，
*   **UICollectionDataSource** 和 **Delegate** 现在可以重用，
*   现在我们可以很容易地扩展新功能，
*   单元格与**uicollectionview data source**或 **Delegate、**之间没有紧密耦合
*   每个项目都有一个构建器，因此我们可以改变它的实现，而不用担心我们在其他项目的实现中改变了一些东西。

感谢您花时间阅读本文。

祝你好运！

完整代码:[https://github.com/Rafal-Prazynski/ReusableCell.Medium](https://github.com/Rafal-Prazynski/ReusableCell.Medium)