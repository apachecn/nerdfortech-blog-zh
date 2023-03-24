# 有竞争力的程序员必须知道的 5 件 Python 事情

> 原文：<https://medium.com/nerd-for-tech/5-must-know-python-things-for-competitive-programmers-757899c10146?source=collection_archive---------4----------------------->

你们中有多少人在用 python 做竞技编程？它可以是任何东西，你想要那份梦想的工作，你想要在 [Hackerrank](https://www.hackerrank.com/dashboard) 爬上 python 排行榜。不管什么原因。有些事情你必须学会，才能让你的旅程平稳而有趣。我将分享 5 件让我的旅程变得轻松和没有痛苦的事情。

![](img/73a4d23c88310a4b817a50c9104bd2ca.png)

照片由 [Keenan Constance](https://unsplash.com/@keenangrams?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/five-finger?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

## 1.默认字典

字典是 python 中最常用的数据结构之一。可以在 O(1)时间内访问、存储元素。但是当你试图用一个不存在的键访问它时，它会抛出一个`KeyError`。如果你想在键不存在的时候使用默认值呢？

你可以这样做，

```
>>> itemsBought = {1:[‘A’,’B’],2:[‘A’,’C’]}
>>> itemsBought[3] #KeyError : 3
>>> itemsBought[3].append('C') # again KeyError: 3
```

我们如何以一种非 pythonic 方式避免 keyError，

```
>>> **itemsBought.get(3,[])** # if key doesn't exist return []
output: []>>> if **not itemsBought.get(3)**: 
       itemsBought[3] = ['C']  
    else:
       itemsBought.get(3).append('C')
```

我们如何删除所有的样板代码，并以更 pythonic 化的方式做同样的事情呢？答案是`defaultdict`

*   从`collections` 库中导入`defaultdict` 。

```
**from collections import defaultdict**
```

*   defaultdict 的语法是，`defaultdict(function)`
*   示例:

```
>>> from collections import defaultdict
>>> frequencyDict = defaultdict(**int**) #default value is 0
>>> frequencyDict[123]
**output: 0**>>> frequencyDict = defaultdict(**lambda : 10**) #default value is 10
>>> frequencyDict['z']
**output: 10** *####   setting list as default value   ####*>>> itemsBought = defaultdict(list)     
>>> itemsBought[123]  #123 is a non-existent key
**output: [] 
** 
>>> itemsBought = defaultdict(lambda : ['apple'])
>>> itemsBought[42] 
**output: ['apple']**
```

## 2.计数器

这个非常有用，你有没有在面试中被要求找出一个字符串中最常见的前 5 个字母，或者你有没有发现自己写了很多次样板代码来找出一个字母在一个字符串中的出现频率？Counter 类可以减少样板代码，这是计算对象数量的 pythonic 方式。

**1。从集合包中导入计数器。**

```
**from collections import Counter**
```

**2。你能用计数器做什么？**

```
>>> freqDict = Counter(“hello world”)
>>> freqDict
Counter({‘l’: 3, ‘o’: 2, ‘h’: 1, ‘e’: 1, ‘ ‘: 1, ‘w’: 1, ‘r’: 1, ‘d’: 1})## not only strings , you can pass any iterables like list,tuple etc
>>> freqDict = Counter([1,1,2,3,3,4])
>>> freqDict
**Counter({1: 2, 3: 2, 2: 1, 4: 1})**
```

如何在计数器中找到 N 个最常见的元素

```
>>> freqDict = Counter(‘counters are awesome’)
>>> **freqDict**.**most_common(5)** #find 5 most common letters
[(‘e’, 4), (‘o’, 2), (‘r’, 2), (‘s’, 2), (‘ ‘, 2)]
```

让我们通过使用`counter`统计你买了多少东西和卖了多少东西来做一些库存管理

```
>>> itemsBought = {‘A’:10,’B’:10,’C’:10} #bought items
>>> itemsSold = {‘A’:5,’B’:10,’C’:7}     #sold items
>>> inventory = Counter()                #create a counter  
>>> inventory.update(itemsBought)      #update it with bought items 
>>> **inventory.subtract(itemsSold)**   #subtract it from the sold items
>>> inventory
Counter({‘A’: 5, ‘C’: 3, ‘B’: 0})   #items remaining!!
```

**3。设定递归限制**

你还记得吗，当你写第一个递归阶乘函数的时候忘记设置基本情况会发生什么？

```
**RecursionError: maximum recursion depth exceeded**
```

但是你不应该讨厌 RecursionError，你不会想把你的电脑挂一天就为了找到 5 的阶乘。

Python 默认的递归限制是 1000。对于大多数情况来说，这是相当好的，但不是所有时候。对于某些问题，您希望增加堆栈深度，以允许更多的递归调用。

**1。导入 sys 包**

```
import sys
```

**2。sys . setrecursionlimit(limit)**

```
sys.setrecursionlimit(10000) 
```

> *可能的最高限制取决于平台。*最高可能限制取决于平台。

## 4.python 中的比较器。

您是否曾经困惑于如何根据值对计数器字典进行排序，或者如何根据年龄和姓名对 Person 类进行排序？让我们看看如何做到这一点，

1.  **键—变换&排序**

有时，您希望在将数据转换为其他值后对其进行排序。

示例:

1.对字典键进行排序，但是基于它们的值。

2.根据循环列表右侧的第三个元素对其进行排序。🤣🤣(是的，有时候你可能会被问到这样的问题。所以，要做好准备)

怎么做？使用**键**！！

你听说过 sort()之类的函数中的`key` 参数吗，它做的正是我上面提到的，**它将元素转换成一些其他的值，然后排序**！！

```
>>> fruitsBought={‘orange’:17,
         ’jackfruit’:5,’apple’:200} #  I Like apples>>> keys = list(fruitsBought.keys())   #convert the keys to list
>>> keys.sort( **key= lambda key: fruitsBought[key]** ) 

>>> keys
['jackfruit', 'orange', 'apple']
```

`key` 所做的是使用传递的函数转换元素。

```
>>> keys.sort(key=lambda key: fruitsBought[key])
```

*   它首先将 `[‘orange’,’jackfruit’,’apple’]`转换为`**[17,5,200]**`
*   然后将转换后的值从`**[17,5,200]**` T17 到 T5 排序
*   将其重新转换为原始值， `[‘jackfruit’,’orange’,’apple’]`

## cmp_to_key

如果您来自 Java 背景，它的作用就像一个比较器。该函数接受 2 个参数`a and b`，并输出

> 如果 a=b，则为 0
> 
> **负数**如果 a < b
> 
> **正数**如果 a > b

示例:

*   当你想根据字母出现的频率对字符串中的字母进行排序，但是当两个字母出现的频率相同时，就按字母顺序进行排序。

```
>>> from collections import Counter
>>> **from functools import cmp_to_key**
>>> def compare(letter1,letter2):
         freqA = counterDict[letter1]
         freqB = counterDict[letter2]

         #just a utility function 
         def comp(a,b):
           if a==b: **return 0    # a==b**
           elif a<b: **return -1  # a<b**
           else: **return 1       # a>b**

         isFreqNotEqual = comp(freqA,freqB) # compare the letters if the frequencies are same
         if **not isFreqNotEqual**: 
                return comp(letter1,letter2) return isFreqNotEqual>>> counterDict = Counter(‘aabbcccdd’)
>>> keys = list(counterDict.keys())
>>> keys.sort(key=**cmp_to_key(compare)**)
>>> keys
['a', 'b', 'd', 'c']
```

## 5.PYPY3

众所周知，python 速度很慢🐢与 C++、Java 等其他编程语言相比。但是看在上帝的份上，我们已经爱上它了。有时候，我们已经编写了一个非常优化的 python 解决方案，但是仍然会出现令人讨厌的`TLE`错误。为了克服这一点，有一个黑客。我们可以使用 python 的优化版本 **PyPy3** (一个带有 JIT 编译器的快速 Python 实现)。

令人惊讶的是，许多编码平台如 Hackerrank、CodeChef 等(不包括 leetCode☹)都支持 PyPy3。

![](img/9cd6fbb0a903b64c0ff50d5d19c5312c.png)

Hackerank 支持 PyPy2、PyPy3

当您下次遇到 TLE 错误时，只需切换到 PyPy3 并复制粘贴您的 python 代码，就可以看到 TLE 消失了。

希望你喜欢这篇文章，如果你知道任何其他必须知道的事情，请分享🤗它在评论中。拍手声👏如果你喜欢这篇文章。