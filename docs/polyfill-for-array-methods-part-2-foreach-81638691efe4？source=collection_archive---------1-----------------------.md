# 数组方法的聚合填充第 2 部分🎥:forEach()

> 原文：<https://medium.com/nerd-for-tech/polyfill-for-array-methods-part-2-foreach-81638691efe4?source=collection_archive---------1----------------------->

![](img/9d7c9aa4944eb7c43bbf60a1743e50b9.png)

照片由[Tine ivani](https://unsplash.com/@tine999?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

嘿！所以在之前的博客中我分享了如何为 **map** 、**fill、**和 **reduce** 数组方法创建 polyfill。如果你没有读过，这里有下面的链接👇

[](/nerd-for-tech/polyfill-for-array-map-filter-and-reduce-e3e637e0d73b) [## 用于数组映射()的 Polyfill、filter()和 reduce()💁

### 嘿！你可能在面试中被问到过这个问题，也可能没有。不管是什么情况，它不会伤害到…

medium.com](/nerd-for-tech/polyfill-for-array-map-filter-and-reduce-e3e637e0d73b) 

> **我们来编码吧！**

## **array . foreach()的 Polly fill**

让我们首先从语法上看一下 **Array.forEach()** 是如何工作的:

```
array1.forEach(**callback**(**element**[, **index**[, **array**]]) {
  //callback function code
}, **thisArg**);
```

所以，原来的 **Array.prototype** 。 **forEach** 函数以一个**回调**函数和 **thisArg/context** (执行`callbackFn`时用作`this`的值)作为参数。

我们的回调函数可以传入三个参数:
a .当前值
b .当前值的索引【可选】
c .数组【可选】

基于此，让我们构建自己的 ***forEach*** 函数:

```
Array.prototype.myForEach = function(**callbackFn, context**) {

  for (var i = 0; i < this.length; i++) { 
    * /* call the callback function for every value of this array with the context provided
     */*
    **callbackFn.call**(context, this[i], i, this));
  }
}
```

*如果你喜欢这个帖子，请点击掌声*👏*图标下面还有关注我* [*这里*](https://tanyas27.medium.com/) *了解更多！*

[](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach) [## array . prototype . foreach()-JavaScript | MDN

### forEach()按索引升序为数组中的每个元素调用一次提供的 callbackFn 函数。它不是…

developer.mozilla.org](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach) [](https://tanyas27.medium.com/) [## 塔尼娅·辛格-中等

### 阅读坦尼娅·辛格在媒体上的文章。软件工程师| ( JavaScript) |居住在印度|关注…

tanyas27.medium.com](https://tanyas27.medium.com/)