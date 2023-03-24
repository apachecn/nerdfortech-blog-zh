# 对于 Noobs:如何在 Python 3 中复制列表。

> 原文：<https://medium.com/nerd-for-tech/for-noobs-how-to-copy-a-list-in-python-3-703522d62ef9?source=collection_archive---------36----------------------->

这篇文章是 noob 写的，也是为 noob 写的。当然，这也适用于任何教 noobs 的人。

对于 noob 来说，只使用等号复制列表会导致灾难。有一个很大的危险是你的代码中会有一个 bug，需要你花很长时间去修复。

这就是如何复制一个列表的方法，至少在开始时是这样，直到你掌握了列表工作的一些惊人的微妙之处。

假设每一年的最佳编码员名单总是略有不同，因此，你不必从头开始制作新的名单，只需复制旧的名单，重命名为新的一年，然后相应地修改它。假设老詹姆斯得到了小詹姆斯的很多帮助，所以你把他们放在一起，就好像他们是一个团队。结果是一个嵌套列表。到 2020 年，小詹姆斯已经不再写代码，他的妹妹帕特里夏接替他的角色，帮助爸爸。列表没有其他变化。所以唯一需要的修改就是用帕特丽夏代替小詹姆斯。首先，导入复制模块，并使用 deepcopy 函数复制旧列表。然后修改列表，用“Patricia”替换“James Junior”。

`best_coders_of_2019 = [['James Junior', 'James Senior'], 'Mary', 'Pete']`

`import copy`

`best_coders_of_2020 = copy.deepcopy(best_coders_of_2019)`

`best_coders_of_2020[0][0] = 'Patricia'`

这就是全部了。如果你打印出 2020 年最佳编码器，你会看到:

`[['Patricia', 'James Senior'], 'Mary', 'Pete']`

如果你打印出 2019 年最佳编码，你会看到:

`[['James Junior', 'James Senior'], 'Mary', 'Pete']`

很好，很简单，对吧？如果你只是想知道如何安全地复制一个列表，你可以停止阅读。这篇文章的其余部分只对你有学术意义。

这样一个简单直观的过程需要导入一个模块，这似乎有点奇怪。

为什么不单独使用等号呢？它看起来像这样:

`#Noobs should not code like this.`

`best_coders_of_2019 = [['James Junior', 'James Senior'], 'Mary', 'Pete']`

`best_coders_of_2020 = best_coders_of_2019`

`best_coders_of_2020[0][0] = 'Patricia'`

想到这一点是很自然的，因为单独使用等号对字符串和数字很有效。如果你打印出 2020 年最佳编码器，你会看到:

`[['Patricia', 'James Senior'], 'Mary', 'Pete']`

如果你打印出 2019 年最佳编码，你会看到:

`[['Patricia', 'James Senior'], 'Mary', 'Pete']`

看到问题了吗？单独使用等号复制 2019 列表，然后修改它，也改变了 2019 列表。我的天啊。

什么样的人会料到呢？Python 3 就是这样设计的。作为一个新手，我不太明白它的来龙去脉(这真的很难理解)，但它与用更少的时间复制列表和用更少的内存存储列表有关。Noobs 不需要知道为什么。有很多方法可以复制列表，但是只有看起来复杂的 deepcopy 函数看起来很简单。

一个警告:我认为单独使用等号复制字典也有同样令人惊讶的行为。好消息是 deepcopy 处理字典和列表的方式差不多。

@bartshmatthew 在 Twitter 上如果不想在这里评论。