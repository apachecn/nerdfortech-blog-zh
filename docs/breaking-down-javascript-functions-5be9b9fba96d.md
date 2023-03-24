# 分解 JavaScript 函数

> 原文：<https://medium.com/nerd-for-tech/breaking-down-javascript-functions-5be9b9fba96d?source=collection_archive---------10----------------------->

Mozilla 开发者网络( [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Functions) )告诉我们，JavaScript 中的函数是“一组执行任务或计算值的语句”。很好，这是一个非常标准的定义，大多数开发人员都很熟悉，不管他们使用什么语言。那么，尽管 JavaScript 函数的定义很简单，但为什么它们如此模糊和令人困惑呢？

![](img/8581bbcd8df62c8da59d67b94b903f48.png)

照片由[维姆·范因德](https://unsplash.com/@wimvanteinde)对[unsplash.com](https://unsplash.com/photos/e6pPIcJ05Jg)拍摄

为了回答这个问题，我们将深入研究编写函数的不同方法，如何定义它们以及如何调用它们，并尝试解释每种方法的优点和缺点。

让我们从最简单、最容易识别的函数编写方式开始。

```
1\. // Function Declaration
2\. function helloWorld() {
3\.   console.log('[Hello, world!](https://en.wikipedia.org/wiki/%22Hello,_World!%22_program)')
4\. }5\. helloWorld()
6\. // Hello, world!
```

上面的代码是一个函数声明，使用关键字`function`后跟函数名`helloWorld`，并且不使用任何参数将文本`Hello, world!`记录到控制台。函数语句是花括号`{…}`中包含的所有内容。您通过调用函数名后跟`()`来调用函数声明，如第 4 行所示。`()`通常被称为调用操作符。

太棒了，让我们更进一步。JavaScript 中的函数被称为一等公民。这意味着函数可以作为变量存储，作为参数传递给其他函数，作为函数的返回值，以及几乎做任何对象能做的事情。MDN 有更多关于这个[主题](https://developer.mozilla.org/en-US/docs/Glossary/First-class_Function)的信息，但是现在我们已经知道了足够多的信息，可以继续下一步写函数了。

经由[giphy.com](https://giphy.com/gifs/season-15-the-simpsons-15x18-l2Jecm1l0wnJ2kQDu)

```
1\. // Function Expression
2\. let helloWorld = function() {
3\.   console.log('Hello, world!')
4\. }5\. helloWorld()
6\. // Hello, world!
```

这就是所谓的函数表达式。您会注意到`function`关键字并不是函数表达式中的第一个单词，但仍然出现在第 2 行。这个函数的调用方式与使用变量`helloWorld`上的调用操作符的函数声明相同，实际上返回的内容与我们上面的函数声明相同！

有趣的事实:在上面的例子中,`helloWorld`变量被设置为一个匿名函数，这只是一种有趣的方式来说明函数本身没有函数名。更准确地说，这个名字实际上是一个等于`‘’`的字符串，JavaScript 从变量名`helloWorld.`推断出这个名字

如果您愿意，您可以指定一个名称，如下例所示，其中函数名不再是匿名的，而是设置为等于字符串`‘hello’`。这允许添加一些功能，比如引用代码块中的函数，这是工具箱中的一个好工具。这个例子中没有演示这种行为，但是您可以随意进行一些谷歌搜索来了解更多信息！

```
1\. // Function Expression with a Named Function
2\. let helloWorld = function hello() {
3\.   console.log('Hello, world!')
4\. }5\. helloWorld()
6\. // Hello, world!
```

我们提到 JavaScript 将函数视为一等公民。允许我们做的事情之一是将函数存储为变量，并将它们作为参数传递。函数表达式就是这样发生的！

```
1\.  // Passing Functions As Arguments2\. function example(arg) {
3\.   arg
4\. }5\. let helloWorld = function() {
6\.   console.log('Hello, world!')
7\. }8\. example(helloWorld())
9\. // Hello, world!
```

第 2 行的`example`函数接受一个参数。在第 8 行，我们调用了`example`函数，并将函数表达式`helloWorld`作为参数传递，以将文本`‘Hello, world!’`记录到控制台。

![](img/29f60aecf40afd2c2e59e6913919c34c.png)

由[paweczerwi ski](https://unsplash.com/@pawel_czerwinski)通过[unsplash.com](https://unsplash.com/photos/B_GEOp9u8h8)拍摄的照片

如果你认为传递函数作为参数很酷，你会爱上箭头函数。arrow 函数是用 JavaScript 编写函数的一种比较晦涩的方法，但是一旦你理解了它们，你就会意识到它们只是一个写起来比较短的函数，但是有一些独特的功能和一些使用上的限制。

```
// Arrow Functions1\. let doMath = (a, b) => a + b

2\. doMath(1, 1)
3\. // 2or 4\. let doMath = (a, b) => { 
5\.   let c = a + b
6\.   return c
7\. }8\. doMath(1, 1)
9\. // 2
```

上面的两个例子是编写执行相同任务的箭头函数的两种不同方式。这里有几点需要指出。单行箭头函数不需要花括号，并且有隐式返回。多行箭头函数需要花括号，也需要显式返回，如第 6 行所示。带有单个参数的箭头函数不需要圆括号`()`，如果没有参数，可以使用`_`代替圆括号，如下所示。

```
// More Arrow Functions1\. let a = 'Hello, world!'
2\. let hello = str => console.log(str)3\. hello(a)
4\. // Hello, world!or5\. let hello = () => console.log('Hello, world!')6\. hello()
7\. // Hello, world!or 8\. let hello = _ => console.log('Hello, world!')9\. hello()
10\. // Hello, world!
```

唷，我们刚刚看到了很多例子，它们都展示了编写箭头函数的不同方法。JavaScript 在编写这些类型的函数时给予我们的灵活性是它们之所以伟大的原因，但也是它们如此晦涩难懂和难以识别/学习的原因。虽然这些方法编写起来更快，但它们确实有一些限制，比如不能使用 call、apply 或 bind 方法。原因与范围有关，范围本身就是一个完全不同的话题。

JavaScript 函数如此模糊的原因和它们如此有用的原因是一样的，那就是我们在如何编写和使用它们时被赋予了灵活性。希望这篇文章能帮你理清思路，让你对 Javascript 为开发人员提供的不同选择感到更舒服。

在未来，我希望涵盖更多与 JavaScript 函数相关的主题，但同时这里有一些资源涵盖基础主题，包括[即时调用函数](https://developer.mozilla.org/en-US/docs/Glossary/IIFE)、[函数范围](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Functions#function_scope)和[闭包](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures)。