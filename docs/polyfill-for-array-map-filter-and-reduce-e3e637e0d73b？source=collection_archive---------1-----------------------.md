# 数组方法的 poly fill:map()、filter()和 reduce()💁

> 原文：<https://medium.com/nerd-for-tech/polyfill-for-array-map-filter-and-reduce-e3e637e0d73b?source=collection_archive---------1----------------------->

![](img/48d0a3b4697ca58bc35a393b8e8fe2f3.png)

费德里科·贝卡里在 [Unsplash](/s/photos/wallpapers-hd?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

嘿！你可能在面试中被问到过这个问题，也可能没有。不管是什么情况，多了解一些也无妨。所以在这里我将分享如何为**贴图**、**滤镜、**和**减少**数组方法创建 polyfill。

> **我们来编码吧！**

## Array.map()的 Pollyfill

让我们首先从语法上看一下 **Array.map()** 是如何工作的:

```
let **newArray** = arr.map(**callback**(**currentValue**[, **index**[, **array**]]) {
  // return element for newArray, after executing something
});
```

因此，原来的 **Array.map** 函数将一个**回调**函数作为参数，这个回调函数可以有三个参数传递给它:
a .当前值
b .当前值的索引【可选】
c .数组【可选】

基于此，让我们构建自己的地图函数:

```
Array.prototype.myMap = function(**callbackFn**) {
  var arr = [];             
  for (var i = 0; i < this.length; i++) { 
    * /* call the callback function for every value of this array and       push the returned value into our resulting array
     */*
    arr.push(**callbackFn**(this[i], i, this));
  }
  return arr;
}
```

## Array.filter()的 Pollyfill

让我们首先从语法上看 Array.filter()是如何工作的:

```
let newArray = arr.filter(callback(currentValue[, index[, array]]) {
  // return element for newArray, if true
});
```

因此，原来的 **Array.filter** 函数将一个**回调函数**作为参数，这个回调函数可以有三个参数传递给它:
a .当前值
b .当前值的索引【可选】
c .数组【可选】

基于此，让我们构建自己的过滤函数:

```
Array.prototype.myFilter = function(**callbackFn**) {
  var arr = [];    
  for (var i = 0; i < this.length; i++) {
    if (**callbackFn**.**call**(this, this[i], i, this)) {
      arr.push(this[i]);
    }
  }
  return arr;
}
```

## Array.reduce()的 Pollyfill

让我们首先从语法上看 Array.reduce()是如何工作的:

```
arr.reduce(callback( accumulator, currentValue, [, index[, array]] )[, initialValue])
```

所以，原来的 **Array.reduce** 函数有两个参数:
1。一个**回调函数**作为一个参数，该回调函数可以有四个参数传递给它:
a .累加器
b .当前值
c .当前值的索引【可选】
d .数组【可选】

2.一个**初始值**。

基于此，让我们构建自己的 reduce 函数:

```
Array.prototype.myReduce= function(**callbackFn**, **initialValue**) {
  var accumulator = initialValue;for (var i = 0; i < this.length; i++) {
    if (accumulator !== undefined) {
      accumulator = **callbackFn**.call(undefined, accumulator, this[i],   i, this);
    } else {
      accumulator = this[i];
    }
  }
  return accumulator;
}
```

*如果你喜欢这个帖子，请点击掌声图标，关注我* [*这里*](https://tanyas27.medium.com) *了解更多！*

*下面跟随****poly fill****系列:*

[](https://tanyas27.medium.com/polyfill-for-array-methods-part-2-foreach-81638691efe4) [## 数组方法的聚合填充第 2 部分🎥:forEach()

### 嘿！所以在之前的博客中，我分享了如何为 map、fill 和 reduce 数组方法创建 polyfill。以防你…

tanyas27.medium.com](https://tanyas27.medium.com/polyfill-for-array-methods-part-2-foreach-81638691efe4) [](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map) [## Array.prototype.map()

### map()方法创建了一个新的数组，该数组填充了对…

developer.mozilla.org](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map) [](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter) [## Array.prototype.filter()

### 方法创建一个新数组，其中所有元素都通过了由提供的函数实现的测试。函数是一个…

developer.mozilla.org](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter) [](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce) [## Array.prototype.reduce()

### reduce()方法在数组的每个元素上执行一个 reducer 函数(您提供的),产生一个…

developer.mozilla.org](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce)