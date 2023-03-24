# 科特林收藏与运营公司

> 原文：<https://medium.com/nerd-for-tech/kotlin-collections-operations-d456cca64617?source=collection_archive---------1----------------------->

第二部分

这是 Kotlin collections & operations 文章的第 2 部分。[第一部](/nerd-for-tech/kotlin-collections-operations-469100411caa)到了。让我们讨论一下 Kotlin 提供的一些更有用的收集操作。

![](img/87b88841583a36784cd3b1bb39423187.png)

## Count() & Count {}

count()返回集合中元素的数量，count{}返回与给定谓词匹配的元素的数量。

count() & count{}

## **取放**

集合函数 *Take* 返回集合中初始的 ***n*** 个元素作为新的集合。 *Drop* 函数在从原始集合中删除最初的 ***n*** 个元素后，返回一个新的集合。

取放

## 总和、最大值、最小值、最大值和最小值

*min()*&*max()*函数实现 ***Comparable*** 接口，返回对象排序的最小元素或最大元素。

Sum，SumOf，Min，MinOf，Max，MaxOf

## 接缝

Kotlin joinToString 集合操作在应用了 ***分隔符*** *、* ***前缀*** *、****后缀*** 后，从所有集合元素中返回一个 ***字符串*** 。

如果集合很大，我们可以通过提供 *limit* 值来限制元素的数量，从而限制字符串的长度。*极限值*应该是一个非负的**值。默认值为-1，整个集合被认为是负的**数字。如果您提供 0 作为*限制，则*集合将被完全忽略。****

接合到字符串

在本文中，我们讨论了一些更有用的 Kotlin 收集操作。希望这篇文章对有兴趣学习 Kotlin collections 及相关操作的人有所帮助。