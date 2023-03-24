# 理解 JavaScript 奇怪的部分

> 原文：<https://medium.com/nerd-for-tech/understand-the-weird-parts-of-javascript-c7f2e9e1ae47?source=collection_archive---------1----------------------->

javascript 如何在内部工作的简单解释

![](img/8ec28836fecb91c3fbf7a1a8a8392430.png)

Artem Sapegin 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Javascript 是开发人员最喜欢也最讨厌的编程语言。这是因为大多数初级开发人员不知道它在内部是如何工作的，并且发现它很难调试和执行。

当我开始使用 javascript 时，我不理解很多事情，并且发现很难知道为什么我的代码会这样。处理未定义的错误或定义变量的范围是困难的。

但是一旦我找到了这是如何发生的以及为什么会发生的答案，它就成了我最喜欢的编程语言。

在这篇文章中，我将向您展示 Javascript 的方式和原因，以及像提升和作用域链这样的奇怪行为。

![](img/1cdb9781e16aef9e95a056f2f426f44f.png)

照片由[本·怀特](https://unsplash.com/@benwhitephotography?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 执行上下文

> **JavaScript 代码是如何执行的？**

在 javascript 引擎中，代码执行的环境称为执行上下文。每当 Javascript 引擎执行代码时，就会创建执行上下文。

**执行上下文分两个阶段工作:**

*   **创建阶段**
*   **执行阶段**

在**创建阶段，**脚本第一次执行，并创建一个全局执行上下文。将执行以下任务:

*   在 web 浏览器中创建名为 window 的全局对象
*   创建一个内存堆来存储所有变量和函数

还创建了一个调用栈来跟踪被调用的函数，并且全局执行上下文存在于该栈中。

在**执行阶段**，javascript 引擎通过从内存中为变量赋值并执行函数调用来逐行执行代码。

每当进行函数调用时，javascript 引擎都会为该函数创建一个新的执行上下文，称为函数执行上下文，它会被添加到调用堆栈中

在上面的示例中，输出是:

```
test
undefined
10
```

> 函数在声明之前是如何执行的？

让我们一行一行地理解上面的例子。

如前所述，执行上下文声明一个全局上下文，将所有变量和函数存储在内存中。

首先在创建阶段，变量在内存中声明，并初始化为未定义。因此，最初它记录的“x”是未定义的。

但是函数没有被初始化为未定义的，它们是在创建阶段声明的，因此我们能够调用函数 test 并执行它。

# 提升

当我们在创建阶段执行函数和变量并执行。所有变量和函数声明都被移动到当前作用域的顶部。

在创建阶段，javascript 引擎将变量放在内存中，并用未定义的值初始化所有变量。

函数也在创建阶段初始化，声明被移到脚本的顶部。

# 范围链

在 javascript 中，一旦理解了执行上下文的概念，就很容易理解变量的范围。

正如我们前面讨论的，无论何时调用一个函数，它都会在调用堆栈中创建自己的执行上下文，并使用自己的内存(称为本地内存)。

> 我们如何访问本地内存之外的变量？

让我们考虑这个例子:

上述示例的输出是:

```
10
100
```

> 在函数 test2 中，当我们使用 console.log(x)时，首先在本地内存中搜索变量，当没有找到时，它在父作用域 test1 中查找 x，其中 x 被定义为 100。类似地，对于 test1，它在它的父作用域中寻找 x。

执行上下文、提升和作用域链是理解 Javascript 的良好开端，但是如果你想深入学习 Javascript，还有更多的主题需要讨论，我将很快添加这些主题。

***感谢你一直读到最后，希望这篇文章对你有用。请关注我，获取更多此类科技文章。万事如意！！！***

# 参考

[](https://www.javascripttutorial.net/javascript-execution-context/) [## 通过示例理解 JavaScript 执行上下文

### 摘要:在本教程中，您将学习 JavaScript 执行上下文，以深入理解 JavaScript 如何…

www.javascripttutorial.net](https://www.javascripttutorial.net/javascript-execution-context/) [](/@happymishra66/execution-context-in-javascript-319dd72e8e2c) [## 执行上下文、作用域链和 JavaScript 内部

### 在本帖中，我们将讨论什么是 JavaScript 中的执行上下文和作用域链。我们还将检查内部…

medium.com](/@happymishra66/execution-context-in-javascript-319dd72e8e2c)