# 在 JavaScript 中调用带括号和不带括号的函数

> 原文：<https://medium.com/nerd-for-tech/invoking-function-with-and-without-parenthesis-in-javascript-9d104c805559?source=collection_archive---------8----------------------->

让我们从理解函数这个术语开始。如果你做过动态网站，那么你很有可能会遇到功能。

# **W** 帽子是什么功能？

> **功能是一组代码，执行一些任务。**

当您有一些代码行执行一个独特的任务时，function 允许您将它们分组到一个块中，并在需要时调用。在您需要执行此任务的范围内定义了函数。

让我们定义一个将两个数相加的函数。

*函数 add(a，b){
sum = a+b；
返回总和
}*

其中 ***函数*** 为关键字， ***添加*** 为函数名，****a&b***为函数参数，花括号内的代码为函数体。*

*现在，这个函数代码不会在 JavaScript 遇到它时被执行，而只会在我们调用带括号的函数名时被执行。*

**例如:* ***将(1，2)*** 相加，那么函数就会被调用，会返回 1 和 2 的和。*

*如果我们只调用' add '会发生什么。*

*例如:添加*

*让我们想一想。*

*只调用 add 就会给出函数定义。所以在这里我们会得到，*

**add(a，b){
sum = a+b；
返回总和
}**

*现在，让我们假设我们需要在点击按钮时提醒“Hello World”。所以同样的函数定义应该是:*

**函数 hello World(){
alert(" hello World ")
}**

*接下来我们定义按钮。*

**const BTN = getElementById(' submit-button ')**

*其中“提交按钮”是按钮 id。*

*接下来，我们添加一个按钮点击事件监听器。*

*AddEventListener 是一个 JavaScript 内置函数。它有 3 个参数——类型、功能和选项。*

**btn。AddEventListener('click '，helloWorld，false)**

*困惑为什么我们没有用括号调用我们的函数。原因是，如果我们用括号调用函数，JavaScript 将立即执行函数，但我们需要在单击按钮时调用函数。为此，我们需要传递函数定义。因此*

****btn。AddEventListener('click '，helloWorld，false)*** 是正确的实现方式*。**

**btn。AddEventListner('click '，helloWorld()，false) //错误**

*我们也可以实现上述场景，例如:*

**btn。AddEventListner('click '，()= > helloWorld()，false) // correct**

*让我们在点击按钮时调用我们的添加函数*

**btn。AddEventListner('click '，()= > add(1，2)，false)**

*附上同样的演示。*

*[**【https://codesandbox.io/s/function-invoking-l8g79】**](https://codesandbox.io/s/function-invoking-l8g79)*

*随意使用各种调用函数的方法。*

*快乐编码！*