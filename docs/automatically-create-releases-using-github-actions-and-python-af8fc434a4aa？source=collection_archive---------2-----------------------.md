# 使用 GitHub 动作和 Python 自动创建发布

> 原文：<https://medium.com/nerd-for-tech/automatically-create-releases-using-github-actions-and-python-af8fc434a4aa?source=collection_archive---------2----------------------->

![](img/d65d51f084ab9bb309b4be99043cba56.png)

[Franck V.](https://unsplash.com/@franckinjapan?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

执行重复的任务既费时又乏味。幸运的是，我们有工具可以用来为我们做重复性的工作并节省时间。最近，我决定在我的一个库中自动化发布过程。在本文中，我将分享我使用 [GitHub Actions](https://github.com/features/actions) 和 [Python](https://www.python.org/) 自动化发布过程的旅程。

# 问题是

我维护一个依赖于新发布的`Detekt CLI`的[库](https://github.com/natiginfo/action-detekt-all)。当有了新版本的`Detekt CLI`时，只要我有空，我就会收到电子邮件并发布我的库。使用这种方法，我通常按照下面的步骤来发布新的版本:

1.  收到一封关于 Detekt 新版本的电子邮件。
2.  一有空就在`Dockerfile`和`README.md`更新版本。
3.  提交更改。
4.  创建新标签。
5.  推动变革。
6.  发布新闻稿。

虽然新版本的发布时间很关键，但很明显，从**第一步**到**第二步**的时间差会根据我的情况而变化，这可能会让用户不高兴。除了发布新版本的时间和让我的库的用户高兴之外，个人时间也很重要，可以花在更令人兴奋的事情上，而不仅仅是手工做一些可以自动化的工作。

# 解决办法

因为我们知道问题所在，并且我们已经写下了步骤，所以我们可以想出解决这个问题的方法。我想制作的解决方案需要以下特性:

*   每 24 小时检查一次`Detekt CLI`的新版本。
*   如果`Detekt CLI`的最新版本与我的存储库的最新标签不同，请更新`Dockerfile`和`README.md`中的`Detekt CLI`版本
*   提交更改
*   创建新标签
*   *创建一个发布分支*
*   推动变革
*   当创建新的发布分支时，创建一个新的拉请求。

创建一个新的发布分支和一个新的拉请求是我过去手工发布时没有做过的事情。我添加了前面提到的步骤，以确保我可以审查变更，从而不会将损坏的代码合并到主分支中。为了检查新的发布并运行[我的 Python 脚本](https://github.com/natiginfo/action-detekt-all/tree/master/.release-manager)，我创建了两个工作流，其中[第一个工作流](https://github.com/natiginfo/action-detekt-all/blob/master/.github/workflows/create_new_release.yml)每 24 小时运行一次脚本，而[第二个工作流](https://github.com/natiginfo/action-detekt-all/blob/master/.github/workflows/create_release_pr.yml)在创建新的发布分支时创建一个拉请求。除了每 24 小时运行一次发布脚本的工作流，我注意到 GitHub actions 中的`workflow_dispatch`触发器可以用于手动运行工作流。

根据`Detekt`的发布历史，每个月大概有两到三个新发布。通过使用 GitHub Actions 和 Python 创建的简单解决方案，我的目标是每月至少节省 20 到 30 分钟。此外，我可以在其他项目中使用该解决方案，从而节省大量时间。

# 最后的话

本文的目的是鼓励您通过自动化任务来利用现有工具并节省时间。正如您所看到的，一旦您写下步骤，使用您所使用的工具实现自动化就很容易了。

我希望你喜欢这篇文章。

> 这最初发布在[natigbabayev.com](https://www.natigbabayev.com/2020-08-18/automating-the-release-process-using-github-actions-and-python)