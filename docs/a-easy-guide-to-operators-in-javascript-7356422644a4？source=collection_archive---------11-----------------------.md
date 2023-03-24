# JavaScript 操作符简易指南

> 原文：<https://medium.com/nerd-for-tech/a-easy-guide-to-operators-in-javascript-7356422644a4?source=collection_archive---------11----------------------->

![](img/1fd7edddd091262933c7bd747e4bfbcc.png)

奥斯卡·伊尔迪兹在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

在[的上一篇文章](https://fufubyte.medium.com/types-in-javascript-8aef9384d1bd)中，我们看了 JavaScript 必须提供的类型。但我们没有谈到的是它们的操作符，基本上是什么使一个价值作用于或被作用于产生一个结果。这些包括但不限于+(正)、-(负)等。你可能会想，字符串类型可以有操作符吗，答案是肯定的**是的**。

**注意:这些不是标准化(官方的)，这只是我自己试图构建它的方式。**

# 操作员的类别

JavaScript 中有三类运算符

*   一元运算符
*   二元运算符
*   三元运算符

这些主要是由于他们对他人的功能，他们的行为方式。

**注意:并非所有运算符都是符号**

## 一元运算符

这些是作用于操作数(值)的运算符，即它们作用于单个值。示例**类型**、 **-** (减号)、**！**(感叹号)等。关键字 **typeof** 产生一个字符串值，命名你提供给它的值的类型。

```
console.log(typeof 3)// Number
console.log(typeof "")// String
console.log(-10)// -10
console.log(!true) // false
```

## 二元运算符

这些运算符作用于两个操作数(值)。这是大多数运营商陷入的困境。它们很常见，有些你可能知道。例+(加号)、-(减号)。

```
console.log(2+2) // 4 
console.log(3-1) // 2
console.log(1-2) // -1
```

## 三元运算符

这些运算符对三个操作数(值)进行运算。这可能不是你第一次听说这个术语。猜猜看…是的，这是个问号(**？**)和冒号(:)。目前，它是唯一具有这种功能的操作符，我们将在本文后面了解它是如何操作的。

```
console.log(true ? "JavaScript" : "Java" ) // JavaScript
```

# 运算符的类型

我将在下面讨论以下操作符。这些不是唯一的运算符类型

*   算术
*   逻辑学的
*   比较
*   有条件的

## 算术

这些是执行算术运算的运算符。其中包括加法(+)、减法(-)、除法(/)、乘法(*)。如果你学过基本的数学，你就会知道这些符号和它们的含义，但是还有一些你不知道的算术符号的功能。

> **字符串也可以使用+符号。这叫做串联。**

```
console.log("Why" + "me")// Whymeif you need space you will have to add it.console.log("Why" + " me")// Why me
```

例如，如果你想找到除法运算的余数，或者如果你想从浮点数(十进制数， **JavaScript 没有这种类型**)中得到一个整数(整数)。好了不再等了，我介绍一下**模数** (%)和**底数** (//)的操作。

**Floor** 运算符是一个双正斜杠(//)，基本上忽略小数点后的任何值，不管该值是大于. 5 还是小于. 5。

```
console.log(3.5) // 3
console.log(5.2) // 5
```

**模数**运算符是一个百分号(%)，返回除法运算的余数。当您想知道一个数字是偶数还是奇数时，这很有用。

```
console.log(3%2)// 1
console.log(5%3)// 2
```

## 逻辑运算符

这些操作布尔值运算符。它们包括**和**、**或**和**而不是**。在 JavaScript 中，它们用符号来表示。所以**和(& & )** ，**或者(||)** ，**不是(！).**

**& &** 运算符代表逻辑**和**。它是一个二元运算符(即对两个值进行运算)，只有当给它的两个值都为真时，它的结果才为真。

```
console.log(false && false)// true
console.log(true && false)// false
console.log(2 < 4&& 3 > 4)// false
console.log(2 >= 4 || 8 == 8)// true
console.log(9 == "9" || 8 > 19)// true
console.log(9 === "9" && 4 > 5)// false
```

## **比较运算符**

>和< signs are the traditional symbols for “is greater than” and “is
分别小于”。它们是二元运算符。应用它们会产生一个布尔值，表明在这种情况下它们是否成立。
字符串也可以用同样的方法进行比较。

```
console.log(2 < 4)// true
console.log(2 > 4)// false 
console.log(4 >= 6)// false
console.log(5 <= 8 )// true
```

## 条件运算符

该运算符是一个三元运算符(对三个值进行运算)。它由一个问题和一个冒号组成。所以它的工作方式是，如果为真，冒号(:)前和问号(？)将被返回，如果为 false，冒号后的值将被返回。

```
console.log(false ? "Python" : "JavaScript" )// JavaScript
```

这就像是一个简单的方法，写出一个没有任何其他条件的条件语句。

## 自动类型转换

JavaScript 尽力接受你给它的几乎任何程序，做奇怪事情的事件程序。

```
console.log(8 * null) // 0
console.log(“5” - 1 ) // 4
console.log(“5” + 1) // 4
console.log(“five” * 2)// NaN
console.log(false == 0)// true
```

当一个操作符被应用于“错误”类型的值时，JavaScript 会使用一组通常不是您想要或期望的规则
悄悄地将该值转换为它需要的类型。这叫做**类型转换**。当没有以明显的方式映射到一个数字的东西(比如
“五”或者未定义)被转换成一个数字时，你就得到值 n an。

当使用 **==** 比较同类型的值时，结果很容易被
预测。如果两个值相同，那么结果应该为真**，除了 NaN 的情况。但是当类型不同时，JavaScript 使用一组复杂而混乱的规则来决定该做什么。在大多数情况下，它只是试图将其中一个值转换为另一个值的类型。但是，当 null 或 undefined 出现在运算符的任一侧时，只有当两侧都为 null 或 undefined 时，才会生成 true。**

```
console.log(null == undefined);
// → true
console.log(null == 0);
// → false
```

## **短路**

**逻辑运算符&&和||以一种特殊的
方式处理不同类型的值。它们会将左侧的值转换为布尔类型，以便
决定要做什么，但是根据运算符和
转换的结果，它们会返回原始的左侧值或右侧值。**

**当**为真**时， **||** 操作器将把该值返回给左侧**操作器，当**为假**时，返回右侧**操作器。******

```
console.log(true || false)// true
console.log(false || true)// false
```

**这对布尔值无效，但对其他值有效。将字符串和数字转换为布尔值的规则规定，0、null、未定义、空字符串、NaN 计为**假**，其他计为**真**。**

```
console.log( 0 || 'I will return')
// I will return
console.log(null || 5)
// 5
console.log('' || 1000)
// 1000
console.log(8 || null)
// 8
console.log('I am not an empty string'|| 'It is well' )
// I am not an empty string
```

****& &** 运算器在**为真**时将数值返回到**右**，在**为假**时将数值返回到**左**。**

```
console.log(true || false)
// false
console.log(false && true)
// true
console.log(8 && 100)
// 100
console.log('' && 'Okay')
// ''
console.log( && )
```

**当你有一个动态设置的变量时，这是很有用的，如果这个值是一个空字符串、null 或 undefined，所有这些都计算为一个**falsy**；右边的值将是默认值。**

```
if the user variable is falsy i.e false. The guest string is used instead, otherwise user will return.

console.log(user || 'guest')
// guest
```

> **"一个 **falsy** (有时写成**false**)值是一个当在[布尔](https://developer.mozilla.org/en-US/docs/Glossary/Boolean)上下文中遇到时被认为是 false 的值。"— Mozilla 开发者网络**

**当一个值评估为真时，它被视为真，否则它被视为**假。这是一个口语术语，所以它不是一个技术术语。评估为 **falsy** 的值为 0，0n(Bigint)、null、NaN、false、undefined 和空字符串(" ")。****

**还有一些类型的操作符没有在本文中讨论，但是您可以在这里找到。另外，[操作符](https://www.dummies.com/web-design-development/javascript-operator-precedence/)中的[优先级](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Operator_Precedence)也是一个东西，所以确保你了解它们以防止意外的错误**

# **摘要**

**我们已经学习了 JavaScript 的操作符，基于它们作用于多少操作数**

*   **一元运算符**
*   **二元运算符**
*   **三元运算符**

**基于它们作用的类型**

*   **算术**
*   **逻辑学的**
*   **比较**
*   **有条件的**

**我们还看短路和自动转换。所以要小心自动转换，这是引入错误的一个途径。**

**编码快乐！！！**