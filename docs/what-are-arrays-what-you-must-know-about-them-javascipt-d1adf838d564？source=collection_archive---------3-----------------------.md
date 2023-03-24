# 什么是数组&关于数组你必须知道什么？🚛(Java 脚本)

> 原文：<https://medium.com/nerd-for-tech/what-are-arrays-what-you-must-know-about-them-javascipt-d1adf838d564?source=collection_archive---------3----------------------->

> 什么是数组？

*一个* ***数组*** *是一个* ***数据结构*** *，其中可以存储一个集合* ***元素*******相同的*** *数据类型并且有一个***。***

**我们已经听过了，对吧？为了更好的理解，我们举个例子。**

**假设你有一些不同颜色的玩具球(粉色、绿色、蓝色、黄色……应有尽有)。**

**![](img/40946d3808f1f33f85e7f19025fa6b9c.png)**

**我要求你收集所有的蓝色球，并把它们存放在一个只能装 **7** 球的盒子里。**

**![](img/bc4d751eae695521f1082c840327a2bb.png)**

**图片来源:ebay**

**现在把它想象成，那些球是**元素**，它们的颜色是它们的**数据类型**(字符串、数字、浮点、布尔等)。).所以现在盒子是**数组**(数据结构)并且有一个**固定大小的**(只能包含 7 个球)。**

**让我们用这个类比来代替上面的定义:**

***A****box****是一个* ***数据结构*** *，它可以存储一个集合* ***元素/球*******相同数据类型/颜色*** *(蓝色)并有一个****

**希望你现在明白了。🎃**

**这是 C、Java、C#等语言的一个常见定义。让我们用 javascript 对它们多了解一点:**

1.  **数组元素存储在连续的内存位置。**
2.  **二维数组元素被逐行存储在后续的存储位置。**
3.  **数组名保存对起始元素地址的引用。**
4.  **̶a̶r̶r̶a̶y̶̶s̶i̶z̶e̶̶s̶h̶o̶u̶l̶d̶̶b̶e̶̶m̶e̶n̶t̶i̶o̶n̶e̶d̶̶i̶n̶̶t̶h̶e̶̶d̶e̶c̶l̶a̶r̶a̶t̶i̶o̶n̶.̶:“在 JavaScript 中，数组不必是固定大小的，也不必在使用前声明长度。”**
5.  **a̶r̶r̶a̶y̶̶e̶l̶e̶m̶e̶n̶t̶s̶̶s̶h̶o̶u̶l̶d̶̶b̶e̶̶o̶f̶̶s̶a̶m̶e̶̶d̶a̶t̶a̶̶t̶y̶p̶e̶.̶*“同样在 JavaScript 数组中可以有不同大小的元素。”***

**此外，我将讨论 JavaScript 中你应该知道的 8 个重要的数组方法。**

# ****01。Array.map( )****

****map()** 方法**创建一个新的数组**，其中填充了调用函数的结果，该函数对调用数组中的每个元素执行一些操作。**

**语法:**

```
**let new_array = arr.map(function callback( currentValue[, index[, array]]) { 
    // return element for new_array 
}[, thisArg])**
```

# **02. **Array.reduce( )****

**与 **array.map( )** 不同， **reduce()** 方法对数组的每个元素执行 **reducer** 函数(您提供的),并返回单个输出值。**

**语法:**

```
**arr.reduce(callback( accumulator, currentValue[, index[, array]] )[, initialValue])**
```

# **03. **Array.filter( )****

****filter()** 方法**创建一个新的数组**，其中包含所有通过函数中提供的条件的元素。**

**语法:**

```
**let newArray = arr.filter(callback(currentValue[, index[, array]]) {
  // return element for newArray, if true
}[, thisArg]);**
```

# **04. **Array.pop( )****

****pop()** 方法从数组中移除最后一个**元素并返回该元素。这个方法改变数组的长度****

**语法:**

```
**arrName.pop()**
```

# **05. **Array.push( )****

**方法将零个或多个元素添加到数组的末尾。它返回数组的新长度。**

**语法:**

```
**arr.push([element1[, ...[, elementN]]])**
```

# **06. **Array.slice( )****

****slice()** 方法将一个数组的一部分的 **shallow** 副本返回到一个新的数组对象中，该对象是从该数组中的项的开始索引到结束索引(不包括结束)选择的。**

**语法:**

```
**arr.slice([start[, end]])**
```

# **07. **Array.sort( )****

****filter()** 方法**创建一个新数组**，其中所有元素都通过了函数中提供的条件。**

**语法:**

```
**arr.sort([compareFunction])**
```

# **08. **Array.reverse( )****

****reverse()** 方法将数组**反转到**的位置。**

**语法:**

```
**arr.reverse()**
```

**[](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array#) [## 排列

### JavaScript 数组类是一个全局对象，用于构造数组；哪些是高层次的…

developer.mozilla.org](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array#)**