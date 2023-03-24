# 范围和上下文的区别是什么？

> 原文：<https://medium.com/nerd-for-tech/what-is-the-difference-between-scope-vs-context-704e6466db20?source=collection_archive---------7----------------------->

![](img/d43e17018a6ecf4295256a34d29212b8.png)

Starks Don Pablo 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

JavaScript 是一种奇怪的语言，但我仍然喜欢它，并且喜欢使用它，不管它是核心 JavaScript、Angular、React 还是 NodeJS。我通过库 jQuery 与 JavaScript 进行交互。最流行的库之一，用于 DOM 操作和在网页上添加动画。

在 2014 年的一段时间后，我开始了解 Angular，然后 React，然后 Vue，然后 React Native，然后 NodeJS，这些库和框架每年都会出现。今天，我们有许多完全基于 JavaScript 的框架和库。我们可以只用 JavaScript 创建一个完整的企业级应用程序，这就是为什么最近我们听到很多对 MEAN 和 MERN 堆栈开发人员的需求。

现在回到正题。随着 JavaScript 越来越流行，要求也越来越高。我们需要非常清楚它的一些核心概念。所以，让我们从范围 vs 上下文开始。大多数开发人员非常清楚什么是范围，但是不太清楚什么是上下文。最有趣的是，如果你问已经在 React 项目中工作的人什么是上下文，他/她会开始谈论 [React useContext()](https://reactjs.org/docs/hooks-reference.html#usecontext) hook。

我们一个一个来看范围和上下文，看看两者有什么区别。

## **什么是范围？**

范围只不过是变量的可访问性(可见性)。JavaScript 有以下类型的作用域。

1.  全球范围
2.  局部范围
3.  块范围

**全局范围:**当我们在函数或块语句(if、switch、for 循环、while 循环)之外声明任何变量时，它被称为全局范围变量，因为它可以被模块/文件中的任何人访问。

```
// Global scoped variable
var scopeName = "Global";function getScopeName() {
    console.log(scopeName); // Global
};if(true) {
  console.log(scopeName); // Global
};console.log(scopeName); // Global
```

**局部作用域:**当我们在一个函数中声明一个变量时，这个变量只能在这个函数中被访问，这个变量就是这个函数的局部作用域。

```
var scopeName = "Global";function getScopeName () {
    var year = 2021;
    console.log(scopeName); // Global
    console.log(year); // 2021
};getScopeName();console.log(year); // "ReferenceError: year is not defined"
```

如果我们在任何函数中没有使用 var/let/const 关键字来定义任何变量，那么这个变量就变成了全局变量。

```
function whichYear() {
  year = 2021;
}whichYear ();console.log(year);
```

**块作用域:**如果我们在任何块语句(If、switch、for 循环、while 循环)内使用 let/const(ECMAScript 6 中新引入的)定义变量，那么这些语句就不能在外部访问。因为 let 和 const 有块作用域，var 有函数作用域。

```
if(true) {
  const greeting = "Hello";
  let year = 2021;

  console.log(greeting); // Hello
  console.log(year); // 2021
}console.log(greeting); // "ReferenceError: greeting is not defined"console.log(year); // "ReferenceError: year is not defined"
```

## **什么是语境？**

范围和上下文之间有很多混淆。我们已经看到了什么是作用域。希望你能明白。那么，什么是背景呢？

上下文与对象有关系。换句话说，我们可以用与函数所属对象相关的关键字**来引用上下文。**

让我们用一个例子来理解这一点:

```
function currentContext() {
  console.log(this);
};currentContext(); // Window {window: Window, self: Window, document: document, name: "JS Bin Output ", location: Location, …}
```

在上面的例子中，函数被直接定义并执行。在这种情况下，定义和执行都发生在全局对象中，在这种情况下是窗口。

请看另一个有趣的例子:

```
function setName() {
  this.myName = "Rakesh";
}setName();console.log(myName); // Rakesh
```

好吧。现在你可能会想， **myName** 的值是如何在函数外部打印出来的？正如我们在最后一个例子**中看到的，这个**指的是窗口，我们将 **myName** 属性附加到窗口对象上。所以，我们可以做 **window.myName** 或者 **myName** 两者相等。希望会清楚。

让我们再举一个 person 对象的例子，该对象中定义了函数。

```
const person = {
  name: "Rakesh",
  getName: function() {
    console.log(this.name); // "Rakesh"
  }
}person. getName();
```

现在在上面的例子中， **getName** 函数被定义在一个 **person** 对象下，在这种情况下，**这个**关键字将引用 **person** 对象，因为 **getName** 函数属于 **person** 对象。

现在，在 ECMAScript 6 引入后，箭头函数也出现了。箭头函数没有自己的执行上下文。所以，在哪里执行箭头函数并不重要。这个内部箭头函数的值将总是指向外部对象。换句话说，arrow 函数在词汇上解析了 **this** 。

> *词法作用域*意味着在一组嵌套的函数中，**内部函数可以访问其父作用域**的变量和其他资源。

```
var name = "I am global name";const person = {
  name: "Rakesh",
  getName: () => {
    console.log(this.name);
    console.log(this); // Window {window: Window, self: Window, document: document, name: "JS Bin Output ", location: Location, …}
  }
}person.getName (); // "I am global name"
```

正如我们在这里看到的，我们已经使用了 arrow 函数，现在它正在打印全局变量的值。

唷！现在我希望你能更清楚范围和上下文的概念，以及这些概念在 ECMAScript 6 中是如何改变的。

快乐学习！