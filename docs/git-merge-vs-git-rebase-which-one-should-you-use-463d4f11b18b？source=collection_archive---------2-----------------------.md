# Git Merge vs Git Rebase:应该使用哪一个？

> 原文：<https://medium.com/nerd-for-tech/git-merge-vs-git-rebase-which-one-should-you-use-463d4f11b18b?source=collection_archive---------2----------------------->

![](img/88e7841f3655a46e40808340e20ebccf.png)

照片由[扬西·敏](https://unsplash.com/@yancymin?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/photos/842ofHC6MaI?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

Git 是一个版本控制系统，允许开发人员跟踪和管理代码变更。它使许多开发人员能够同时在同一个项目上工作，并让他们组合他们的修改。

在处理 Git 时，您的 Git 存储库中通常会有许多分支。这些分支可以用来隔离变更并与其他开发人员交互，它们可以代表不同的特性、问题修复或实验工作。

您最终需要将一个分支的修改合并到另一个分支中。Git 提供了两种主要的方法来实现这一点:Git merge 和 Git rebase。两者都可以用来合并来自不同分支的变更，但是方式不同，使用哪一个将取决于您的需求和喜好。

在本帖中，我们将详细讨论 Git merge 和 Git rebase，比较它们的区别以及何时使用它们。

## Git 合并

Git 合并是将一个分支的变更合并到另一个分支的最简单的技术。当您进行 Git 合并时，您从一个分支获取更改，并将它们应用到另一个分支，从而导致新的提交。

这里有一个 Git 合并如何工作的例子:

```
$ git checkout master
$ git merge feature
```

在这种情况下，我们将切换到主分支，并将功能分支合并到其中。当您进行 Git 合并时，Git 会创建一个新的 commit 来表示两个分支的联合。通常，提交消息将包括文本“合并分支‘特征’”，随后是对修改的解释。

Git merge 是一个简单易懂的选项。当您需要在不修改提交历史的情况下将一个分支的更改快速集成到另一个分支时，这是一个非常好的选择。

然而，Git 合并可能会导致提交历史膨胀，尤其是如果您经常合并分支的话。每个合并提交都象征着两个分支的联合，使得提交历史更加难以阅读和理解。

## Git Rebase

Git rebase 是将一个分支的变更合并到另一个分支的更强大的方法。当您进行 Git rebase 时，您是在一个分支之上重放另一个分支的提交。这将修改从源分支集成到目标分支，同时也更改了提交历史。

下面是 Git rebase 工作原理的一个例子:

```
$ git checkout feature
$ git rebase master
```

在这个例子中，我们将特征分支重新放在主分支上。Git 将在主分支之上重放来自特性分支的提交，在这个过程中创建新的提交。因此，功能分支显示为主分支之上的一系列提交，从而产生线性提交历史。

Git rebase 是比 Git merge 更强大的选项，允许您以多种方式更改提交历史。例如，您可以使用 Git rebase 将许多贡献合并到一个提交中，或者重新组织提交。这有助于清除提交历史，使其更容易阅读和理解。

## 何时使用 Git Merge 和 Git Rebase

那么，Git merge 和 Git rebase 哪个更好？答案将由您的要求和偏好决定。以下是一些需要记住的一般规则:

1.  如果您想快速简单地将一个分支的变更合并到另一个分支，那么 Git merge 是一个非常好的选择。它简单明了，不需要特殊的专业知识或能力。
2.  Git rebase 有助于清理提交历史，使其更容易阅读和理解。它可以用于将多个提交合并成一个提交，或者重新安排提交。但是，请注意，Git rebase 可能会更复杂，如果处理不当，可能会导致冲突或丢失提交。
3.  当许多开发人员将变更推向同一个分支时，与团队合作时，Git merge 通常是更好的选择。因为它不修改提交历史，所以不太可能产生冲突或误解。
4.  如果您正在处理一个特性分支，并且希望将您的提交与主开发分支区分开来，那么 Git rebase 会非常有用。它使您能够集成您的修改，而不会生成冗余的合并提交。

最后，Git merge 和 Git rebase 之间的决定将由您的工作流和项目需求决定。两者各有利弊，你必须决定哪个最适合你。

我希望这篇文章对你有所帮助。 ***感谢阅读。***