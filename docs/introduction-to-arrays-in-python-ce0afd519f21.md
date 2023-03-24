# Python 中数组的介绍

> 原文：<https://medium.com/nerd-for-tech/introduction-to-arrays-in-python-ce0afd519f21?source=collection_archive---------2----------------------->

![](img/e0a6252d7a5bbf708e1d65bc97fb47f7.png)

由[穆罕默德·拉赫马尼](https://unsplash.com/@afgprogrammer?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/array-programming?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

# Python 中的数组

在 Python 中，数组允许存储许多相同类型的值。上面几行经常被误解为列表；然而，python 数组与列表不同。让我们看看什么是数组，以及如何实现它们。

![](img/b7a0566911db34c0cad6c51791eef84d.png)

# 什么是数组

计算机编程中用于保存同类数据的静态数据结构称为数组。

对于数据科学来说，数组是 Python 中最重要的一种数据结构，可用于多种目的，例如实现其他数据结构，如堆栈或队列。

> 使用数组时需要注意的一点是，每个元素都必须是相同的类型，比如全是整数或全是浮点数等。

# 使用数组的优势

数组数据结构的优点是:

*   阵列能够有效地管理非常大的数据集。
*   阵列中的计算和分析速度更快
*   它用于用一个名称表示许多相同类型的数据元素。
*   它能够实现更多的数据结构，如链表、栈、队列、树和图。
*   矩阵用二维数组表示。

# 使用数组的缺点

数组数据结构的缺点是:

*   数组的大小一旦定义就无法更改。它无法增加或减少分配给它的内存。
*   由于元素存储在连续的内存区域中，在数组中插入和删除项目非常困难。
*   必须预先知道要存储在数组中的项目数。
*   由于数组的大小是有限的，如果我们分配的内存比需要的多，我们就会浪费内存空间。如果我们分配的内存比需要的少，就会出现问题。

# 数组的应用

除了广泛用于编程之外，数组还有其他应用，它可以用于:

*   存储相同数据类型的数据元素。
*   使用一个名称维护多个变量名。
*   使用不同的排序技术对数据元素进行排序，如冒泡排序、插入排序、选择排序等
*   执行矩阵运算。
*   实现其他数据结构，如堆栈、队列、堆、哈希表等。

# 数组示例

为了演示数组是如何工作的，我们将使用 NumPy 的 array()函数将一个`list`转换成一个`array`。

为此，首先，我们创建一个基于整数的列表，然后使用 array()函数将其转换为数组。复制并粘贴以下内容

```
import numpy as npinteger_list = [1, 2, 3, 4, 5, 6, 7, 8, 9]to_array = np.array(integer_list)
print(to_array)
```

输出: **([1，2，3，4，5，6，7，8，9])**

# 访问数组中的元素

数组中的元素被称为*索引号*。

现在，要获得第一个数组项的值:

```
names = ["Favour", "Kelvin", "Mary", "Jane"]z = names[0]print(z)
```

输出:**偏向**

# 数组长度

数组的长度是指数组中元素的数量。

为了找到一个数组的长度，我们使用了`len()`方法

```
names = ["Favour", "Kelvin", "Mary", "Jane"]z = len(names)print(z)
```

输出: **4**

# 循环数组中的元素

为了遍历数组中的所有元素，我们使用了`for in`循环

```
names = ["Favour", "Kelvin", "Mary", "Jane"]for z in names:
  print(z)
```

输出:

青睐凯尔文玛丽简

# 在数组中添加元素

要在数组中添加元素，我们使用`append()`方法

```
names = ["Favour", "Kelvin", "Mary", "Jane"]names.append("John")print(names)
```

输出: **['赞成'，'开尔文'，'玛丽'，'简'，'约翰']**

# 移除数组中的元素

要移除数组中的元素，我们使用`remove()` 方法。

```
names = ["Favour", "Kelvin", "Mary", "Jane"]names.remove("Jane")print(names)
```

输出:**[' favor '，' Kelvin '，' Mary']**

此方法仅移除指定值的第一个匹配项。例如

```
names = ["Favour", "Jane", "Kelvin", "Mary", "Jane"]names.remove("Jane")print(names)
```

输出: **['赞成'，'开尔文'，'玛丽'，'简']**

或者，您也可以使用`pop()`方法通过指定索引从数组中移除元素。

```
names = ["Favour", "Kelvin", "Mary", "Jane"]names.pop(1)print(names)
```

输出: **['恩宠'，'玛丽'，'简']**

另一个例子

```
names = ["Favour", "Kelvin", "Mary", "Jane"]names.pop(1)print(names)
```

输出: **['凯尔文'，'玛丽'，'简']**

# 结论

耶！！🥳:你能够坚持到这篇文章结束。

![](img/37ff84e0b0e582da8f3db966102f27ea.png)

我希望这能很好地介绍 Python 中的数组数据结构。祝你学习愉快！