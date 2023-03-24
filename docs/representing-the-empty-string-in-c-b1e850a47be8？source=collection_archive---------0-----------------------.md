# C#中的空字符串

> 原文：<https://medium.com/nerd-for-tech/representing-the-empty-string-in-c-b1e850a47be8?source=collection_archive---------0----------------------->

![](img/1ba22f76233ebf19a7f15155c1a46859.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上[博欣·卡拉伊万诺夫](https://unsplash.com/@bkaraivanov?utm_source=medium&utm_medium=referral)的照片

我最近向我的团队提出了以下问题:

对于我们的项目，我们更喜欢空字符串的哪种表示？:

A) `""`

B) `string.Empty`

我提出这个问题是因为我们决定使用 StyleCop 分析器，它有一个规则鼓励开发人员更喜欢`string.Empty`而不是`""`。但是不要盲目的跟随棉绒…