# 发现 JavaScript 的 Set 对象

> 原文：<https://medium.com/nerd-for-tech/discovering-javascripts-set-object-ed34cdbc677?source=collection_archive---------34----------------------->

![](img/c6efc045d0adbc6cffcb287d716ccc91.png)

雅各布·欧文斯通过 unsplash.com[拍摄](https://unsplash.com/photos/CiUR8zISX60)

我最近在 Hacker Rank 上练习一些算法问题，以帮助自己准备技术面试。我遇到了一个问题，需要我将一个数组过滤成唯一的值。这是我以前做过的事情，我想出的解决方案有效，但不是很漂亮。事情是这样的:

```
1\. let a = [1, 1, 2, 3, 3, 4, 5, 5]2\. console.log(a.reduce((acc, val) => acc.includes(val) ? acc : [...acc, val], []))
// *[1, 2, 3, 4, 5]*
```

Reduce 就像 JavaScript 函数的瑞士军刀，几乎可以用来做任何事情。但是如果你不熟悉它，它可能会令人困惑。我决定做一些谷歌搜索，并找到另一种更简单的方法来解决这个问题。JavaScript Set 对象步入其中。

根据 [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set) 的说法，Set 对象“让你存储任何类型的唯一值，无论是原始值还是对象引用”。存储在集合中的值是根据插入时间排序的值的集合(新插入的值位于集合的末尾)。当创建或添加到集合中时，使用严格相等运算符`===`将被添加的值与集合中的每个值进行比较，只有每次比较返回 false 的值才会被添加。

有太多要理解的了。让我们来看一些例子和不同的用例，以帮助强化 Set 对象的工作方式。首先，让我们看看如何使用集合来解决我们最初的问题，即把一个数组过滤成一个唯一值的数组。

```
1\. let x = [1, 1, 2, 3, 3, 4, 5, 5]2\. console.log([...new Set(x)]) 
// *[1, 2, 3, 4, 5]*
```

在这里，我们创建一个新的集合，并在使用 spread 操作符创建一个新的数组之前将一个数组`a`传递给它。如果你问我，这是更容易阅读的，也是更容易打字的。假设你明白什么是集合，这样写也更有描述性。

集合也可以用来计算两个数组之间的并集、交集和差集。让我们看看使用下面的数组会是什么样子。

```
1\. let a = [1, 1, 2, 3, 3, 4, 4]2\. let b = [3, 3, 4, 5, 6, 6]
```

# **工会**

两个数组的并集是一个新数组，它包含由两个数组组合而成的元素的唯一列表。

```
3\. let union = [...new Set([...a, ...b])]4\. console.log(union) 
// *[1, 2, 3, 4, 5, 6]*
```

# 十字路口

两个数组的交集是一个新数组，它包含两个数组中都存在的元素的唯一列表。

```
5\. let intersection = [...new Set(a.filter(i => b.includes(i)))]6\. console.log(intersection) 
// *[3, 4]*
```

# 差异

数组 a 和数组 b 之间的差异是唯一值的新数组，这些值出现在 a 中，但不在 b 中。数组 b 和数组 a 之间的差异正好相反。

```
7\. let difference = [...new Set(a.filter(i => !b.includes(i)))]8\. console.log(difference) 
// *[1, 2]*or 9\. difference = [...new Set(b.filter(i => !a.includes(i)))]10\. console.log(difference) 
// *[5, 6]*
```

如你所见，集合对于比较两个数组非常有用。有几个缺点值得注意。首先是 Internet Explorer 不支持 Set prototype 的许多方法。幸运的是 IE 很快就要走到它的生命尽头了，但这并不意味着每个人都会停止使用它。

经由[giphy.com](https://giphy.com/gifs/colbertlateshow-stephen-colbert-surprise-late-show-l0HlO3BJ8LALPW4sE)

另一个需要注意的重要事情是，由于 Set 使用严格的相等运算符`===`来比较值，所以它是区分大小写的。这导致了一些有趣的行为。让我们快速看一些例子。

```
1\. console.log(new Set('baseball')
// *Set { 'b', 'a', 's', 'e', 'l' }*2\. console.log(new Set('Baseball')
// *Set { 'B', 'a', 's', 'e', 'b', 'l' }*
```

在第一行和第二行，Set 遍历字符串中的每个字符，并创建一组新的唯一值。这导致所有重复的字母被省略，但是大写字母`B`出现在第二行的集合中，因为`'b' !== 'B'`。

```
1\. let mySet = new Set()2\. mySet.add('Baseball')3\. mySet.add('baseball')4\. console.log(mySet)
// *Set { 'Baseball', 'baseball' }*
```

这个例子稍微明显一些，但仍然值得指出。`'Baseball’`和`‘baseball’`都可以加到`mySet`上，因为它们彼此并不严格相等。

Set 对象是一个工具，我很高兴将它添加到我的工具箱中。当**比较两个不同的数组**时，这看起来很棒，并且可以以多种方式使用。希望你已经学到了一些东西，并且能够更好地处理不同的算法问题。我知道我是！

如果您对 Set 对象有任何其他用例，请在下面分享！