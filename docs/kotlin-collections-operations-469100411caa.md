# 科特林收藏与运营公司

> 原文：<https://medium.com/nerd-for-tech/kotlin-collections-operations-469100411caa?source=collection_archive---------1----------------------->

第一部分

## 你应该知道的所有 Kotlin 收集操作

![](img/87b88841583a36784cd3b1bb39423187.png)

在这篇文章中，我们将讨论一些 Kotlin 集合操作，一个 Kotlin 开发人员，Android 开发人员，应该知道如何利用这种语言提供的巨大资源。

假设您理解可变集合和不可变集合之间的区别，包括列表、映射和集合。

## ForEach，forEachIndexed & withIndex

forEach，forEach indexed & within indexed result

***forEach*** lambda 函数遍历一个列表，并提供对每个元素的访问。在 lambda 块中，可以直接访问元素。在上面提到的例子中，访问的是 integer 元素。

如上所述，****forEachIndexed****lambda 函数提供了对给定列表中的元素及其索引的访问。更重要的是，***forEachIndexed***使用一个局部变量来保持索引计数。**

**within index 的 ***返回一个***lazy***Iterable，它将原始数组的每个元素包装成一个 IndexedValue，包含该元素的索引和元素本身。*****

## *Map，MapNotNull & MapIndexed*

*kot Lin***map***for collection 返回一个新的集合，将变换函数应用于原始集合中的元素。*

*地图*

****mapNotNull*** 在将转换应用于原始集合的元素之后，返回包含非空值的新集合。*

*mapNotNull*

****mapIndexed*** 在对具有当前索引的原始集合的元素应用所提供的变换之后，返回新的集合。*

*地图索引*

## *Filter，FilterNot，FilterNotNull() & Partition*

****过滤器*** 集合操作在将给定的谓词与原始集合中的元素进行匹配后，返回新的元素集合。*

****filterNot*** 操作返回不满足所提供谓词的项目的新集合。*

*在匹配了所提供的谓词之后，*分区*操作返回一对集合，将元素分成两组。第一个集合由满足谓词的元素组成，第二个集合由其余的元素组成。如果我们显式声明分区结果的类型，那么就会是***Pair<List<T>，List < T > >*** 。在下面的例子中，是 ***对<列表< Int >，列表< Int > >****

*过滤器、过滤器非、过滤器非&分区*

## *过滤实例*

****filterIsInstance*** 返回一个新集合，该集合包含从原始集合中提取的参数中指定的特定类型的元素。*

*过滤实例*

*在下一部分中，将讨论更多的 Kotlin 收集操作。*