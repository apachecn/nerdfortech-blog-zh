# 评估 JavaScript 评估，第 3 部分

> 原文：<https://medium.com/nerd-for-tech/assessing-the-javascript-assessment-part-3-d49fcf0d9459?source=collection_archive---------19----------------------->

欢迎回来！准备好接受更多的 JavaScript 熏陶了吗？这是我关于 LinkedIn JavaScript 评估的下一篇文章。

“狐狸”说了什么？

![](img/198f53baf0d9e48737bdfcdfe80ebb9b.png)

图片来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=1773727) 的[图形妈妈团队](https://pixabay.com/users/graphicmama-team-2641041/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=1773727)

认为你已经掌握了 JavaScript 数组？查看下面的代码:

```
var a = [‘dog’, ‘cat’, ‘hen’]
a[100] = ‘fox’
console.log(a.length)
```

现在，您能猜到控制台上会记录什么吗？

变量 *a* 被赋予一个数组的值，其初始长度为 3。很简单。然后他们在第二条线上给了我们一个难题。只有 3 个元素的数组可以有一个索引设置为 100 的附加元素吗？数组中最后一个元素的索引，字符串' hen '，将是 2(记住数组索引总是从 0 开始)，所以我们可以通过声明 *a[3] = 'fox'* 将' fox '添加到数组的末尾。(是的，我们可以使用 push()方法，但是这个问题显然是为了测试您对数组索引如何工作的理解。)

你的选择是 3，101，100 和 4。第二行代码是否没有做任何事情，使得数组只剩下三个元素？或者，如果将“fox”添加到数组中，数组的长度是否设置为 4，以便只考虑新元素？或者添加索引为 100 的“fox”是否意味着我们现在有了一个更大的数组？

有趣的是，第二行也可以，fox 的索引确实是 100。索引为 3–99 的元素将被*未定义*。因此，我们知道问题的答案不会是 3 或 4。由于索引从 0 开始，这意味着“fox”将是第 101 个元素。因此， *console.log(a.length)* 将返回 101。

**何以“为”？为什么是“forEach()”?**

有时您需要循环一些代码，JavaScript 有多种方法可以做到这一点。如果您参加 JavaScript 评估，您可能会被要求找出 [*forEach()*](https://www.w3schools.com/jsref/jsref_foreach.asp) 方法和 [*for*](https://www.w3schools.com/js/js_loop_for.asp) 语句之间的区别。您可能的答案:

*   for 循环可以嵌套，而 forEach 循环则不能
*   forEach 只能用于字符串，而 for 可以用于其他数据类型
*   forEach 只能用于数组，而 for 可以用于其他数据类型
*   forEach 允许您指定自己的迭代器，而 for 不允许

我将继续说，我们可以忘记第一个和第四个选项，因为嵌套和迭代器不是问题所在。如果你需要刷新你的记忆，让我们看看几个 *forEach()* 和 *for* 的例子。

```
const dogs = [‘German Shepherd’, ‘Jack Russell Terrier’, ‘Pomeranian’, ‘Golden Retriever’]dogs.forEach(d => {
  console.log(`My dog is a ${d}`)
})
```

我们的 *forEach()* 将在数组上循环，导致以下内容被记录到控制台:

```
My dog is German Shepherd
My dog is a Jack Russell Terrier
My dog is a Pomeranian
My dog is a Golden Retriever
```

我们可以用循环的*做同样的事情吗？*

```
for (i = 0; i < dogs.length; i++) {
  console.log(`My dog is a ${dogs[i]}`)
}
```

是的，运行这个命令将会得到与使用 *forEach()* 时相同的文本。

这些例子涉及一个数组。其他数据类型呢？下面是另一个*为*循环的例子:

```
let str = ‘’;
let sym = ‘$’;
for (let i = 0; i <= 10; i++) {
  str += sym;
  console.log(str)
}
```

运行这个命令会将美元符号记录到控制台，每一行都比前一行多一个。

```
“$”
“$$”
“$$$”
“$$$$”
“$$$$$”
“$$$$$$”
“$$$$$$$”
“$$$$$$$$”
“$$$$$$$$$”
“$$$$$$$$$$”
“$$$$$$$$$$$”
```

要是我能那么容易就赚到真正的美元就好了！无论如何，回到我们的问题…

在这个实例中，我们没有对数组进行循环，所以我们还可以使用 *forEach()* 吗？神奇的 8 号球说，“如果你想让你的代码工作，就不要。”不像 *for* 循环，只要满足一个条件就一直运行，“forEach()方法为*每个数组元素*执行一次提供的函数，”根据 Mozilla Developer Network(着重部分由作者添加)。这告诉我们，评估问题的正确答案是第三个，因为我们只能对数组使用 *forEach()* 。

**调用所有函数！**

为了让一个[函数](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Functions)完成它的工作，它必须被调用。在那之前，它只是坐在那里，希望有人最终会记住它，并调用它做它被写的事情。假设您需要调用下面的函数来确定购买 50 美元的商品应该缴纳多少税。

```
function addTax(total) {
  return total * 1.05;
}
```

仔细选择你的答案，年轻的学徒:

*   返回 addTax 50
*   加税 50；
*   附加税(50 美元)
*   附加税(50)

注意到第一个选项有什么问题吗？它有函数名和你要计算税款的金额，但是开头的关键字 *return* 呢？你需要它来运行你的功能吗？不，它不应该在那里，所以我们马上知道第一个可能的答案是错的。

第二种选择怎么样？我们去掉了 *return* ，留下了函数名和金额。丢了什么吗？确实有。这个选项和它上面的选项没有括号中的数量，括号是调用函数的语法的关键部分。正如 W3Schools.com 告诉我们的那样，“访问一个没有()的函数将返回函数对象而不是函数结果。”如果你试图将 *addTax* 登录到控制台，你会得到这样的结果:

```
ƒ addTax(total) {
  return total * 1.05;
}
```

不是很有用。如果你试图运行*add tax*50，你会得到一个错误。

那么最后两个选项呢？两者都将金额括在括号中，唯一的区别是一个在金额前有美元符号，而另一个没有。如果你试图调用包含美元符号的函数会发生什么？您将得到这条可爱的消息:“ReferenceError: $50 未定义。”这就给我们留下了选项#4， *addTax(50)* 。

今天到此为止。在进入新的话题之前，我将至少再写一篇关于 LinkedIn JavaScript 评估的文章。感谢阅读！