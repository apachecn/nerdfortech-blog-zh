# Python 列表、集合和元组的异同。

> 原文：<https://medium.com/nerd-for-tech/differences-and-similarities-between-python-lists-sets-and-tuples-5f9975db6c2b?source=collection_archive---------1----------------------->

![](img/4d18ebfb770d627f75576495cff4d09b.png)

Muhannad Ajjan 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

列表、集合和元组是 python 中的数据结构。它们存储值(如字符串、数字，甚至其他列表、集合和元组！).它们的值用逗号分隔，并用括号括起来。

# 差异

1.  也许最普遍的区别是每个数据结构使用的括号类型。列表使用方括号，集合使用花括号，元组使用大家熟悉的圆括号。

```
>>> myList = [1, 2, 3]
>>> type(myList)
<class 'list'>
>>> myTuple = (1, 2, 3)
>>> type(myTuple)
<class 'tuple'>
>>> mySet = {1, 2, 3}
>>> type(mySet)
<class 'set'>
```

**类型**函数告诉传递给它的参数的类。您将值括起来的括号类型很大程度上定义了您正在创建的数据结构的类型。

2.列表和元组是有序的数据结构，而集合是无序的。简单地说，如果我要创建一个集合数据结构，并打印出结果，结果将不会是我最初输入的顺序。

```
>>> fruits = {'apple', 'banana', 'cherry', 'date'}
>>> print(fruits)
{'cherry', 'banana', 'apple', 'date'}
```

由于集合是无序的，因此不能通过索引来访问集合中的项目。列表和元组根本不会改变它们的顺序。

3.列表和集合是可变的，元组是不可变的。这意味着您不能向元组添加新元素。这可以从缺少向元组添加新项的方法中看出来[例如 append()、extend()、add()]。元组一旦创建，就不能更改或修改。尽管如此，还是有办法解决这个问题😏。其中一种方法是将数据结构从元组改变为列表，修改列表，然后将列表变回元组。

```
>>> small_numbers = (0, 1, 2, 3)
>>> edit_tuple = list(small_numbers)
>>> edit_tuple.append(4)
>>> small_numbers = tuple(edit_tuple)
>>> small_numbers
(0, 1, 2, 3, 4)
>>> type(small_numbers)
<class 'tuple'>
```

应该注意，列表可以修改它的项目，也可以添加项目。集合可以添加项目，但不能修改或更改其项目。

4.集合不允许重复项，列表和元组允许重复项。如果一个集合中的一个值被写入两次或更多次并打印出来，那么输出将只有这些重复值中的一个。

```
>>> colorList = ['red', 'orange', 'yellow', 'orange', 'red']
>>> colorTuple = ('red', 'orange', 'yellow', 'orange', 'red')
>>> colorSet = {'red', 'orange', 'yellow', 'orange', 'red'}
>>> colorList
['red', 'orange', 'yellow', 'orange', 'red']
>>> colorTuple
('red', 'orange', 'yellow', 'orange', 'red')
>>> colorSet
{'red', 'orange', 'yellow'}
```

5.集合不能包含可变对象。例如，如果您尝试在一个集合中输入一个列表，您会得到一个错误，告诉您您正在尝试将一个不可共享的类型放入该集合中。 *(unhashable 指基本可变)。*

```
>>> numbers = {1, 2, [3, 4, 5,], 6}
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: unhashable type: 'list'
```

# 类似

列表、元组和集合之间没有明显的相似之处。在陈述不同之处时，提到了三者中任何两者之间的相似之处。为了清楚起见，我将它们列举如下:

1.  列表和集合是不可变的数据结构。它们可以被修改。
2.  列表和元组是有序的。它们可以通过索引来访问
3.  列表和元组可以保存重复值。
4.  列表和元组可以像列表一样保存可变对象。

# 结论

这些不同的数据结构有不同的情况，可以用来优化您的代码。例如，如果你想写特定位置的坐标，一个元组会更好，因为没有两个位置可以有相同的坐标。

感谢阅读:)。