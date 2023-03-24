# 评估 JavaScript 评估，第 2 部分

> 原文：<https://medium.com/nerd-for-tech/assessing-the-javascript-assessment-part-2-7c1f24555a61?source=collection_archive---------12----------------------->

这里是我在 LinkedIn 的 JavaScript 评估中遇到的一些问题/话题。

**谈论我的发电机**

在学习 JavaScript 的时候，我见过很多可怕的函数，也见过很多可怕的函数(大部分都是我写的！).但是就在我认为我已经最终掌握了 JavaScript 函数的神圣艺术时，JavaScript 用一些新的、更复杂的东西给了我当头一棒。我最近一次这样的经历来自 LinkedIn 评估中的一个问题:“一个函数的名字是什么，它的执行可以被暂停，稍后再恢复？”

说什么，JavaScript？

到目前为止，我见过或用过的所有函数都直接运行在它们的代码中。套用《大富翁》中“进监狱”的卡片，他们直接从开始到结束，没有通过 Go，也没有收取 200 美元。然而，事实证明，JavaScript 有一个功能，可以通过 Go，休息一下，收集 200 美元，然后最终进入监狱。

以下是这个问题的可能答案:

*   箭头功能
*   承诺函数
*   发电机功能
*   异步/等待功能

箭头函数只是编写 JavaScript 函数的一种新方法，所以我们可以马上把它从列表中划掉。一个[承诺](https://www.w3schools.com/js/js_promise.asp)是一个对象，不是一个函数。有一个 [async](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function) 函数，但它不被称为 async/await ( *await* 是 async 可能包含的一个关键字)。答案是“[生成器函数](https://javascript.plainenglish.io/how-to-use-the-generator-function-in-javascript-6ab00380cc5f)”,你可能已经从上面的标题中猜到了。

好的，如果你没用过发生器函数，请举手。(是的，我正在举手。)因此，我不会假装完全理解它是如何工作的。让我们看一个极其简单的例子:

```
function* myGenerator(x) {
  yield x;
  yield x * x;
}let g = myGenerator(5)
```

让我们试试这个:

```
console.log(g.next().value)
```

您认为控制台上会记录什么？它将是… 5。

然后如果我们运行 *console.log(g.next()。值)*又来了？这次我们会得到 25 分。我们能再来一遍吗？我们可以，但我们只会得到“未定义”。

关于生成器函数的语法，您首先会注意到的是关键字*函数*后面的*。我们的例子还使用了关键字 [*yield*](https://www.javascripttutorial.net/es6/javascript-generators/) ，因为“yield 语句暂停了生成器的执行并返回一个值。”要记住的另一个关键方面是使用 *next()* ，根据 [Mozilla Developer Network](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function*) 的说法，它“返回一个对象，该对象具有包含生成值的 value 属性和一个 done 属性，该属性指示生成器是否已经生成了它的最后一个值，作为一个布尔值。”

为了演示，如果我们运行了 *console.log(g.next())* ，我们将得到这个对象: *{value: 5，done: false}* 。第二次运行 *console.log(g.next())* 会返回 *{value: 25，done: false}* 。第三次会返回 *{value: undefined，done: true}* 。

生成器函数是可迭代的，非常适合异步编程。点击以上段落中的链接，了解更多信息。

改变你的订单？

现在来点简单的。评估中的一个问题显示了以下代码:

```
const dessert = { type: ‘pie’ };
dessert.type = ‘pudding’;
```

这里我们有一个变量*甜点*，它被赋予一个对象文字作为它的值。

*甜品. type* 的价值会是多少？它还会是“馅饼”吗？改成‘布丁’了？抛出错误？还是作为‘未定义’返回？这个问题可能是为了测试你对关键字 [*const*](https://www.w3schools.com/js/js_const.asp) 的理解。如果用更老的关键字 *var* 声明*甜品*，您可能马上就会知道第二行代码将*甜品的值. type* 改为‘布丁’。但是我们在这里使用了常量，正如 w3schools.com 告诉我们的，“用常量定义的变量不能被重新赋值。”

然而，在这种情况下，我们处理的是对象，而不是原始数据类型。这有区别吗？的确如此。即使对象是用*常量*定义的，它的属性也可以改变。因此，*甜品. type* 现在是‘布丁’。(如果由我来决定，我会指定它为巧克力布丁，上面放一些凉奶油。)

**一元还是非一元？这是个问题。**

"哪个选项中的*不是*是一元运算符？"

如果没有咨询我亲爱的朋友谷歌，我不会知道这个问题的答案。事实上，我甚至不记得曾经遇到过术语 [*一元运算符*](https://www.digitalocean.com/community/tutorials/javascript-unary-operators-simple-and-useful) ，尽管事实证明我以前用过好几个。根据 [Mozilla 开发者网络](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_Operators#unary_operators)的说法，“一元运算是只有一个操作数的运算。”其中有逻辑 NOT(！)、减量运算符(---)、增量运算符(++)、关键字 *typeof* 。

评估中的这个问题有四种可能的答案，如下所示:

*   实例 of
*   空的
*   类型 of
*   删除

而正确答案是… *instanceof* 。javascript.info 说[运算符的*实例*](https://javascript.info/instanceof)的目的是“检查一个对象是否属于某个类”，与一元运算符不同，它返回一个布尔值。

更多即将推出！