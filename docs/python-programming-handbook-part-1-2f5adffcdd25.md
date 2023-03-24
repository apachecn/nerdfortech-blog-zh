# Python 编程手册第 1 部分

> 原文：<https://medium.com/nerd-for-tech/python-programming-handbook-part-1-2f5adffcdd25?source=collection_archive---------3----------------------->

Python 是许多 web 开发人员、机器学习实践者使用的最广泛、最强大的编程语言之一……列表还在继续，但是当我们使用 python 编程语言时，我们是否使用了它提供的所有功能！！。这是研究 python 中一些强大但广泛使用的编程实践的系列文章的第一篇。

![](img/37e3b7dcc5e4f8f8d981f00d10796647.png)

在本文中，我们将重点关注以下 python **数据结构**。我们不会深入这些数据结构的基础知识，所以那些不熟悉 **python 2** 或 **python 3** 的人可能会发现理解本文中解释的一些概念有困难。

*   **列表**
*   **元组**
*   **字典**
*   **设置**

**元素数组**

最基本和最广泛使用的可变和混合类型数据结构是列表。考虑到你们大多数人都习惯于处理列表数据类型，让我们直接进入列表理解。

> 列表理解和生成器表达式是在 python 中构建元素序列的两种快速方法。在编程社区中，列表理解一般称为 ***列表组合*** ，而生成器表达式称为***gen-exps****。*

**使用列表理解构建列表**

```
symbols = "abcdefg"
lis1 = [ord(sym) for sym in symbols]
print(lis1)>>> [97, 98, 99, 100, 101, 102, 103]
```

我的目标是构建一个包含上述符号“ **abcdefg** ”的 Unicode 替换的列表。上面的例子显示了使用列表理解来构造这样的列表。

> *在 python 代码中，像* ***{ }，( )，[ ]*** *这样的符号成对出现时会忽略换行符。因此，您可以构建多个行列表 comps，而无需使用* ***\*** *换行符。*

**列表理解的其他一些例子**

**例 1** :过滤掉大于 100 的 **Unicode 值**。

```
lis2 = [ord(sym) for sym in symbols if ord(sym) > 100]
print(lis2)>>>[101, 102, 103]
```

**例 2** :笛卡尔积

```
lis3 = [(student, subject) for student in students for subject in subjects]
print(lis3)>>>[('jon', 'science'), ('jon', 'maths'), ('jon', 'history'),   ('doe', 'science'), ('doe', 'maths'), ('doe', 'history'), ('jack', 'science'), ('jack', 'maths'), ('jack', 'history')]
```

**元组和生成器表达式**

生成器表达式类似于列表理解，但是生成器表达式通过一个接一个地产生条目来节省内存，而不是构建一个完整的列表来提供给构造函数。

```
tup1 = tuple(ord(sym) for sym in symbols)
print(tup1)>>>(97, 98, 99, 100, 101, 102, 103)
```

一些 python 入门教材将列表称为可变数据类型，将元组称为不可变数据类型。但是元组远不止这些，现在让我们来看看元组可以被使用的领域。

**用作记录的元组**

元组广泛用于将值存储为记录，下面给出了一个将元组用作记录的示例。

```
City = ("Tokyo", "Delhi", "New York")
Student = ("doe", "john", "jack")
student_list = [('john', 1), ("doe", 2), ('jack', 3)]

for student in student_list:
    print('%s-%s' % student)>>>john-1
doe-2
jack-3
```

**元组解包**

提取存储在一个元组中的值被称为**元组解包**，解包元组值的方式有很多，但并行执行是元组解包最有效、最简单的方式之一。

```
co_ordinates = (1.0, 2.0, 3.0)
x, y, z = co_ordinates
print(x, y, z)>>>1.0 2.0 3.0
```

元组示例的另一种方式是解包，即在函数中使用 ***args** 。

```
def div(a, b):
    return a / b

t = (10, 2)
print(div(*t))>>>5.0
```

**命名元组**

命名元组可以用作存储关于对象的信息的存储器。

在下面的例子中，创建了一个命名的 tuple student 来存储学生信息。在第二行中，创建了一个名为 doe 的学生实例。

```
from collections import namedtuple

Student = namedtuple('Student', 'name marks')
doe = Student('Doe', (22, 34, 45, 64))
print(doe.marks)>>>(22, 34, 45, 64)
```

