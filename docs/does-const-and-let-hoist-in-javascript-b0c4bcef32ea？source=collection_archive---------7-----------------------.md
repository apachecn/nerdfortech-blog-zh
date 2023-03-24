# Javascript 中 const 和 let 提升吗？

> 原文：<https://medium.com/nerd-for-tech/does-const-and-let-hoist-in-javascript-b0c4bcef32ea?source=collection_archive---------7----------------------->

![](img/2683c8a896017725793bbed6fcb863be.png)

马库斯·斯皮斯克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

在这篇文章中，我将讨论 javascript 提升，并打破一些关于它的神话。

每个 Javascript 开发人员在学习语言时都会经历术语**提升**。但大多数人都弄错了。

**这是关于吊装的两个最流行的神话**

*   在编译过程中，所有的变量和函数声明都被移到程序的顶部
*   `const`和`let`不提升

让我们来谈谈第一个流言，

所有的变量和函数声明都被移到程序的顶部了吗？

*没有*

*什么！如果不是这样，会发生什么？*

*根据 MDN [“在*编译*阶段，变量和函数声明被放入内存，但是停留在你在代码中键入它们的地方。”](https://developer.mozilla.org/en-US/docs/Glossary/Hoisting#:~:text=Conceptually,code.)*

*那么我们来看看 javascript 是如何运行程序的？*

*在更高的层次上，运行任何 JS 程序都涉及到两个步骤*

1.  *程序编译*
2.  *程序的执行*

*所以当我们把任何一个程序交给 JS 引擎，那么引擎先把它编译成机器码，然后执行。在编译程序时，JS 引擎将所有的声明放入内存中。*

*   *当编译器遇到任何类似`var someVariable = "SomeValue"`的语句时，它会将变量`someVariable`放入内存，并用`undefined`初始化。*
*   *当编译器看到任何函数声明时，它会将函数声明放入内存，并用函数定义初始化它。*

*所以我们可以注意到这里的区别，带有`var`关键字的变量被放入内存并用`undefined`初始化，而带有`function`关键字的变量被放入内存并用真正的函数定义初始化。*

```
*console.log(a);
var a = "Hello js";
foo();
function foo() {
  console.log("From Inside foo");
}*
```

*当上面的代码片段运行时，我们将得到以下输出*

```
*undefined
From Inside foo*
```

*   *现在我们来分析一下刚刚发生了什么，当我们运行上面的代码片段 Javascript 首先编译程序，所以在编译过程中，它看到第 2 行有一个以`var a`开头的语句，于是编译器把`a`放到内存中，用`uneifined`初始化。然后它又在第 4 行看到一个函数声明`function foo() {`，所以编译器把`foo`声明放在内存中并用它的定义初始化它。*
*   *现在，在执行阶段，JS 引擎遇到第一行，然后检查内存中的变量`a`，发现有一个名为`a`的变量，它的值为`undefined`，所以它打印未定义的内容。然后在第 2 行，它将`a`赋给`Hello js`。现在在第 3 行，它调用`foo`，然后 JS 引擎再次在内存中查找`foo`，它发现它有一个函数`foo`并且有它的定义，所以它调用这个函数。*

*这就是`var`和`functions`被提升的过程。*

*现在来看第二个流言，*

*`*const*` *和* `*let*` *不吊，真的是这样吗？**

***“又一个大不了”***

*`const`和`let`同样，像`var`和`function`一样提升。但是与`var`和`function`不同的是，`const`和`let`不会被初始化。而且我们不能在声明它的行之前使用它，因为它会进入一个叫做*“时间死区(TMZ)”的特殊区域。**

*让我们用一个例子来理解:*

```
*console.log(a);
var a= "hello world"
var b = "again hello"
console.log(b)*
```

*现在你应该已经猜到了上面程序的输出，是的，你是对的*

```
*undefined
again hello*
```

*好的，现在你能猜出下面程序的输出吗*

```
*console.log(a);
var a= "hello world"
var b = "outer b"
console.log(b)
{
  console.log(b); // ReferenceError: Cannot access 'b' before initialization
  let b = "inner b";
}*
```

*您可能已经猜到了它会像上面的程序一样打印出来*

```
*udefined
outer b*
```

*但事实并非如此，因为`let`的作用域是在块级别，所以它被提升并进入*时间死区*。这就是我们得到`ReferenceError`的原因。所以事实证明`let`像`var`和`function`一样被吊起来。`const`也是如此。*

## *结论*

*好了，现在我们知道提升并不意味着将所有的声明移动到程序的顶部。最重要的是`const`和`let`被提升。*

*希望你喜欢这篇文章。欢迎任何建议和反馈。*