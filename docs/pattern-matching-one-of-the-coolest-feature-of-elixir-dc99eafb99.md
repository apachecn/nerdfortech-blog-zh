# 模式匹配——灵丹妙药最酷的特性之一

> 原文：<https://medium.com/nerd-for-tech/pattern-matching-one-of-the-coolest-feature-of-elixir-dc99eafb99?source=collection_archive---------12----------------------->

分享我对 Elixir 的模式匹配功能及其工作原理的看法。

![](img/f1da396b1c1b9c8a5e7d3f191bdd7765.png)

由[克里斯·福勒](https://unsplash.com/@cgfowler?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 什么是模式匹配？

Elixir 中的`=`操作符实际上是一个匹配操作符，我们使用了`=`操作符来分配变量。

```
$ iex -S mix
> x = 1
1
> 1 = x
1
> 2 = x
** (MatchError) no match of right hand side value: 1
```