**用二等分和内排序管理序列**

**collections . bi section**有两个方法用于按顺序管理序列。

```
lis1 = [1, 4, 5, 6, 8, 12, 15, 20, 21, 23, 23, 26, 29, 30]
print(bisect.bisect(lis1, 6))
bisect.insort(lis1, 31)
print(lis1)>>>4
[1, 4, 5, 6, 8, 12, 15, 20, 21, 23, 23, 26, 29, 30, 31]
```

在上面的例子中，二分法将返回元素 6 在列表中的位置，即 4。

insort 方法会将元素添加到列表中的正确位置，即元素 31 将被添加到列表的末尾。

**字典**

> 字典不仅在我们的编程实现中很重要，在整个 python 编程结构中也很重要。字典在类、名称空间、模块构造等中无处不在。由于散列表的重要作用，它们被用在字典的后端，这使得它们非常有效。

类似于用于创建列表的列表理解。字典理解用于创建字典。

```
DIAL_CODES = [
    (86, 'China'),
    (91, 'India'),
    (1, 'United States'),
    (62, 'Indonesia'),
    (55, 'Brazil'),
    (92, 'Pakistan'),
    (880, 'Bangladesh'),
    (234, 'Nigeria'),
    (7, 'Russia'),
    (81, 'Japan'),
]
dict1 = {country: code for code, country in DIAL_CODES}
print(dict1)>>>{'China': 86, 'India': 91, 'United States': 1, 'Indonesia': 62, 'Brazil': 55, 'Pakistan': 92, 'Bangladesh': 880, 'Nigeria': 234, 'Russia': 7, 'Japan': 81}
```

**使用默认字典处理字典中缺失的键值**

使用字典时，很有可能会引用字典中没有的值，**默认字典**是一种用来防止这种问题的字典。

```
dict2 = collections.defaultdict(list)
print(dict2[3])>>>[]
```

默认字典采用一个称为默认工厂的参数，这个值可以是列表、字典、集合等。基于这个值，如果关键字不在字典中，将返回上面的值。所以在上面的例子中，将返回一个空列表。

**字典的变化**

**命令字典**

保持键的插入顺序。在调用 popitem 方法时，默认情况下将弹出第一个项目。这可以通过用 popitem()给参数 reverse=True 来改变。

**Collections.counter**

计数器用于保存每个键的整数计数。更新时，键计数器的值也将被更新。

```
counter = collections.Counter('aaaabctdedede')
print(counter)>>>Counter({'a': 4, 'd': 3, 'e': 3, 'b': 1, 'c': 1, 't': 1})counter.update('aaaaaa')
print(counter)>>>Counter({'a': 10, 'd': 3, 'e': 3, 'b': 1, 'c': 1, 't': 1})
```

从上面的例子中我们可以看到，通过更新计数器的值，a 的计数在字典中得到更新。

**映射代理**

在某些情况下，我们不想改变字典的映射，因为 python 提供了一个 mappingproxy 类型，它的映射是不能改变的。

```
from types import MappingProxyType

d = {1: 'A'}
d_proxy = MappingProxyType(d)
print(d_proxy)
```

在上面的例子中，d_proxy 就像一个不可变的字典，它的赋值是不能改变的。

```
d_proxy[1] = 'B'
```

这种重新分配是不可能的。

**集合理论**

现在让我们来看本文的最后一部分，即集合论，集合是定义明确的对象集合，基本用途是避免重复。

**基本设定操作**

```
a = ['apple', 'orange', 'apple']
b = ['apple', 'apple']
a = set(a)
b = set(b)
print(a & b)  # intersection of a & b
print(a | b)  # Union of a and b
print(a - b)  # Difference between a & b>>>{'apple'}
>>>{'apple', 'orange'}
>>>{'orange'}
```

> 重要的一点是，空集没有字面上的定义，定义的空集将被当作一个字典。

```
d1 = {'one'}
print(type(d1))
d2 = {}
print(type(d2))>>><class 'set'>
>>><class 'dict'>c = set()  # empty set
```

**集合理解**

类似于字典和列表，集合理解可以用来构造集合。

```
set1 = {chr(i) for i in range(1, 256)}
print(set1)
```

数字 1 到 256 的所有 Unicode 表示。