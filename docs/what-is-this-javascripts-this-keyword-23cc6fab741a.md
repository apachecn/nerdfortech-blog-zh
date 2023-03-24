# 这是什么？JavaScripts 这个关键字

> 原文：<https://medium.com/nerd-for-tech/what-is-this-javascripts-this-keyword-23cc6fab741a?source=collection_archive---------20----------------------->

![](img/ae5e9d94cffdf159967b7a8f1545f6f2.png)

照片由[艾米丽·莫特](https://unsplash.com/@emilymorter)通过[Unsplash.com](https://unsplash.com/photos/8xAA0f9yQnE)拍摄

***关于 JavaScript 介绍性面试主题的迷你系列的第 1 部分。***

请原谅这个双关语，自从上周晚些时候我完成了一次模拟技术面试后，我就等着用它了。这篇博客将是我探索一些关于 JavaScript 的常见面试问题的迷你系列的第一篇，从 JavaScript 关键字`this`开始。

我的面试官问的第一个问题是描述一下与 JavaScript 相关的`this`。一个简单、天真的问题，让我无言以对。我知道如何使用`this`，但当被要求描述它时，我甚至不知道从何说起。采访结束后，我立即跳上谷歌，开始阅读我在`this`上能找到的任何东西，以及我将在未来几周内写的一些其他话题。

简单来说，JavaScripts 的`this`关键字指的是它所属的对象。一个简单问题的简单答案是对的，特别是考虑到 JavaScript 中几乎所有的东西都是对象。没那么快。如果事情这么简单，我就不会写这篇文章了。对于 JavaScript 来说，这个主题比大多数语言更令人困惑的是使用`this`的上下文很重要！

![](img/4e755efb5ee5a530df049b3bee320783.png)

经由 imgflip.com

最容易理解的上下文是全局上下文(在函数或对象之外)。在这里调用时，`this`指的是全局对象，它在浏览器中是窗口对象，无论您是否处于严格模式。

在函数内部，`this`的值取决于它是如何被调用的。在严格模式下，如果值在执行前没有被绑定/应用/调用，那么`this`将是未定义的。在以后的文章中会有更多的介绍。然而，在*“草率模式”*中，这仍然是指全局对象。

*—更多信息请参见 MDN 页面上的* [*严格模式*](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode) *。*

JavaScript 类实际上是用来模拟面向对象的函数，因此类中的`this`的行为与函数中的一样。然而，类构造函数`this`内部只是一个对象，就像 JavaScript 中的所有东西一样。

让我们快速看几个例子。

```
//  Global Context
1\.  let x = this2\.  console.log(x === window) // true//  Function Context - "Sloppy Mode"
3\.  function thisExample () { console.log(this === window) }4\.  thisExample() // true//  Function Context - Strict Mode
5\.  function thisExample () {
6\.     "use strict"
7\.     console.log(this === window) }8\.  thisExample() // false9\.  /* this is undefined in strict mode because we didn't bind this 10\. to a value before executing the thisExample */
```

旁注:如果你想在函数上下文中用`this`访问全局变量，使用 var 来赋值。如果你不这么做，会发生什么。

```
1\.  var a = "THIS"
2\.  function thisEx () { return this.a } 3\.  thisEx() ==> "THIS"4\.  let y = "NOT THIS"
5\.  function nextEx () { return this.y }6\.  nextEx() ==> undefined
```

这是因为 let 是块范围的局部变量，而 var 是全局范围的变量。第 5 行的函数 nextEx 不能访问`y`,因为它不在函数范围内。

我想展示的最后一个例子是，当它被用在一个对象内部定义的方法中时。在这种情况下`this`指的是父对象。有趣的事实是，在对象内部定义和调用的函数实际上叫做方法！

```
//  Object Method
1\.  let obj = {
2\.     a: 10,
3\.     func: function () { return this.a }
4\.  }5\.  console.log(obj.func()) ==> 10
```

到目前为止，一切都是 JavaScripts 关键字`this`的“是什么”。我不认为你需要记住所有这些，相反，只要知道`this`指的是一个对象，它的值根据它被调用的范围/场景而变化。

为了完整地回答这个问题，你可能需要提到 bind()、call()和 apply()，因为这些是最常用的。我的下一篇博文将深入这三个函数，以及它们如何让我们使用`this`关键字。

面试时如何形容`this`？请在下面的评论中告诉我！

[***第二部分:JavaScript:本和箭头功能***](/nerd-for-tech/javascript-this-and-arrow-functions-215a90973a05)

[***第三部分:使用 JavaScripts bind()、call()和 apply()***](https://davidpolcari.medium.com/using-javascripts-bind-call-and-apply-868c4347cea9)