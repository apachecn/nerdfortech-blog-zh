# JavaScript: this 和箭头函数

> 原文：<https://medium.com/nerd-for-tech/javascript-this-and-arrow-functions-215a90973a05?source=collection_archive---------15----------------------->

![](img/f3ff1775fc4d09446c94c18b2814fad8.png)

图片由[迈克尔·泽兹奇](https://unsplash.com/@lazycreekimages)通过[unsplash.com](https://unsplash.com/photos/ir5gC4hlqT0)拍摄

***关于 JavaScript 介绍性面试主题的迷你系列的第 2 部分。***

我上一篇帖子，[这是什么？](/nerd-for-tech/what-is-this-javascripts-this-keyword-23cc6fab741a)，试图澄清什么是`this`，为什么它在 JavaScript 中如此令人困惑。这篇文章将继续深入箭头函数以及它们如何影响`this`所指的东西，从而理解`this`。

> 在开始之前，如果你需要复习一下 arrow 函数，你可以看看我的第一篇文章[分解 JavaScript 函数](/nerd-for-tech/breaking-down-javascript-functions-5be9b9fba96d)。

那么箭头功能是如何关联并影响`this`的呢？为了理解这一点，首先理解词法范围的含义会有所帮助。如果你理解了词法范围，你可以跳过下一节，直接进入[为什么词法范围很重要](#3066)。

用我自己的话来说，我将词法范围定义为嵌套函数的外部范围。实际上这要复杂一点，可以用 [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures#lexical_scoping) 来更彻底的描述。

> 单词*词法*指的是词法作用域使用变量在源代码中声明的位置来确定该变量在哪里可用。嵌套函数可以访问在其外部作用域中声明的变量。— MDN

这意味着嵌套函数总是可以访问任何父作用域(变量或函数),而不是相反。对 scope 的访问在家族树中向上移动，而不是向下。

对我来说，举例比阅读定义更容易理解。

```
1\.  function lexicalDemo() {
2\.    let a = 1003\.    function innerFunction() {
4\.      console.log(a * 5)
5\.    }6\.    innerFunction()
7\.  }8\.  lexicalDemo() // => 500
```

即使变量`a`是在函数`innerFunction`之外定义的，它仍然能够访问该变量的值，因为它可以访问`lexicalDemo`的范围。

# 为什么词法范围很重要

箭头函数不同于 JavaScript 中的常规函数，因为它们维护其父函数的作用域，而不是创建新的作用域，并且它们没有自己的`this`。如果它们没有自己的作用域，那么`this`在一个箭头函数内部指的是什么？让我们测试一下，看看。

```
1\.  function arrowFuncTest() {
2\.    let a = this3\.    const arrow = () => {
4\.      return this
5\.    }6\.    return arrow() === a
7\.  }8\.  arrowFuncTest() // => true
```

如你所见，从箭头函数`arrow`在第 4 行返回的`this`，严格等于变量`a`，变量【】在第 2 行被赋值为`this`。这是因为箭头函数没有自己的`this`，而是在它们的词法范围内引用`this`！

知道了这一点，我们现在知道如何理解箭头函数中的`this`所指的内容，但是如果箭头函数没有在另一个函数中定义呢？简单，让我们看一个例子。

```
1\. let a = this2\. const globalArrow = () => this3\. console.log(a === globalArrow()) // => true
```

这里我们的箭头函数`globalArrow`是在全局范围内定义的，所以它的词法范围是全局范围。正如我们在我以前的帖子中看到的,`this`在全局范围内是指窗口对象。

在我们结束之前，很快有必要讨论一下`bind()`、`call()`和`apply().`，我计划在本系列的后面讨论这些，但是我想指出，不建议在箭头函数中使用它们。快速而肮脏的解释是因为箭头函数假定了它们的父作用域的`this`，而没有它们自己的`this`。

现在不要太担心最后一段，因为我将介绍一个在箭头函数和常规函数上使用`bind()`、`call()`和`apply()`的比较示例。

希望在阅读完这个迷你系列的前两篇文章后，你会对`this`和箭头功能感到更加舒服。类似于第一篇文章，我不认为你需要记住每一个例子或用例。我认为对于面试来说，至少对于入门级的角色来说，最重要的事情是箭头函数维护它们的词法范围，并且不创建它们自己的`this`，相反，它们维护它们词法范围内的`this`。

下周收听如何使用`bind()`、`call()`和`apply()`的介绍。和往常一样，欢迎在下面评论`this`或箭头功能或你用来准备面试的任何技巧。

[***第一部分这是什么？JavaScripts 本关键字***](/nerd-for-tech/what-is-this-javascripts-this-keyword-23cc6fab741a)

[***第三部分:使用 JavaScripts bind()、call()和 apply()***](https://davidpolcari.medium.com/using-javascripts-bind-call-and-apply-868c4347cea9)