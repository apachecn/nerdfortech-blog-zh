# 关于现代 JavaScript 你必须知道的事情

> 原文：<https://medium.com/nerd-for-tech/things-you-must-know-about-the-modern-javascript-fe0ea470e375?source=collection_archive---------26----------------------->

![](img/95d309a829b733bbe100676b9f538ced.png)

关于现代 JavaScript 你必须知道的事情

**概述**

JavaScript 或 JS 是一种多范例语言，支持面向对象和函数式编程。这是在第一次发布后改进并发布的几个版本。JavaScript 拥有不同的数据类型，例如:

*   数字
*   BigInt
*   线
*   布尔代数学体系的
*   标志
*   对象(函数，数组，日期，正则表达式)
*   空
*   不明确的

**现代 JavaScript**

这里定义了现代 JavaScript 中的一些关键改进。

*让*

通常，let 关键字只在块内部可见。参考下面的代码。

```
Index is: 0
Index is: 1
Index is: 2
Index is: 3
Index is: 4
    console.log("'Length: "+temp);
                            ^ReferenceError: temp is not defined
```

但是，可以通过在块外使用 let 关键字来避免这种情况。参考下面的代码。

```
Index is: 0
Index is: 1
Index is: 2
Index is: 3
Index is: 4
Length: 4
```

*常量*

该关键字用于定义常数。通常，常量值不能进一步更改，但这对于对象和数组无效。这意味着，const 只保护不是对象或数组的变量。请参考以下代码。

```
temp=index;
            ^TypeError: Assignment to constant variable.
```

```
{ indices: 0 }
{ indices: 1 }
{ indices: 2 }
{ indices: 3 }
{ indices: 4 }
```

*冻结*

这用于避免特定变量的变化。但是，这只冻结一级对象。这意味着 freeze 关键字不能用于保护二级或内部对象不被更改。参考下面的代码。

```
{ name: 'Hasini', indices: { index: 0 } }
{ name: 'Hasini', indices: { index: 1 } }
{ name: 'Hasini', indices: { index: 2 } }
{ name: 'Hasini', indices: { index: 3 } }
{ name: 'Hasini', indices: { index: 4 } }
```

*去结构化*

在现代 JavaScript 中，可以用多种方式进行去结构化。下面定义了其中的一些。

当常量从同一个模块中获得时，我们可以用一行代码来定义它们。请参考以下代码。

DE 结构化的另一个实现是:

我们可以将一个对象传递给函数，但是函数只从对象中获取所需的参数来执行在对象上定义的特定任务。参考下面的代码。

```
Name: Hasini
Position: ASE
```

在这里，在 DE 结构化中，…可以用来用一行声明单独的数组(这对于对象是可能的)。参考下面的代码。

```
1
[ 2, 3, 4 ]
[ 1, 2, 3, 4 ]
```

*动态属性*

在现代 JavaScript 中，我们可以为对象内部的未知键设置值。参考下面的代码。

```
{ name: 'Hasini', tier: 'level-3' }
{ name: 'Hasini', stage: 'level-3' }
{ name: 'Hasini', type: 'level-3' }
```

*承诺*

这里，当我们调用一个函数时，如果它有一些结果，那么它承诺将结果传递给另一个函数(回调函数)。这意味着，如果结果出来了，那么它会自动调用回调函数并移交给它来完成剩下的工作。请参考下面的示例代码。

通常，我们使用承诺来包装内容，因为承诺内的内容需要一些时间来完成执行。当响应准备好时，它调用回调函数。之后，只有在完成之后，才会调用另一个名为 print(value)的函数。根据具体情况，您可以使用 finally 和 catch 语句。

不使用 then()，我们可以用另一个函数来实现，这个函数涉及到 *async* 和 *await* 关键字。这里 await 只在*异步*函数内部有效。

**参考文献**

*   [https://www.youtube.com/watch?v=Jc2iW4yVv38&t = 16s](https://www.youtube.com/watch?v=Jc2iW4yVv38&t=16s)
*   https://www.youtube.com/watch?v=XK_lB5-XzhQ
*   [https://developer . Mozilla . org/en-US/docs/Web/JavaScript/A _ re-introduction _ to _ JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/A_re-introduction_to_JavaScript)