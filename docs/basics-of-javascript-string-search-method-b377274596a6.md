# Javascript 字符串搜索的基础()(方法)

> 原文：<https://medium.com/nerd-for-tech/basics-of-javascript-string-search-method-b377274596a6?source=collection_archive---------12----------------------->

![](img/160fe1faae71dda15d3ea5d445b0117e.png)

这篇文章是我在 youtube 上免费发表的关于网络开发基础的系列文章的抄本。如果你更喜欢看而不是读，请随时访问我的频道“Dev Newbs”。

各位开发者朋友，你们好！在今天的这一集里，我们将会做一些探索。但是这一次，我们将只使用正则表达式。

search()方法在字符串中搜索指定的值，并返回匹配的位置。搜索值可以是字符串或正则表达式。如果提供 string 作为输入参数，它将自动转换为正则表达式。如果没有找到匹配项，该方法返回-1。

让我们在示例 1 中尝试一些简单的基本场景。

```
const str = "Blue sweater itches. I'll wear a red t-shirt and a blue jeans.";// string that will be converted to RegExp internally
let blue = "blue";
str.search(blue)                                       // 51// same as previous but explicitly RegExp
let regExp = /blue/;
str.search(regExp)                                     // 51// RegExp without case-sensitivity
let regExpIns = /blue/i;
str.search(regExpIns)                                  // 0// string that is not present in the searched string
let green = "green";
str.search(green)                                      // -1
```

第一种情况是一个简单的字符串作为输入参数。“蓝色”字符串第一次出现在位置 51。下一个案例实际上是同样的事情，但是我们没有让 Javascript 在内部完成到 RegExp 的转换，而是我们自己直接提供 RegExp。

第三种情况是查找时不区分大小写，这导致在位置 0 找到句子的第一个单词。最后一种情况涵盖了搜索字符串或正则表达式不存在的情况。我们得到-1。

这可能有点夸张，但是 String 中有大量的方法可以做几乎相同的事情。但是它们之间有细微的差别和不同的限制。让我们来看一个与搜索有些相似的方法的比较，并对它们进行比较。

```
// exec()
/blue/.exec(str)                   // Array ["blue", ..., index: 51]
/blue/i.exec(str)                  // Array ["Blue", ..., index: 51]// indexOf()
str.indexOf("blue")                // 51// match()
str.match("blue")                  // Array ["blue", ..., index: 51]
str.match(/blue/i)                 // Array ["Blue", ..., index: 51]
str.match(/blue/gi)                // Array ["Blue", "blue"]// matchAll()
...str.matchAll("blue")            // Array ["blue", ..., index: 51]
...str.matchAll(/blue/g)           // Array ["blue", ..., index: 51]
...str.matchAll(/blue/gi);         // 2 arrays (see below)// Array ["blue", ..., index: 51]
// Array ["Blue", ..., index: 0]// search()
str.search("blue")                 // 51
str.search(/blue/)                 // 51
str.search(/blue/i)                // 0
```

还有其他方法可以获得与 search()方法相同的信息。这是搜索字符串中第一个匹配项的位置。其中三个方法属于 String 对象。一个属于 RegExp 对象，但是我们之前已经提到过。我们一个一个来看。

exec()方法属于 RegExp 对象，但是我们已经用 match 和 matchAll()方法覆盖了它。它可以为我们提供匹配的索引，作为匹配数组中的一个额外属性。

可能最接近我们的 search()方法的方法是 indexOf()方法。它也只输出匹配的索引，但不幸的是它不能使用正则表达式。

match()和 matchAll()方法都可以获得与带有匹配项的数组的额外属性相同的信息。局限性在于，如果我们在 match()方法中使用全局标志，我们只能获得匹配项，而没有额外的信息。matchAll()的局限性在于它需要一个全局标志，因此会找到比我们可能需要的更多的内容。

通过正确的设置，我们可以用这 5 种方法中的任何一种得到相同的结果。但是它们中的每一个都最适合不同的场景，我们作为开发人员应该尊重这一点，并且总是尝试使用最好的工具来完成工作。但如果我们不这样做，也不会是世界末日。正如我常说的——让我们先把它做好，然后再把它做好。

所以，搜索方法就是这样。这又是一个相当简单直接的方法。至少当我们想到我们以前不得不处理的那些疯狂的事情的时候。一如既往——感谢您的关注，我们将很快看到下一个方法。