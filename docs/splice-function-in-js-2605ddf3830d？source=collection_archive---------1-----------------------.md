# 拼接可以做很多！！！

> 原文：<https://medium.com/nerd-for-tech/splice-function-in-js-2605ddf3830d?source=collection_archive---------1----------------------->

我们知道 JS 中的 splice 函数用于删除从给定数组的任何索引开始的一个或多个元素。它还可以用于在特定索引处添加元素。

![](img/3421938fea8a5378da920a557bed64fd.png)

接合

> syntax of**Splice**
> ***array . Splice(start[，deleteCount[，item1[，item2[，…]]])***

一般来说，有三个参数:

1. **Start** —表示在哪个索引处添加或删除元素。

2. **deleteCount** —说明要从指定的索引中删除多少个元素。

例如:如果为 1，则删除指定的索引

3. **items** —(可选)声明要添加到数组中的元素。通常用于向现有数组中添加元素。

4.**返回—** 包含已删除元素的数组。
如果只移除一个元素，则返回一个元素的数组。
如果没有删除任何元素，则返回一个空数组。

**以下示例有助于更好地说明拼接的功能:**

1.  设 arr = [1，2，3，4]
    arr.splice(0) // arr= []

由于起始索引在这里被指定为 0 但是删除计数没有被指定，
所以它会直接从数组中删除所有的元素。

2.设 arr = [1，2，3，4]。arr.splice(0，0) // arr = [1，2，3，4]

因为 start 是 0，deleteCount 是 0。因此，它返回相同的数组。

3.设 arr = [1，2，3，4]
arr.splice(0，0，5) // arr = [1，2，3，4]

这里也不会添加或删除数组中的元素，这些元素可以在 *eg-2* 中引用

4.设 arr = [1，2，3，4]
arr.splice(0，1) // arr = [2，3，4]

它从索引 1 中删除该元素。

5.设 arr = [1，2，3，4]
arr.splice(1，1，5) // [1，5，3，4]

它在索引 1 处替换 5

6.设 arr = [1，2，3，4]
arr.splice(1，2，5) // [1，5，4]

它用一个单一元素(此处为 5)替换了第一个和第二个索引。
as 2 表示删除计数。

**splice 的一个有趣用法是以如下方式重新初始化一个数组:**

> 为了重新初始化一个数组，我们也可以使用 arr.splice(0)来删除数组中的所有元素。 *arr.splice(0) // arr = []。*

我希望您现在对拼接功能有了更好的理解。有关该功能的详细信息，请查看以下文档:

[https://developer . Mozilla . org/en-US/docs/Web/JavaScript/Reference/Global _ Objects/Array/splice](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/splice)

谢谢大家！！！
:)