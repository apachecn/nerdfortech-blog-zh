# 让我们来解决这个问题

> 原文：<https://medium.com/nerd-for-tech/lets-sort-this-out-4b15ab49b4f2?source=collection_archive---------7----------------------->

这是我关于模拟 JavaScript 面试中向我提出的问题的系列文章的下一部分。

当被要求对一个数字数组中的元素进行排序时，我首先犯了一个简单使用 *sort()* 方法的错误。虽然这对于字符串元素来说很好，但是对于数字来说会产生一些奇怪的结果。让我们看看会发生什么。

首先，一个字符串数组。

```
const anArray = [“today”, “is”, “a”, “whole”, “new”, “beginning”]console.log(anArray.sort())[“a”, “beginning”, “is”, “new”, “today”, “whole”]
```

*数组*中的元素根据它们的首字母按字母顺序排序。很简单。如果两个或更多的元素以同一个字母开始，那么排序方法将转到第二个字母，依此类推。例如:

```
const newArray = [“cat”, “car”, “bat”, “box”, “doll”, “dill”]console.log(newArray.sort())[“bat”, “box”, “car”, “cat”, “dill”, “doll”]
```

如果我们对一组数字使用 *sort()* 会怎么样呢？

```
const numArray = [12, 5, 13, 9, 2, 0]console.log(numArray.sort())[0, 12, 13, 2, 5, 9]
```

坚持住。12 和 13 怎么会先于 2、5 和 9 出现？？？

发生这种情况是因为使用 *sort()* 将数组的元素转换成字符串，然后按 Unicode 顺序排列。

我的面试官给了我一些提示，让我使用一个函数来比较这些数字元素。凭着我的记忆，我在 *sort()* 的括号内使用了以下内容:

```
function(a, b){return a — b}
```

让我们试着用这个排序 numArray。

```
console.log(numArray.sort(function(a, b){return a — b}))[0, 2, 5, 9, 12, 13]
```

这还差不多。现在我们将数组的元素按升序排序(即从最小到最大)。如果我们想按降序(从大到小)对它们进行排序呢？我们不是在 return 语句中从 *a* 中减去 *b* ，而是从 *b* 中减去 *a* 。

```
console.log(numArray.sort(function(a, b){return b — a}))[13, 12, 9, 5, 2, 0]
```

这一切是如何运作的？compare 函数接受两个值(由参数 *a* 和 *b* 表示)，从一个值中减去另一个值，并将一个值返回给 sort 方法。如果*a-b*返回负值，那么排序方法将把 *a* 设置为比 *b* 更低的数组索引。以我们的 *numArray* 为例，假设 *a* 为 2， *b* 为 5。compare 函数将获取这两个值并执行一些简单的运算(2–5 =-3 ),然后将结果返回给 sort 方法。sort 方法接收到一个负值，将给予 2 一个低于 5 的索引，因此当数组中的元素按升序排序时，2 将列在 5 之前。

但是，如果 *a — b* 返回一个正值呢？在这种情况下， *a* 将被分配较高的索引。在我们的 *numArray* 示例中，当 compare 函数比较 9 和 5(9–5 = 4)时，它将向 sort 方法返回一个正值，这将为 9 分配一个更高的索引。因此，当列出排序后的数组元素时，9 在 5 之后。

当数组元素按降序排序时，这种情况会发生变化。也就是说，如果 *b — a* 返回负值， *b* 被分配较高的索引，如果 *b — a* 返回正值， *b* 被分配较低的索引。例如，如果 *b* 是 2，而 *a* 是 5，比较函数将运行其计算(2–5 =-3)，这将提示排序方法给 2 一个比 5 更高的索引。因此，当列出排序后的数字时，5 将出现在 2 之前。

但是如果比较函数返回 0 呢？让我们用另一组数字来试试:

```
const myNumbers = [7, 5, 91, 52, 7, 8]console.log(myNumbers.sort(function(a, b){return a — b}))[5, 7, 7, 8, 52, 91]
```

该数组包含两个 7，当然，当 compare 函数开始比较它们时，它将返回 0(7–7 = 0)。正如[Mozilla Developer Network](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort)所说，这将“使 a 和 b 相对于彼此保持不变，但是相对于所有不同的元素进行排序。”

简而言之，我强烈建议将这两个比较函数提交到内存中。

*   按升序排序:function(a，b){ return a-b })
*   按降序排序:function(a，b){return b — a})

顺便说一下，这些可以很容易地重写为箭头函数:

*   (a，b) => a — b
*   (a，b) => b — a