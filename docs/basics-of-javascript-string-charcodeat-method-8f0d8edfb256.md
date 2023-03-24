# Javascript 字符串 charCodeAt()的基础知识(方法)

> 原文：<https://medium.com/nerd-for-tech/basics-of-javascript-string-charcodeat-method-8f0d8edfb256?source=collection_archive---------19----------------------->

![](img/e5b3bbfb8c806766c55c7ec3855a20b3.png)

这篇文章是我在 youtube 上免费发表的关于网络开发基础的系列文章的抄本。如果你更喜欢看而不是读，请随时访问我的频道“Dev Newbs”。

你好，我的新手伙伴们。今天，我们将讨论下一个字符串对象方法，那就是 charCodeAt()。抓紧你们的帽子，因为我们要开始潜水了。

charCodeAt()方法返回一个数字，表示指定字符串中给定索引处字符的 Unicode 值。第一个字符的索引是 0，第二个字符的索引是 1，依此类推。最后一个字符的索引等于指定字符串的长度减一。

```
let myNotSoLongString= "Hello newbs!";myNotSoLongString.charCodeAt(0)         // 7'
myNotSoLongString.charCodeAt(1)         // 101
myNotSoLongString.charCodeAt(2)         // 108
myNotSoLongString.charCodeAt(3)         // 108
myNotSoLongString.charCodeAt(4)         // 111
myNotSoLongString.charCodeAt(5)         // 32
myNotSoLongString.charCodeAt(6)         // 110
myNotSoLongString.charCodeAt(7)         // 101
myNotSoLongString.charCodeAt(8)         // 119
myNotSoLongString.charCodeAt(9)         // 98
myNotSoLongString.charCodeAt(10)        // 115       myNotSoLongString.charCodeAt(11)        // 33
```

类似地，像 charAt()一样，当方法不返回实际的 Unicode 值时，也有一些例外。如果指定的索引处没有字符，或者索引小于“0”，就会出现这种情况。在这些情况下，我们用“NaN”来代替。

```
myNotSoLongString.charCodeAt(-1)       // NaN
myNotSoLongString.charCodeAt(12)       // NaN
```

两个结果都是“NaN”，因为我们在第一种情况下提供了负指数，在第二种情况下提供了超出范围指数。

让我们再看几个边缘案例，看看这个方法是如何工作的:

```
myNotSoLongString.charCodeAt("A")             // 72
myNotSoLongString.charCodeAt(NaN)             // 72
myNotSoLongString.charCodeAt(null)            // 72
myNotSoLongString.charCodeAt(undefined)       // 72
myNotSoLongString.charCodeAt(false)           // 72
myNotSoLongString.charCodeAt(true)            // 101
```

前四种情况是索引值无效的例子。因为它们不能被转换成整数，所以使用等于零的缺省索引值。

好了，我们已经完成了这个方法的基础。现在让我们深入一点。您可能还记得以前的文章，我多次提到了术语“代码单元”。如果你没有读过这篇文章或者只是忘记了，那么这里有一个简短的解释:在 javascript 中，代码单元是一个 16 位的数字。换句话说，它是一个仅由 1 和 0 组成的 16 位长数字。

现在，我还提到了一些字符不适合这些 16 位，因此它们由多个代码单元组成。正因为如此，我们有了 Unicode 码位，简称码位。

为了简单起见，我们假设一个代码点可以由一个或多个代码单元组成。并且它们表示特定的 unicode 字符。它们的值可以在 0 到 1，114，111 之间。

有趣的事实是，前 128 个 Unicode 码位与 ASCII 字符编码直接匹配。

现在回到我们今天的方法。charCodeAt()总是返回小于 65，536 的值。如果您忽略了这一点，那么它就是一个代码单元所能容纳的最大理论值。这意味着，对于由多个代码单元组成的字符，它不会返回完整的 unicode 值。

此时此刻，你可能会头疼，你可能会考虑放弃发展，去做一些压力较小的事情——比如修理高压电线。但是不要放弃。让我们用一个例子来看看这个问题。你会发现其实没那么糟。

```
let thumbsUpIcon = "👍";thumbsUpIcon.length                       // 2thumbsUpIcon.charCodeAt(0)                // 55357
thumbsUpIcon.charCodeAt(1)                // 56397thumbsUpIcon.codePointAt(0)               // 128077
```

好了，我们可以看到，由于 length 属性，我们的字符长度大于 1 个代码单位。所以我们已经知道我们将得到的值不是正确的，但是没关系。因为有另一种方法可以帮助我们。这就是 codePointAt()，它返回代码点的实际 unicode 值，而不仅仅是代码单元。

但是这是下一篇文章的主题。谢谢你坚持到最后。我希望你慢慢开始感觉到 Javascript 的专业技能在你的血管中流动。如果没有，不要担心——下一篇文章肯定会解决这个问题。

一如既往，非常感谢你的时间，我会尽快看到你的下一个方法。