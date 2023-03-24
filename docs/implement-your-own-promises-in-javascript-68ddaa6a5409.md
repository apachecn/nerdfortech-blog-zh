# 用 JavaScript 实现你自己的承诺

> 原文：<https://medium.com/nerd-for-tech/implement-your-own-promises-in-javascript-68ddaa6a5409?source=collection_archive---------2----------------------->

![](img/f647edf9748495392f469292b391519a.png)

照片由[郭佳欣·阿维蒂斯扬](https://unsplash.com/@kar111?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Promises 是 JavaScript 中最基本的概念之一，我们都在应用程序中多次使用过，但是我们能实现自己的 Promise API 吗？别担心，它没有看起来那么复杂。在这篇文章中，我们将自己实现一个基本的 Promise API。

## 什么是承诺？

> [**Promise**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) 对象表示异步操作的最终完成(或失败)及其结果值。

它可以处于以下三种状态之一:

*   挂起，操作正在进行时的初始状态
*   履行，定义操作成功
*   拒绝，表示操作失败

> **注**:当一个承诺被履行或拒绝时，就说这个承诺被**解决了**。(我们将在本文中大量使用这个术语)

## 你如何使用承诺？

对于实现承诺，让我们首先看看它的框架，本质上是它接受的输入，以及它公开的方法。

它有一个接受回调的构造函数，以及像 then、catch 和 finally 这样的方法。

使用 JavaScript Promise

# 1.定义骨架

我们首先定义我们的承诺类 **MyPromise** 。

## 以下属性在构造函数中定义:

1.  `state` :可以是`PENDING`、`FULFILLED`或`REJECTED`
2.  `handlers`:存储 then、catch、finally 方法的回调。*(处理程序只有在承诺完成时才会执行。)*
3.  `value`:解析或拒绝值。

**我的承诺骨架**

# **2._resolve()和 _reject()方法实现**

**`_resolve()`或`_reject()`分别将承诺的`state`设置为`FULFILLED` 或`REJECTED` ，更新`value`属性并执行附带的处理程序。**

> ****注意**:如果我们试图就一个已经达成的承诺给`_resolve()`或`_reject()`打电话，什么也不会发生。**

**想知道上面代码中的`isThenable(value)`是什么？**

**对于一个承诺被另一个承诺解决/拒绝的情况，我们必须等待它完成，然后处理我们当前的承诺。**

## **isThenable()函数实现**

**一个`isThenable` 函数检查 value 是一个`MyPromise`的实例还是一个包含`then`函数的对象。**

# **3.然后()方法**实现****

**`then()` 方法以两个参数作为回调`onSuccess`和`onFail`。`onSuccess` 在承诺兑现时调用，而`onFail` 在承诺被拒绝时调用。**

> ****“记住承诺是可以被锁链锁住的”。****
> 
> ****承诺链接**的本质是`then()`方法返回一个**新的承诺**对象**。承诺就是这样被锁住的。这在我们需要连续执行两个或更多异步操作的场景中特别有用，其中每个后续操作在前一个操作成功时开始，并带有前一个步骤的结果。****

**传递给`then()`的回调使用`addHandlers`函数存储在`handlers`数组中。处理程序是一个对象`{onSuccess, onFail}`,当一个承诺完成时，它将被执行。**

**我们对`then()`的实现如下所示:**

**然后是功能实现**

# **4.catch()方法实现**

**`***catch()***` ***是利用*** `***then()***` *实现的。*我们调用`then()`方法，将`onSuccess`回调作为`null`，并将`onFail`回调作为第二个参数传递。**

# ****5。finally()方法实现****

**在我们开始实施`finally()`方法之前，让我们先了解它的行为*(我自己也花了一些时间来理解)*。**

**来自 MDN 文档:**

> **`finally()`方法返回一个`[Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)`。当承诺完成时，即履行或拒绝时，执行指定的回调函数。这为代码提供了一种运行方式，无论承诺是成功实现还是在处理完`Promise`后被拒绝。**
> 
> **`finally()`方法与调用`.then(onFinally, onFinally)`非常相似，但是有一些不同之处:**
> 
> **当内联创建一个函数时，您可以传递它一次，而不是被迫声明它两次，或者为它创建一个变量**
> 
> **与`Promise.resolve(2).then(() => {}, () => {})`(将用`undefined`解析)不同，`Promise.resolve(2).finally(() => {})`将用`2`解析。**
> 
> **同样，与`Promise.reject(3).then(() => {}, () => {})`(将用`undefined`)不同，`Promise.reject(3).finally(() => {})`将用`3`拒绝。**

**`finally()`方法返回一个承诺，该承诺将用以前的`fulfilled`或`rejected`值进行结算。**

## **摘要**

**我们模仿承诺的基本实现。除了作为实例方法的`then()`、`catch()`、`finally()`方法之外，还有很多其他的方法。还有一些静态方法，我会在以后的文章中介绍。**

**我希望你喜欢这篇文章。**

**点击查看 [Codepen 上的完整代码。](https://codepen.io/mayank_shubham/pen/abpEVJK?editors=0012)**