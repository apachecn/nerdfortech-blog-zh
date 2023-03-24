# 调优代码性能的基本技巧

> 原文：<https://medium.com/nerd-for-tech/basic-tips-for-tuning-code-performance-cfcd722d23e9?source=collection_archive---------22----------------------->

## 遇到性能问题时要考虑的一些非常基本的事情

![](img/8a8859bb6c2c1e17cc7727c2e7d90062.png)

由 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的 [CHUTTERSNAP](https://unsplash.com/@chuttersnap?utm_source=medium&utm_medium=referral) 拍摄

代码慢很烦人。不仅对用户如此，对开发者也是如此。我注意到，在性能不是主要关注点的项目中，优化代码性能通常是非常低效的。因此，我想我应该写下一些我在试图提高一段代码的性能时倾向于做和首先考虑的事情，这段代码以前很可能没有被调优过。

请注意，这不是关于如何开发高性能代码的指南。这只是一个(希望是有帮助的)提示集，告诉你在开发过程中，当试图获得一些性能时，首先应该从哪里入手，而不要过多地关注这个非常宽泛的主题。

这些技巧的目的是
a)节省你无效地调整代码性能的时间
b)节省你在开发期间运行代码的时间

## 技巧 1 —测量

![](img/7ef51ad22eb61325ede2d1f04ffb6c6f.png)

照片由 [Sabri Tuzcu](https://unsplash.com/@sabrituzcu?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

我的第一条建议，也是我在代码中任何地方考虑提高性能时总是做的第一件事，就是进行度量！花时间测量代码的每一部分执行的时间，并将这些时间与其他部分进行比较。通常情况下，您会发现在您不期望的代码部分，您的性能正在下降。因此，在投资时间来尝试加速代码的某个特定部分之前，请确保它确实是在消耗您的性能！听起来很简单，但是这个基本的评估步骤经常被跳过，因为人们确信他们的表现到底在哪里下降了。

分析不同代码段性能的最简单的方法——相信我，我们都曾因使用这种方法而感到内疚！—将 print()语句插入到代码中。这是一个快速而麻烦的解决方案，只要你能事先缩小你想查看的代码范围，它就能很好地工作。不要在您对代码及其性能一无所知的大型代码库中尝试这种方法！另一方面，还有一些工具，如[英特尔 VTune 分析器](https://software.intel.com/content/www/us/en/develop/tools/oneapi/components/vtune-profiler.html#gs.5xgs7z)，它也用于高性能计算领域。好的一面是:它曾经是一个非常昂贵的工具，但是现在可以免费使用了。没有什么真正的理由不应该尝试一下，放弃 print()策略。

分析之后，您应该准确地知道代码在哪里花费了大量的运行时间。从现在开始，专注于这些部分。记下这些计时，因为在您完成代码性能改进之后，您应该再次运行相同的测量，以确保您获得了运行时间。说到新旧性能的比较，这里有一个额外的提示:在调整性能之前编写测试。如果它们破坏了代码，那么您的性能改进没有任何好处！

## 技巧 2——减少

![](img/cf800fa9697a1677763ada65d579a9c2.png)

[钳工](https://unsplash.com/@benchaccounting?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

另一个看似简单的技巧，听起来很容易，但仍然非常非常有价值:最快的代码是不执行的代码！也就是说，提高性能最简单的方法是丢弃不再需要的代码。这特别包括放弃所有不使用的特性。尤其是在数据科学代码中，但在所有其他代码中也是如此，特性被开发，然后再次被转储。或者人们测试了一些东西，但是之后并没有完全恢复他们的更改(一个适当的 git 分支工作流将会防止你的代码中出现这种情况！).我最近遇到的一个例子是一个曾经对我们的客户非常有用的功能，但在开发过程中已经过时了:在我们项目的开始，我们将最终结果以 Excel 文件的形式写入磁盘。后来，我们开发了一些功能，允许客户使用我们的 GUI 调整这些结果。现在，保存调整后的结果需要相当长的时间，因为我们仍然将它们作为 Excel 文件写下来。然而，由于我们在项目期间所做的所有开发，我们注意到那些 Excel 文件现在已经过时了。简单地删除这个“功能”可以使保存过程加快大约 100 倍，这让我们的客户很高兴，因为这意味着他们的工作流程现在可以更顺畅了。

如果你坚持技巧 1，你知道你的代码大部分时间花在哪里。批判性思考。与利益相关者交谈。代码在那里做的工作还值得努力吗？或者是花费时间来产生无论如何都不会被使用的结果？请注意，这通常不是一个非黑即白的问题。有时答案是“我们不需要全部，而是部分”。在这些情况下，您仍然可以通过删除不太相关的部分来获得一些好处。

## 技巧#3 —并行化

![](img/0345040eac0097efc6189677e6dbdabb.png)

约翰·安维克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

现在，对于那些一直在等待如何提高特定代码的性能的技巧的人来说，这是我现在的最后一个技巧:并行化。

不，我不是在谈论类似高性能计算的并行化。这是一个非常非常复杂的主题，深入这个主题将占用您大量的时间来做“实际的”(即与性能无关的)实现工作。但是在很多情况下，琐碎的并行化可以帮到你很多。例如，在最近的一个项目中，我们必须读取大量文件并验证其内容。因为我们使用了技巧 1，我们知道读取文件很快，但是验证它们却不是。提高验证功能的性能是相当困难的，但是很容易将工作分散到多个进程中，每个进程操作自己的文件集。

再次注意，我在这里不是在谈论高性能计算。我说的是“日常”代码的性能。这就是我在这里举 Python 例子的原因。如果你生活在高性能计算的世界里，你很可能不会使用 Python。Python 为广泛的用例提供了一些精彩的模块。关于并行化，其中有[多重处理](https://docs.python.org/3/library/multiprocessing.html)和 [joblib](https://joblib.readthedocs.io/en/latest/) ，我发现这两个都非常有用并且相当容易使用。因此，如果您刚刚开始使用并行化，就从这里开始吧。

感谢阅读！请在评论中发布更多简单的建议。我还会留意我注意到的其他事情，然后在将来整理出这 101 的第二部分。

**资源**

1.  [英特尔 VTune 分析器](https://software.intel.com/content/www/us/en/develop/tools/oneapi/components/vtune-profiler.html#gs.5xgs7z)
2.  Python 模块:[多重处理](https://docs.python.org/3/library/multiprocessing.html)， [joblib](https://joblib.readthedocs.io/en/latest/)