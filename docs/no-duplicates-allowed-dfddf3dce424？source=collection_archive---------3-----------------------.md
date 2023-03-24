# 不允许重复！

> 原文：<https://medium.com/nerd-for-tech/no-duplicates-allowed-dfddf3dce424?source=collection_archive---------3----------------------->

几周前我进行模拟 JavaScript 面试时，我的一个编码挑战是从数组中移除重复元素。令我懊恼的是，我真的不知道该怎么做。幸运的是，这并没有发生在实际的工作面试中，面试官告诉了我一个完成这个任务的方法。当我得知从数组中删除重复项是一个常见的 JavaScript 面试问题时，我并不感到惊讶，这意味着我们这些 JavaScript 新手应该知道至少有几种方法可以做到这一点。

让我们来看看三种方法来摆脱这些该死的重复。(但是，请记住，与 JavaScript 中的许多其他编码任务一样，有许多方法可以实现。我刚刚发现我喜欢这三个，如果在面试中再次提出这个任务，我会使用其中的一个或多个。)

预备，集合，开始！

为 ES6 欢呼三声！这个最新版本的 JavaScript 给了我们一个超级简单的方法来删除数组中的重复项。那就是使用[设置对象](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set)，如下所示:

```
const myArray = [9, 9, 1, 5, 8, 9, 13, 1, 7, 5]function noDupsAllowed(arr) { return […new Set(arr)]}console.log(noDupsAllowed(myArray))
```

或者更简单:

```
const myArray = […new Set([9, 9, 1, 5, 8, 9, 13, 1, 7, 5])]console.log(myArray)
```

每个示例中记录到控制台的结果是*【9，1，5，8，13，7】*。

在我的正式 JavaScript 研究中，从来没有涉及过 Set 对象。为什么？我不知道，但我希望是这样。

这如何让我们得到一个没有重复的数组呢？Set 对象保存值，但是每个值必须是唯一的。如上面的例子所示，传入一个 iterable 对象(在本例中是一个数组)将产生一个新的集合，该集合包含与该 iterable 对象相同的值，只是新集合中的每个值只有一个实例。任何重复的值都会被丢弃。这种方法的一个好处是新集合中的项目遵循它们的插入顺序。

使用 Set 对象从数组中删除重复项是如此简单和高效，以至于 coder 和 YouTuber [Robin Pokorny](https://www.youtube.com/watch?v=bRpVR1x_O8Q) 称之为执行这项任务的“唯一正确的方法”。

**失意？用滤镜！**

获得无重复数组的一个不太简单但仍然容易的方法是使用 [filter()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter) 方法。

```
const myArray = [9, 9, 1, 5, 8, 9, 13, 1, 7, 5]function noDupsAllowed(item) { return item.filter((v, index) => item.indexOf(v) === index)}console.log(noDupsAllowed(myArray))
```

顾名思义，filter()方法过滤掉任何不符合我们设置的条件的元素。在我们的例子中，filter()有两个参数:被评估的当前元素的值(由 *v* 表示)和该元素的*索引*。注意，它还使用 indexOf()，返回该值的第一个实例的索引。如果一个值在被过滤的数组中出现不止一次，那么我们的函数会想，“嘿，我已经在先前的索引中遇到这个值了。抱歉，伙计，但我不能让你加入新的队伍。”

上面的例子也可以这样写:

```
let newArray = myArray.filter((v, index) => myArray.indexOf(v) === index)console.log(newArray)
```

使用 filter()方法而不是 Set 对象的一个优点是，它还可以用于返回重复项。

```
const myArray = [9, 9, 1, 5, 8, 9, 13, 1, 7, 5]let newArray = myArray.filter((v, index) => myArray.indexOf(v) !== index)console.log(newArray)
```

结果:*【9，9，1，5】*。我们唯一要做的改变就是使用*！==* 而不是 *===* 。

**For tooth，a For Loop**

最后一种方法比前两种方法稍微复杂一点。它包括创建一个空数组，并使用一个 *for* 循环迭代原始数组，并将唯一值推入新数组。

```
const myArray = [9, 9, 1, 5, 8, 9, 13, 1, 7, 5]function noDupsAllowed(arr) { const newArray = [] for(let v of arr) { if(newArray.indexOf(v) === -1) { newArray.push(v) }
   }
   return newArray
}console.log(noDupsAllowed(myArray))
```

与前面使用 filter()的示例类似，这个示例使用 indexOf()，只是它检查新数组以查看其中是否已经有值。如果不是(因此是 *-1* ，这是 indexOf()在没有找到该值时返回的内容)，那么该值将被推入新数组。

**为什么不直接设定？**

如果我在以后的编码面试中被要求从数组中删除重复项，我更愿意使用 Set。然而，我听说面试官可能认为这太容易了，希望你展示更多的编码技能。在这种情况下，我更喜欢使用 filter()方法。除此之外，我会求助于循环的*。我遇到过其他方法，但发现它们太复杂，不适合我。*

你有什么想法？如果你在一次编码面试中被要求这样做，你用的是什么方法？