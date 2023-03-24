# 前端—第二轮:JavaScript 简介！

> 原文：<https://medium.com/nerd-for-tech/front-end-round-two-introduction-to-javascript-deb3d0c1c2d3?source=collection_archive---------30----------------------->

![](img/4f04479ed4297b813c826bf63028f79d.png)

琼·加梅尔在 [Unsplash](https://unsplash.com/s/photos/javascript?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

好吧，让我们提高一点炒作因素。在这一部分，我们将触及前端编程语言 JavaScript。这是 JavaScript 全栈开发的一个关键组件，对于那些计划关注前端的人来说也是如此。

以下是一些需要记住的事情:

1.  当你第一次学习 JavaScript 时，可能会有点痛苦，因为它可能会变得令人费解。
2.  它往往是公司使用的基础，但是随着 TypeScript 的发明，有些公司将会脱离纯粹的、普通的 JavaScript。【https://www.typescriptlang.org/ 
3.  不是你爷爷的 JavaScript！

你会注意到所有这三件事都紧密地联系在一起。那是故意的。作为初学者，您需要理解您可以快速构建一个无限循环，或者遇到范围和闭包问题。开始时，我想简单地说一下。慢慢来，不断测试你的代码！

从什么开始:

*   Var vs Let vs Const

这是 ES2015 (ES6)中新增的一项重要功能。这很关键。有一种说法，很多人都是从它出来之后才被教的。“坚持到不行！”你可能会问自己我在说什么，所以让我们用几篇关于这个主题的文章来分解一下:
[https://www . freecodecamp . org/news/var-let-and-const-whats-the-difference/](https://www.freecodecamp.org/news/var-let-and-const-whats-the-difference/)
[https://ui.dev/var-let-const/](https://ui.dev/var-let-const/)
[https://codeburst . io/JavaScript-var-let-or-const-which-one-should-you-use-2fd 521 b 050 fa](https://codeburst.io/javascript-var-let-or-const-which-one-should-you-use-2fd521b050fa)

或者简单来说:
Var 就是老办法。如果你不是 100%的时间都在关注它，它会导致大量的问题，即使这样，如果你使用 var，你也会以一个超级慢的应用程序而告终。它可以被重新声明，并使用提升，这意味着它被加载在代码的顶部，先于其他任何东西。

定义—提升:
**提升**是 **JavaScript 的**默认行为，将所有声明移动到当前作用域的顶部(到当前脚本或当前函数的顶部)。
来源:[https://www . w3schools . com/js/js _ raising . ASP #:~:text = raising % 20 is % 20 JavaScript 的% 20 default % 20 behavior % 20 of % 20 moving % 20 all % 20 declarations % 20 to，script % 20 or % 20 the % 20 current % 20 function](https://www.w3schools.com/js/js_hoisting.asp#:~:text=Hoisting%20is%20JavaScript's%20default%20behavior%20of%20moving%20all%20declarations%20to,script%20or%20the%20current%20function)。

所以当你开始学习 JavaScript 时，你会想使用 Let 或 Const。以下是您使用每种方法的场景:

Const = >在不对代码产生负面影响的情况下，尽可能地使用它。

Let = >仅当自然改变变量的值至关重要时。有很多方法可以用 const 干净地做到这一点，但是有些时候只使用 let 是合理的。for 循环就是一个例子。

![](img/df9482cbdbaac33d1a15625ac47ce4ce.png)

JavaScript for 循环

现在我们将在后面讨论循环，因为它比我们现在需要的要高级一些。但是我希望在我放弃学习变量的一些资源之前，您能看到一些代码。

var:
[https://developer . Mozilla . org/en-US/docs/Web/JavaScript/Reference/Statements/var](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/var)
[https://www.w3schools.com/jsref/jsref_var.asp](https://www.w3schools.com/jsref/jsref_var.asp)

let:
[https://developer . Mozilla . org/en-US/docs/Web/JavaScript/Reference/Statements/let](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let)
[https://www.w3schools.com/js/js_let.asp](https://www.w3schools.com/js/js_let.asp)

const:
https://www.w3schools.com/js/js_const.asp
T21[https://developer . Mozilla . org/en-US/docs/Web/JavaScript/Reference/Statements/const](https://www.w3schools.com/js/js_const.asp)

恭喜你。你已经正式用 JavaScript 看了你的第一个主要话题(实际上是三个)！我们讨论了不同的基础变量类型 var、let 和 const。接下来，我们将讨论如何输入普通的 JavaScript！

不过，在我们继续之前，我想先介绍几个学习网站，让你了解一下。我希望您继续阅读这些文章，但是让我们让您朝着学习 JavaScript 的方向努力吧！
[https://www . freecodecamp . org/learn/JavaScript-algorithms-and-data-structures/# basic-JavaScript](https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/#basic-javascript)
[https://www . theodinproject . com/paths/full-stack-JavaScript/courses/JavaScript](https://www.theodinproject.com/paths/full-stack-javascript/courses/javascript)

好了，现在我们来讨论一下原始类型:

定义:
在 [JavaScript](https://developer.mozilla.org/en-US/docs/Glossary/JavaScript) 中，**原语**(原语值，原语数据类型)是不是[对象](https://developer.mozilla.org/en-US/docs/Glossary/Object)并且没有[方法](https://developer.mozilla.org/en-US/docs/Glossary/Method)的数据。共有 7 种原始数据类型:[字符串](https://developer.mozilla.org/en-US/docs/Glossary/String)，[数字](https://developer.mozilla.org/en-US/docs/Glossary/Number)， [bigint](https://developer.mozilla.org/en-US/docs/Glossary/BigInt) ，[布尔](https://developer.mozilla.org/en-US/docs/Glossary/Boolean)，[未定义](https://developer.mozilla.org/en-US/docs/Glossary/undefined)，[符号](https://developer.mozilla.org/en-US/docs/Glossary/Symbol)，以及[空值](https://developer.mozilla.org/en-US/docs/Glossary/Null)。
来源:[https://developer.mozilla.org/en-US/docs/Glossary/Primitive](https://developer.mozilla.org/en-US/docs/Glossary/Primitive)

作为一个已经开发和学习了一年多的人，我可以毫无疑问地告诉你，你很少会看到 bigint 和 symbol。我从来没有机会使用它们(尽管我一直在找借口！).我鼓励你探索定义中嵌入的链接，找出每一个链接是什么和它做什么！

那么为什么原语如此重要呢？因为它们是你在使用 JavaScript 时所做的一切的基础，实际上是作为一个整体的编程。是的，这个话题很重要。

让我们快速浏览一遍。

*   字符串—文本值
*   number —是一个数字(可以包括负数和小数，这和大多数语言都不一样！其他的有 float 之类的东西…[https://www . tutorialspoint . com/python/python _ numbers . htm #:~:text = float % 20(float % 20 point % 20 real % 20 values，x%20102%20%3D%20250)。](https://www.tutorialspoint.com/python/python_numbers.htm#:~:text=float%20(floating%20point%20real%20values,x%20102%20%3D%20250).))
*   bigInt — **BigInt** 是 **JavaScript** 中的内置对象，提供了一种表示大于 253–1 的整数的方法(来源:[https://www.geeksforgeeks.org/bigint-in-javascript/](https://www.geeksforgeeks.org/bigint-in-javascript/)
*   布尔型—真或假
*   未定义——顾名思义。有些东西没有定义。
*   Symbol—[https://developer . Mozilla . org/en-US/docs/Web/JavaScript/Reference/Global _ Objects/Symbol](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol)
*   Null 没有值。

哇哦。链接太多了！我明白了。这是一些枯燥的东西，除非你对编程感兴趣！我当然鼓励每个人一个接一个地阅读这些部分，看看我提供的信息，并在谷歌上搜索，以获得关于每个主题的更多信息。学会为自己寻找资源是一件非常重要的事情。

为此，我们将在这一点上稍作休息，谈谈我和大多数工程师将从哪里获取信息并得到问题的答案。

名单上的第一个:谷歌！在几毫秒之内，你可以得到大量的信息，基本上是任何你需要的信息。这包括编程语言。(我真的需要在这里提供链接吗？)

第二名:谷歌，但有了 grepper！从某种意义上来说，Grepper 基本上是 google 的精华版本。一旦你在谷歌上搜索了与编程相关的内容，它会在搜索栏下提供用户提交的答案。这是一个 chrome 插件(与勇敢的浏览器一起工作)。[https://chrome . Google . com/web store/detail/g repper/amaaokahonnfjjemodnpmeenpnnbkco？hl=en](https://chrome.google.com/webstore/detail/grepper/amaaokahonnfjjemodnpmeenfpnnbkco?hl=en)

第三:堆栈溢出！堆栈溢出是一个很好的资源，但是我要警告你，它已经存在很长时间了，所以将会使用 ES6 之前的变量，并且你可能会因此得到一个令人困惑的答案。无论如何，这是一个很好的资源！[https://stackoverflow.com/](https://stackoverflow.com/)

好吧！这是个不错的小突破，对吧？让我们再复习一遍！

先说数组！这些小 buggers 在 vanilla 以及前端和后端框架中都有大量的实用程序。

定义:JavaScript `**Array**`类是一个全局对象，用于构造数组；它们是高级的、类似列表的对象。
来源:[https://developer . Mozilla . org/en-US/docs/Web/JavaScript/Reference/Global _ Objects/Array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)

更简单地说，它是一个可以用数组方法迭代的数据容器。它可以保存和存储基本上任何原始类型，以及对象和其他数组！对于前端框架，您将使用它来存储从 API 调用的内容，比如帖子或评论。(有很多例子，但这是我想到的第一个)。对于后端，您将类似地使用它，但是当您到达那里时，您将看到它的其他用途。

数组是什么样子的？

![](img/35fd7a24f7b55b468641a2a1d6120e73.png)

来自[https://www.programiz.com/javascript/array](https://www.programiz.com/javascript/array)的数组示例

我想看看这篇文章中所有关于数组的例子:[https://www.programiz.com/javascript/array](https://www.programiz.com/javascript/array)

此外，在这一点上，我想真正促进你提前学习，或者除了我分享的内容之外，你还可以自己学习，所以这里有一个“备忘单”，给你提供大量关于各种事情的基本信息 JavaScript:[https://htmlcheatsheet.com/js/](https://htmlcheatsheet.com/js/)

为了结束我们学习前端的这一部分，我将用一些材料来结束，以了解更多关于数组的知识，然后我会给你们一些信息，让你们看一个超级复杂的主题，即 JavaScript 中的对象。

关于数组的更多信息:
[https://www.javascripttutorial.net/javascript-array/](https://www.javascripttutorial.net/javascript-array/)
[https://javascript.info/array](https://javascript.info/array)
[https://www.geeksforgeeks.org/arrays-in-javascript/](https://www.geeksforgeeks.org/arrays-in-javascript/)

JavaScript 中的对象:

这就是它让人费解的原因:[https://gomake things . com/everything-is-an-object-In-JavaScript/#:~:text = In % 20 JavaScript % 2C % 20 everything % 20 is % 20 an，Functions%20are%20objects。&text = length % 20 works % 20 因为%20arrays%20are，property % 20 of % 20 the % 20 array % 20 object](https://gomakethings.com/everything-is-an-object-in-javascript/#:~:text=In%20JavaScript%2C%20everything%20is%20an,Functions%20are%20objects.&text=length%20works%20because%20arrays%20are,property%20of%20the%20Array%20object)。

[https://www.w3schools.com/js/js_objects.asp](https://www.w3schools.com/js/js_objects.asp)
[https://developer . Mozilla . org/en-US/docs/Web/JavaScript/Reference/Global _ Objects/Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)
[https://eloquentjavascript.net/06_object.html](https://eloquentjavascript.net/06_object.html)(很棒的资源！阅读！)

我想感谢你，并祝贺你，如果你已经在这个系列中走了这么远！我期待着从我们现在的位置继续下去，直到我们完成整个 JavaScript 栈，以及一些有趣的主题，如 TypeScript、GraphQL、UI 组件库等等！