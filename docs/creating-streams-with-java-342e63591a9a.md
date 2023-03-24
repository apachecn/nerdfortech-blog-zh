# 用 java 创建流

> 原文：<https://medium.com/nerd-for-tech/creating-streams-with-java-342e63591a9a?source=collection_archive---------17----------------------->

## Java 指南

## 了解用于创建流的最常用方法

![](img/fcad54f26b1ccc965ab75eafad5a5fbc.png)

如果你理解这些方法，创建流是非常简单的。您通常会从数组创建流，但是您也可以从一个对象创建无限流、空流甚至流。

# 从数组创建流

若要从数组创建流，可以同时使用 Arrays.stream()和 Stream.of()方法。这些方法以不同的方式工作:

*   **Arrays.stream()** 接受一个数组作为参数并返回一个顺序流。要创建 double、long 和 integer 数组，需要使用 IntStream、DoubleStream 和 DoubleStream 类。
*   **Stream.of()** 接受一个或多个值作为参数，这些值将被包含在新的流中。

使用两种方法创建简单流:

使用 Arrays.stream()方法从数组创建双精度、长整型和整型流:

使用 Arrays.stream(T[] array，int startInclusive，int endExclusive)方法，可以从数组的一部分创建流:

这就是如何从数组创建流的方法，你也可以使用下面的方法来创建流:IntStream.of()、LongStream.of()和 DoubleStream.of()，这些方法用于创建整数、长整型和双精度流，它们的工作方式类似于 Stream.of()方法。

# generate()和 iterate()方法

这两种方法的工作方式如下:

*   方法**生成(供应商< T > s)** 创建由供应商生成的无限个流。如果我们看一个例子，这就更容易理解了:

如果要创建有限流，请使用 limit()方法:

*   如果 *hasNext* 条件为真，则使用 **iterate(T 种子，谓词 hasNext，一元运算符 next)** 方法将操作 *next* 应用于*种子*。这里我们用这个方法将一个数乘以 *n* ，直到这个数大于 100:

如果不提供谓词，这个方法也可以产生无限的流。

# empty()和 ofNullable()方法

这两种方法很容易理解:

*   可以想象，empty()方法返回一个没有元素的流。
*   ofNullable(T t)接受单个元素作为参数，如果该元素为 null，则流将为空，否则，该方法将返回具有单个元素的流。

*感谢阅读本文！如果你有任何建议，请在下面留下评论。一定要拍拍这个帖子，关注我。*