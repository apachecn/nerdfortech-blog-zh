# 理解纪念品设计模式

> 原文：<https://medium.com/nerd-for-tech/understanding-memento-design-pattern-5c4f09be639?source=collection_archive---------6----------------------->

![](img/91dc418681b4b9b3445db991dabaa655.png)

吉姆·威尔森在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Memento 模式(也称为快照模式)是一种行为设计模式，用于保存和恢复对象以前的状态。如果你想开发一个具有撤销或回滚功能的应用程序，你应该使用 Memento 设计模式。大多数软件开发人员在他们希望在应用程序中开发这种功能时都会使用这种模式。

理解 Memento 模式概念的一个简单例子是 Windows 还原点系统。在 Windows 中，我们创建还原点，因为如果发生故障或系统崩溃，我们可以使用这些还原点将操作系统恢复到以前的状态。

Memento 设计模式有 3 个组成部分，称为，**发起者、管理者和 Memento。**

**发起者:**我们需要维护状态的对象。基本上，发起者对象创建一个 memento 对象来存储它的内部状态。因此，发起者对象知道如何保存和恢复自身。

**看守者:**跟踪发起者的对象。基本上，Caretaker 知道发起者需要保存和恢复自身的原因和时间。

**Memento:** 包含基本状态存储和检索功能的对象。通常，Memento 对象是不可变的，只通过构造函数传递一次数据。

![](img/b8f0106a9453c1b42d96a230d82ca533.png)

图像:纪念品模式 UML 表示(dotnettricks.com)

当实现 Memento 设计模式时，发起者与 Memento 耦合，并将状态传递给看守者。所以，每当我们需要回到以前的状态时，我们必须与管理员交谈，检查以前的状态，然后移动。

因此，在下一章，我将通过一个使用 Java 的真实例子来解释如何应用 Memento 设计模式。

# 纪念品设计模式的用例

假设有一个体育俱乐部，需要一个软件来跟踪他们的球员的排名。玩家排名会根据两个事实来衡量:**【表现】**和**【健康状态】**因此，体育俱乐部在每次训练课中给运动员排名时都会考虑这两个因素。

在一次训练中，运动员的表现分数最高可达 100 分，最低为 0 分。健康状态有两种不同的状态，分别命名为:**“健康”**和**“不健康”**如果玩家在健康状态为“健康”的情况下表现良好且得分超过 80 分，该玩家将在该环节获得**“优秀”**等级。此外，还有 4 个等级，称为**好、一般、差、**和**非常差。但是所有这些分数、健康状况和排名只对特定的训练有效。每当他们在另一天开始一个新的会话时，玩家将被分配新的标记和状态。**

此外，运动俱乐部需要一个功能来撤销会议的进展。因为在某些情况下，教练可能需要撤销最近的课程细节并返回到以前的课程。特别是当一名球员药检呈阳性，或者他们有任何违法行为时，他们的参赛资格将被取消。因此，教练必须根据特定的场景回滚他们的进度。

现在让我们看看这个例子的实现。

## 第一步:

作为第一步，我创建了一个名为**“Rank”的类。**在这个类中，我定义了 3 个名为“性能”、“健康”和“等级”的字段此外，我还声明了一个名为 **"rankGenerator()"** 的方法，它负责根据玩家的表现和健康状态来生成玩家等级。*参考以下要点。*

此外，我还创建了一个枚举来保存健康状态类型。

## 第二步:

第二步，我创建了作为发起者的**“Player”类和作为纪念品的**“RankMemento”类。**在“Player”类中，我实现了一个名为**“addRank()”**的方法，将玩家的等级细节添加到一个数组列表中。然后我实现了一个名为 **"getRankList()"** 的方法，从数组列表中返回一个克隆的对象。使用“clone()”方法的原因是，防止引用原始对象。**

在这个应用程序中，Memento 类(RankMemento)是在 Originator 类(Player)中实现的。但是如果你愿意，你可以分别实现这两个类。此外，在同一个 Originator 类中，我创建了两个特定的方法，分别命名为**“save rank()”**和**“revertRank()”。**

“saveRank()”方法负责将玩家的状态交给**看守者(RankHistory)。**因此，它通过从数组列表中传递一个克隆对象来返回“RankMemento”的实例。

“revertRank()”方法负责恢复当前状态。

## 步骤 03:

