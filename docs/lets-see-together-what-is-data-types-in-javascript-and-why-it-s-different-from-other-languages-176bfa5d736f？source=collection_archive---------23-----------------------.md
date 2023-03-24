# 我们一起来看看 JavaScript 中的数据类型是什么，为什么和其他语言不同？

> 原文：<https://medium.com/nerd-for-tech/lets-see-together-what-is-data-types-in-javascript-and-why-it-s-different-from-other-languages-176bfa5d736f?source=collection_archive---------23----------------------->

![](img/53bc63df46b02f272dcacb74c0b6eb33.png)

欧文·史密斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

所有编程语言都有某种可用的数据类型，并且它们在不同的语言之间是不同的。这些数据类型帮助我们理解任何变量包含的数据类型。

# **很高兴知道**

众所周知，JavaScript 是一种松散类型的动态语言。我的意思是，如果我们声明一个变量，变量的类型是根据值定义的，我们已经给它赋值了。

```
let dynamicTyped = "Foo"; // string typedynamicTyped = 100; // number typedynamicTyped = false; // boolean typedynamicTyped = {}; // object type
```

正如我们在上面的例子中看到的，如果我们改变了变量的值，它也改变了它的类型。

# **什么是数据类型？**

数据类型有助于我们理解任何变量持有的数据类型，因此我们可以使用它的值。

在 JavaScript 中，我们有以下可用的数据类型。

1.  原始数据类型
2.  结构化数据类型(非原始数据类型)

## **原始数据类型**

原始数据类型是那些保存单个值并且不可变的数据类型。共有 7 种原始数据类型:[字符串](https://developer.mozilla.org/en-US/docs/Glossary/String)，[数字](https://developer.mozilla.org/en-US/docs/Glossary/Number)， [bigint](https://developer.mozilla.org/en-US/docs/Glossary/BigInt) ，[布尔](https://developer.mozilla.org/en-US/docs/Glossary/Boolean)，[未定义](https://developer.mozilla.org/en-US/docs/Glossary/undefined)，[符号](https://developer.mozilla.org/en-US/docs/Glossary/Symbol)，以及[空值](https://developer.mozilla.org/en-US/docs/Glossary/Null)。

> 一个**可变对象**是一个对象，它的状态在它被创建后可以被修改。
> **不可变**是对象一旦创建，其状态就不能改变的对象。

下面是原始数据类型的列表-

1.  [**未定义**](https://developer.mozilla.org/en-US/docs/Glossary/undefined) **:** 已声明但尚未赋值的变量，它是未定义的。
2.  [**布尔**](https://developer.mozilla.org/en-US/docs/Glossary/Boolean)；布尔值根据需要只能有真值或假值。
3.  [**字符串**](https://developer.mozilla.org/en-US/docs/Glossary/String) **:** 字符串类型包含文本格式的任何值。
4.  [**数字**](https://developer.mozilla.org/en-US/docs/Glossary/Number) **:** 数字类型可以有数据整数值。
5.  [**bigint**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/BigInt)**:**表示大于 2⁵ -1 的整数。
6.  [**符号**](https://developer.mozilla.org/en-US/docs/Glossary/Symbol) **:** 这种数据类型可以称为**【字符串值】**。
7.  [**null**](https://developer.mozilla.org/en-US/docs/Glossary/Null) : null 是一种异常的数据类型。它接受单个值，但是如果我们输入 null，那么它将返回“object”。在计算机科学中，空值表示通常有意指向不存在或无效的对象或地址的引用

```
const undefinedType;                          // typeof undefinedconst stringType = "Foo";                     // typeof stringconst numberType = 12345;                     // typeof numberconst booleanType = true;                     // typeof booleanconst bigIntType = BigInt(9007199254740991n); // typeof bigintconst symbolType = Symbol("Symbol");          // typeof symbolconst nullType = null                         // typeof object
```

## **结构化数据类型(非原始数据类型)**

结构化数据类型是用于创建数据的结构化格式的数据类型。像:new Object()、new Array()、new Map()、new Set()都是结构化数据类型。函数在 JavaScript 中也被称为一等公民，它也属于这一类。

1.  [**对象**](https://developer.mozilla.org/en-US/docs/Glossary/Object) :凡是用 new 关键字实例化的都是对象的类型。
2.  [**函数**](https://developer.mozilla.org/en-US/docs/Glossary/Function) :函数是一段代码片段，可以被其他代码调用，也可以被自身或引用该函数的变量调用

```
const objectVar = new Object(); // typeof objectconst objectLitteral = {};      // typeof objectconst arrayVar = new Array();   // typeof objectconst arrayLitteral = [];       // typeof objectconst setVar = new Set();       // typeof objectconst mapVar = new Map();       // typeof object
```

在上面的例子中，我们看到了定义对象类型数据的不同方法，对于每一种方法，如果我们使用 typeof，那么它将返回对象。但是如果我们需要显式地检查数组、集合和映射类型呢？让我们看看下面的例子。

```
// different ways to implicitly check array type
Array.isArray(arrayVar)         // true
Array.isArray(arrayLitteral)    // truearrayVar.constructor === Array; // truearrayVar instanceof Array;      // true// Checking set type explicitly
const setVar = new Set();
setVar instanceof Set;          // true// Checking map type explicitly
const mapVar = new Map();
mapVar instanceof Map;          // true
```

希望这对你有所帮助。快乐学习！:)