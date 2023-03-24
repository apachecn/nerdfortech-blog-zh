# Javascript 字符串 codePointAt()的基础知识(方法)

> 原文：<https://medium.com/nerd-for-tech/basics-of-javascript-string-codepointat-method-d46ce2dd8562?source=collection_archive---------25----------------------->

![](img/4ccd5e0978eb823ddd10e21fb6e8aa96.png)

这篇文章是我在 youtube 上免费发表的关于网络开发基础的系列文章的抄本。如果你更喜欢看而不是读，请随时访问我的频道“Dev Newbs”。

再次向各位开发者问好。你会问，今天为我们准备了什么？嗯，我们今天请客。今天我们将讨论一种源自 Javascripts 老版的方法。这个方法就是 codePointAt()。

好吧，那关于大姐的评论是怎么回事？我指的是什么语言？正确答案是我说的大姐指的是 Java。当我在准备这一集的字符串方法时，我显然需要做一些研究。当我在我最喜欢的网站上寻找关于这种方法的信息时，有趣的事情发生了。其中两个给我指出了 Java 方法 codePointAt()，而不是 Javascript 版本。更有趣的是，该方法的描述实际上与 Javascript 版本的定义一模一样。这让我得出结论，Javascript 从 Java“借用”了这个方法的规范。好了，今天教程的琐事部分就到这里，现在让我们回到解释上来。

codePointAt()方法返回一个非负整数，即 UTF-16 码位值。强调代码点。

```
let mySoSoLongString= "Hi devs! 🙂";mySoSoLongString.codePointAt(0)         // 72
mySoSoLongString.codePointAt(1)         // 105
mySoSoLongString.codePointAt(2)         // 32
mySoSoLongString.codePointAt(3)         // 100
mySoSoLongString.codePointAt(4)         // 101
mySoSoLongString.codePointAt(5)         // 118
mySoSoLongString.codePointAt(6)         // 115
mySoSoLongString.codePointAt(7)         // 33
mySoSoLongString.codePointAt(8)         // 32
mySoSoLongString.codePointAt(9)         // 128578
```

我们当然有一些边缘案例要讨论。如果在给定的索引处没有字符，方法返回**未定义的**。为了获得这种行为，我们可以使用负整数作为索引，或者使用等于或大于给定字符串长度的整数值。

```
mySoSoLongString.length                 // 11
'🙂'.length                             // 2mySoSoLongString.codePointAt(-1));      // undefined
mySoSoLongString.codePointAt(11));      // undefined
```

如果作为索引值提供的值不能转换为整数值，该方法将默认使用等于零的索引。

```
mySoSoLongString.codePointAt("X")        // 72
mySoSoLongString.codePointAt(NaN)        // 72
mySoSoLongString.codePointAt(null)       // 72
mySoSoLongString.codePointAt(undefined)  // 72
mySoSoLongString.codePointAt(false)      // 72
mySoSoLongString.codePointAt(true)       // 105
```

前四个示例的索引值不能转换为整数，因此使用等于零的索引默认值。

最后两个案例有点不同。布尔值 true 和 false 都可以转换为整数— false 是零，true 是一。在 false 的情况下，这并不重要，因为结果就像 false 不可转换一样—结果是使用零作为索引值。

在 true 的情况下更有趣一些，因为我们使用的索引值是 1。所以我们得到了位置 1 的代码点。

到目前为止，一切顺利。这种方法似乎没有问题。最后。不附带任何条件的字符串方法。嗯，很抱歉打破你的幻想，但这个故事从现在开始走下坡路了。

前面例子中的字符串末尾有一个表情符号。我们获得了这个笑脸的实际 unicode 值，因为我将该方法指向了正确的位置——创建表情符号的第一个代码单元的索引。表情符号由两个代码单元组成，从索引 9 开始，到索引 10 结束。

但是，如果我给 index 10 作为我想从中获取代码点的字符的索引，会发生什么呢？好吧，我们一起看例 4。

```
mySoSoLongString.length                 // 11
'🙂'.length                             // 2mySoSoLongString.codePointAt(9)         // 128578
mySoSoLongString.codePointAt(10)        // 56898   
mySoSoLongString.charCodeAt(9)          // 55357
mySoSoLongString.charCodeAt(10)         // 56898
```

我输出字符串的长度只是为了提醒一下。接下来的输出显示表情符号确实有两个代码单位长。

然后我们得到了索引号 9 的代码点值。我们得到我们期望的。到目前为止一切顺利。

但是我们从位置 10 得到代码点值，问题就在这里。我们得到的实际上只是指定位置的代码单元的一个值。

使用上一篇文章中的 charCodeAt()方法，检查接下来的两行输出，其中我输出了代码单元的代码值，而不是代码点。在位置 9 的情况下，我们得到不同的结果，但是在索引 10 的情况下，我们得到完全相同的结果。

问题是 codePointAt()方法就像任何其他字符串方法一样，不知道字符有多大。

大多数表情符号和特殊符号是一个或两个代码单元，在大多数情况下，这些代码单元又转化为一个代码点。对于这些情况，我们已经有了识别这些字符的方法。我们可以使用 spread 语法，将它们分割成一组单独的符号。然后我们可以毫无困难地处理每个字符。我将在下一个例子中展示给你看。

```
let ordinaryEmojis = "🐊🤦👍🙂";ordinaryEmojis.length                     // 8

[...ordinaryEmojis]                       // 🐊,🤦,👍,🙂[...ordinaryEmojis].length                // 4[...ordinaryEmojis][0].length             // 2
[...ordinaryEmojis][1].length             // 2
[...ordinaryEmojis][2].length             // 2
[...ordinaryEmojis][3].length             // 2
```

但有时，我们甚至有更多特殊的角色，表情符号…无论你想怎么称呼它们。这些是其他表情符号的组合，甚至是一些特殊的“胶水”字符，告诉我们需要以某种方式修改默认字符。

让我们看看今天最后一个例子。

```
let specialEmoji = "🤦🏼‍♂️";specialEmoji.length                       // 7[...specialEmoji].length                  // 5[...specialEmoji]                         // 🤦,🏼,‍,♂,️[...specialEmoji][0].length               // 2
[...specialEmoji][1].length               // 2
[...specialEmoji][2].length               // 1
[...specialEmoji][3].length               // 1
[...specialEmoji][4].length               // 1
```

在 99%的情况下，你会遇到那些普通的表情符号。然而，你可能会很幸运地成为一百个开发者中的一个，遇到“特殊的莫基”。

默认表情符号的名称是 facepalm，我们可以在前面例子的 ordinaryEmojis 变量中看到它的中性版本。

但是我们的特殊的有额外的信息，进一步修改它。最后，它由以下五个代码点组成:

*   默认中性 facepalm 表情符号(2 个代码单位)
*   表情符号肤色修饰词[高加索人] (2 个代码单位)
*   零宽度连接器(1 个代码单位)
*   性别标志[男性] (1 个代码单位)
*   变化选择器[多色] (1 个代码单位)

幸运的是，这些来自地狱的表情符号仍然非常罕见，但它们的受欢迎程度与日俱增。不幸的是，目前还没有简单的方法来处理包含这种符号的字符串。也许当它们成为表情符号事实上的标准时，这种情况将会改变。在那之前，你能做的只有祈祷。

一如既往地感谢您的宝贵时间，我们将在下一篇文章中再见。