现在我已经实现了**“rank history”类作为看守者。**这个类负责跟踪“RankMemento”类。在“RankHistory”类中，我创建了一个堆栈，以后进先出(LIFO)的顺序存储“RankMemento”类的对象。因此，当我们使用“reverRank()”方法时，我们可以将最后一个对象保存在堆栈中。*参考以下要点。*

## 步骤 04:

最后我们可以运行我们的应用程序来检查输出。*参考以下要点。*

输出:

```
Player{rankList=[Rank{, rank='Excellent'}, Rank{, rank='Good'}]}Player{rankList=[Rank{, rank='Excellent'}, Rank{, rank='Good'}, Rank{, rank='Very Poor'}]}Player{rankList=[Rank{, rank='Excellent'}, Rank{, rank='Good'}, Rank{, rank='Very Poor'}, Rank{, rank='Poor'}]}
```

正如您在要点中看到的，教练添加了 2 个训练课程细节(第 9 & 10 行)并使用“saveRank()”方法保存了这些细节(第 13 行)。假设那一周有两次训练。在接下来的一周内，有另一个会议，他也必须添加这些细节(第 16 行)。他再次使用“saveRank()”方法，并在另一周重复同样的事情。

正如你在输出中看到的，玩家排名是根据传递的参数生成的，到目前为止系统运行正常。现在让我们看看如何使用“revertRank()”方法来恢复这些。

```
Player{rankList=[Rank{, rank='Excellent'}, Rank{, rank='Good'}]}Player{rankList=[Rank{, rank='Excellent'}, Rank{, rank='Good'}, Rank{, rank='Very Poor'}]}Player{rankList=[Rank{, rank='Excellent'}, Rank{, rank='Good'}, Rank{, rank='Very Poor'}, Rank{, rank='Poor'}]}**------------------Start Reverting------------------**Player{rankList=[Rank{, rank='Excellent'}, Rank{, rank='Good'}, Rank{, rank='Very Poor'}, Rank{, rank='Poor'}]}Player{rankList=[Rank{, rank='Excellent'}, Rank{, rank='Good'}, Rank{, rank='Very Poor'}]}Player{rankList=[Rank{, rank='Excellent'}, Rank{, rank='Good'}]}Nothing left to UndoPlayer{rankList=[Rank{, rank='Excellent'}, Rank{, rank='Good'}]}
```

正如您在输出中看到的，即使我们调用了“revertRank()”方法，它看起来一开始也不会产生影响。然而，其他方法调用如预期的那样工作。那么，第一个方法调用有什么问题呢？

这里发生的是，我们给玩家增加了一个新的等级，并把它给了管理员。之后，我们在第 28 行调用了“revertRanked()”方法。这意味着我们要求管理员给我们一个玩家以前的等级信息。因此，Caretaker 所做的是，它会给我们最后保存的等级细节，因为“revertRanked()”方法从堆栈中返回它。因此，这个返回的对象(rank details)正是我们在调用 revert 方法之前保存的最后一个对象。这就是为什么我们在上面的输出中看到这个结果的原因。因此，我们需要**移除第 23 行**上的“saveRank()”方法调用，它将按预期工作。

此外，如果您不明白为什么输出会打印出**“Nothing left to Undo”**，尽管它还有 2 个条目(排名)。这仅仅是因为它与单个项目无关，而是与版本/集有关。如果你一个接一个地把它们交给看守者，它们会一个接一个地被还原。但是如果你一次保存几个项目(排名)，这些将被还原为一个版本/集合。

如果您感兴趣，可以使用下面的 GitHub 链接来查看我对 Memento 设计模式的完整实现。

[](https://github.com/nisal-kumara/krish-lp-training/tree/master/Design%20Patterns/Memento) [## 尼萨尔-库马拉/克里希-LP-培训

### 在 GitHub 上创建一个帐户，为 nisal-kumara/krish-lp-training 的发展做出贡献。

github.com](https://github.com/nisal-kumara/krish-lp-training/tree/master/Design%20Patterns/Memento) 

所以，这是我的文章的结尾，我希望你喜欢它。快乐编码👨‍💻。

# 参考

[](https://howtodoinjava.com/design-patterns/behavioral/memento-design-pattern/) [## Memento 设计模式 Java 中的 Memento 模式- HowToDoInJava

### 纪念品设计模式是行为模式，是“四人帮”讨论的 23 种设计模式之一。纪念品图案…

howtodoinjava.com](https://howtodoinjava.com/design-patterns/behavioral/memento-design-pattern/)