# 评估 JavaScript 评估，第 4 部分

> 原文：<https://medium.com/nerd-for-tech/assessing-the-javascript-assessment-part-4-e760fce811a8?source=collection_archive---------17----------------------->

这是我在 LinkedIn 的 JavaScript 评估上的最后一篇文章。

# **异步 NSYNC**

如果你曾经使用过 JavaScript，那么你会对

```
<script src=”app.js” async></script>
```

*异步*是怎么回事？根据[W3Schools.com](https://www.w3schools.com/tags/att_script_async.asp)的说法，这个布尔属性“指定脚本一旦可用就异步执行”。LinkedIn 评估中的一个问题是，你可以使用什么样的 JavaScript 代码来实现: *async* 用于:内部、外部、内部和外部，或者只用于内部或外部，以导出一个承诺。幸运的是，对于我们这些以前没有遇到过这个属性的人来说(你的也包括在内)，快速查看一下 W3Schools.com(或另一个参考源)会告诉我们，它只适用于*外部* JavaScript 代码。

# **输入错误的时间**

哪个开发人员不喜欢好的错误消息呢？(这里的讽刺意味很重…)所以，你正在测试你的代码，你得到了这个令人愉快的消息:

```
TypeError: Cannot read property ‘reduce’ of undefined
```

哦，乔伊！这究竟意味着什么？好吧，它让你知道你正在调用一个名为 *reduce* 的方法:

*   空数组
*   不存在的对象
*   具有空值的对象
*   已声明但没有值的对象

哪一个？我突然想尝试一些自己的代码。

```
var scoreList = []function addUp(total, num) {
  return (total + num)
}console.log(scoreList.reduce(addUp))
```

在控制台上，我看到……一个“类型错误”的信息，虽然和问题中的不一样。这一条写着，“减少没有初始值的空数组。”因此，可能答案列表中的第一个选项是不正确的。

如果我们用一个不存在的对象来尝试呢？让我们看看如果运行相同的代码会得到什么，除了我们不会声明或初始化我们的变量 *scoreList* 。控制台显示，

```
ReferenceError: scoreList is not defined
```

一种完全不同的错误。[Mozilla Developer Network](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/ReferenceError)告诉我们，“引用一个不存在的变量时，ReferenceError 对象表示一个错误，”与 [*TypeError*](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/TypeError) 相反，后者“表示一个操作无法执行时的错误，通常(但不排除)当一个值不是预期的类型时。”这就排除了第二个可能的答案。

问题可能是对象的值为空吗？让我们试试这个:

```
var scoreList = nullfunction addUp(total, num) {
  return (total + num)
}console.log(scoreList.reduce(addUp))
```

这一次，我们得到…

```
TypeError: Cannot read property ‘reduce’ of null
```

很接近，但没有雪茄。事实上，得到一个不同的 [*类型错误*](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/TypeError) 并不奇怪，因为 [*未定义*](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/undefined) 和 [*null*](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/null) 并不意味着同一件事。

通过排除，这意味着列表上的最后一个选项是我们的正确答案。我们的错误消息是由于声明了一个对象，但没有给它赋值。让我们测试一下。

```
var scoreListfunction addUp(total, num) {
  return (total + num)
}console.log(scoreList.reduce(addUp))
```

果然，我们得到了问题中出现的错误。这个故事的寓意是什么？不要忘记给你的变量赋值！

# **我们该给这个变量取什么名字？**

现在来一个简单的。查看这些可能的变量名:

*   总计
*   功能
*   第五条
*   名字

如果你选择接受它，你的任务就是选择一个有效的。

好吧，我就开门见山了——是最后一个，*名*。其他三个怎么了？是时候回顾一下 JavaScript 变量的命名约定了！因为变量名不能包含空格，*总计*不起作用。我们不能使用保留字，所以*功能*是不可能的。虽然变量名可以包含数字，但它们必须以字母开头，从而消除了 *5thItem* 。我们可以使用 *firstName* ，因为它以一个字母开头，并且使用 camel case。(使用 snake case 来命名变量 *first_name* 也可以；然而，camel case 被认为是 JavaScript 中的最佳实践。)

# **增量地说…**

如果你运行下面的代码，你会得到什么？

```
let a = 5;
console.log(++a);
```

你认识变量 *a* 前面的“++”吗？那是[增量运算符](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Increment)，用来把数值加 1。正确回答这个问题的关键是知道当你把递增运算符放在操作数前面而不是后面时会发生什么。执行前者将导致返回增加的值，在这种情况下将是 *6* 。执行后者将产生 *5* ，因为虽然值是递增的，但返回的是初始值，而不是递增的值。所以 *6* 就是*。*

如果你决定参加 LinkedIn JavaScript 评估，我希望这篇文章和我之前的三篇文章对你有所帮助。一如既往，继续学习，继续编码！