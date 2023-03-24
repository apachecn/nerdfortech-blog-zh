# For Noobs:如何在 Python 3 中复制字典？

> 原文：<https://medium.com/nerd-for-tech/for-noobs-how-to-copy-a-dictionary-in-python-3-6341d22b9ac2?source=collection_archive---------23----------------------->

这篇文章是由 noob 写的，当然也是为 noob 和任何教 noob 的人写的。

只使用等号复制字典很有诱惑力，因为这对于字符串和数字很有效，但对于新手来说，这是一个灾难。几乎可以肯定，你的代码迟早会有一个可能需要很长时间才能修复的 bug。

noob 应该从使用看似复杂的程序复制字典开始，事实上这是 noob 唯一简单且完全安全的方法。只有掌握了字典令人厌恶的微妙之处，noob 才应该尝试其他抄字典的方法。

noob 应该这样做。

例如，假设一个公司有四个程序员，其中两个组成“团队”，而另外两个单独工作。每个人都有一个工作质量满分的分数。为了开玩笑，你决定把杰克的分数改成不可能的 70 分(满分 10 分)。为了防止对名为“coders”的原始字典造成损害，您制作了一个名为“deep_copy_of_coders”的副本，并且只对其进行如下所示的修改。您导入复制模块，然后使用 deepcopy 函数生成一个普通、简单的字典副本，称为“coders”。然后你把杰克的分数改成 70，把笑话词典打印出来。一旦笑声，或者愤怒的抱怨平息了，你打印出原来的字典，表明没有造成伤害，因为原来的字典(“编码者”)没有被修改。

> > > coders = {'A team': {'Jack': 7，' Pete': 8 }，' John':9，' Jane':10}
> > >导入复制
>>>deep _ copy _ of _ coders = copy . deep copy(coders)
>>>deep _ copy _ of _ coders[' A team]][' Jack ']= 70
>>>deep _ copy _ of _ of

就在那里。就这么简单。“deep_copy_of_coders”已被修改,“coders”与以前一样。“当然”，noob 想。

从 noob 的角度来看,“deepcopy”有点用词不当，因为它所做的是普通的复制，正是 noob 期望一个名为“copy”的函数所做的，也正是 noob 在使用简单的等号复制字符串和数字时所习惯的。

Python 3 这样工作的原因对于 noob 来说实在是太复杂了，花十几秒的时间去思考它们简直是浪费时间。这是一个十秒钟的解释。Python 3(主要)不是为 noobs 使用而优化的，在某些情况下，它使用的内存更少，运行速度更快。

这篇文章的其余部分只对那些只想要一种安全、万无一失的方式来复制字典的新手有学术兴趣。所以如果你是这样一个新手，请不要再在这里阅读了。复制字典的其他方法很复杂，新手应该避免使用，直到不那么笨为止。

## 抄字典的其他方法。

让我们先来看看这个非常吸引人的简单的等号，它是复制字典的一种方式。它看起来会像这样。

#Noobs 不应该这样编码。

> > > coders = {'A team': {'Jack': 7，' Pete': 8 }，' John':9，' Jane ':10 }
>>>equals _ sign _ copy _ of _ coders = coders
>>>equals _ sign _ copy _ of _ coders[' A team '][' Jack ']= 70
>>>equals _ sign _ copy _ of _ coders
{ ' A team ':{ '

看看原来的字典是如何被修改的？苍天在上。对 noob 来说，这确实令人惊讶，而且完全不是典型的 noob 希望发生的事情。

还有导入复制模块和使用复制函数，这对于 noob 来说也有点用词不当，因为 deepcopy 函数给你一个普通的副本(正如 noob 所理解的那样),而 copy 函数给你一个叫做“shallow copy”的东西，这是一种奇怪的(对于 noob 来说)副本，有时至少表现得像一个简单的等号副本，而根本不是 noob 想要的。

还有其他复制列表的方法，我就不多说了，但是据我所知，没有一种像 deepcopy 函数那样友好。

顺便说一句，只要你还是一个新手，Python 3 列表也应该只使用复制模块中的相同的 deepcopy 函数来复制，原因完全相同。详见我的[https://medium . com/nerd-for-tech/for-noobs-how-to-copy-a-list-in-python-3-703522d 62 ef 9](/nerd-for-tech/for-noobs-how-to-copy-a-list-in-python-3-703522d62ef9)。

![](img/71f771d50535c82bd8cbd085d02e8ba7.png)

[天一马](https://unsplash.com/@tma?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照