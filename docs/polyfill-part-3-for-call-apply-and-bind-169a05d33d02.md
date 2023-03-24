# 聚合填料第 3 部分🎥:对于 Call()，Apply()和 Bind()⏳

> 原文：<https://medium.com/nerd-for-tech/polyfill-part-3-for-call-apply-and-bind-169a05d33d02?source=collection_archive---------2----------------------->

![](img/78bcc359ecfcefc194464159f8d6ad2a.png)

亚历山德拉·奥尼索在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

嘿！你可能在面试中被问到过这个问题，也可能没有。不管是什么情况，多了解一些也无妨。所以在这里我将分享如何为**调用**、**应用、**和**绑定**方法创建 polyfill。

> ***咱们码！🤽‍♀***

## 调用的 Pollyfill():

让我们先从语法上看一下**function . prototype . call()**是如何工作的:

```
myFunc.call(context, arg1, /* …, */ argN)
```

所以，原来的**功能**。**原型**。**调用**函数需要一个上下文和逗号分隔的参数。

基于此，让我们构建自己的**调用**函数:

```
Function.prototype.**myCall** = function (context, ...args) { if (typeof this !== "function") {
     throw new Error(this + ".call is not a function");
  } context.tempRef = this;let **result** = context.tempRef(...args); **delete** context.tempRef; return **result**;};
```

## 应用的 Pollyfill():

让我们首先从语法上看一下**function . prototype . apply()**是如何工作的:

```
myFunc.apply(thisArg, argsArray)
```

所以，原来的**功能**。**原型**。 **apply** 函数接受一个上下文和一个参数数组。

基于此，让我们构建自己的**应用**函数:

```
Function.prototype.**myApply** = function (context, args) { if (typeof this !== "function") {
       throw new Error(this + ".apply is not a function");
  } context.tempRef = this; let **result** = context.tempRef(...args); **delete** context.tempRef; return **result**;};
```

> 它类似于**调用**的 polyfill，除了这里我们得到的是一个参数数组，而不是逗号分隔的参数。

## Bind()的 Pollyfill:

让我们首先从语法上看一下**function . prototype . bind()**是如何工作的:

```
myFunc.bind(thisArg, arg1, arg2, /* …, */ argN)
```

所以，原来的**功能**。**原型**。 **bind** 函数接受一个上下文和逗号分隔的参数。

基于此，让我们构建我们自己的**绑定**函数:

```
Function.prototype.**myBind** = function(**context**, ...**args**){
  if (typeof this !== "**function**") {
    throw new Error(this + ".bind is not a function");
  }
  let **callbackFn** = this; return function(){        
    **callbackFn**.call(**context**, ...[...**args**, ...**arguments**]);
  }
}
```

*如果你喜欢这个帖子，请点击掌声👏* *图标下方，关注我这里的*[](https://tanyas27.medium.com/)**了解更多！**

**下面跟随****poly fill****系列:**

*[](/nerd-for-tech/polyfill-for-array-map-filter-and-reduce-e3e637e0d73b) [## 用于数组映射()的 Polyfill、filter()和 reduce()💁

### 嘿！你可能在面试中被问到过这个问题，也可能没有。不管是什么情况，它不会伤害到…

medium.com](/nerd-for-tech/polyfill-for-array-map-filter-and-reduce-e3e637e0d73b) [](https://tanyas27.medium.com/polyfill-for-array-methods-part-2-foreach-81638691efe4) [## 数组方法的聚合填充第 2 部分🎥:forEach()

### 嘿！所以在之前的博客中，我分享了如何为 map、fill 和 reduce 数组方法创建 polyfill。以防你…

tanyas27.medium.com](https://tanyas27.medium.com/polyfill-for-array-methods-part-2-foreach-81638691efe4) [](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/call) [## function . prototype . call()-JavaScript | MDN

### 注意:除了 call()接受一个参数列表，而 apply()接受一个…

developer.mozilla.org](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/call) [](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/apply) [## function . prototype . apply()-JavaScript | MDN

### 注意:除了 call()接受一个参数列表，而 apply()接受一个…

developer.mozilla.org](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/apply) [](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind) [## function . prototype . bind()-JavaScript | MDN

### 函数创建了一个新的绑定函数。调用绑定函数通常会导致执行…

developer.mozilla.org](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind)*