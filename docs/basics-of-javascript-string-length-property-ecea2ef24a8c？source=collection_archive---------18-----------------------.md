# Javascript 字符串长度的基础知识(属性)

> 原文：<https://medium.com/nerd-for-tech/basics-of-javascript-string-length-property-ecea2ef24a8c?source=collection_archive---------18----------------------->

![](img/5b016a8e779cfd238a45668956059450.png)

这篇文章是我在 youtube 上免费发表的关于网络开发基础的系列文章的抄本。如果你更喜欢看而不是读，请随时访问我的频道“Dev Newbs”。

大家好，欢迎来到 Javascript 基础知识系列中关于字符串的第二篇文章。今天，我们讨论的是字符串对象为数不多的可用属性之一，它就是“长度”。

length 属性返回字符串的长度(字符数)。

```
let sentence = "Hello newbs!";// returns "Number of characters in the sentence is: 12"
```

好吧，够直接了。你会问，空弦怎么办？让我想想。空字符串的长度将是 0。

```
let sentence = "";// returns "Number of characters in the sentence is: 0"
```

这真的很简单。我想我们就到此为止吧…我给你一个大拇指表示对这条路的赞赏怎么样？

```
let thumbsUp = "👍";         // outputs 2
```

好吧，这很奇怪…让我们试试其他的角色:

```
let unknownCharacter= "你";  // outputs 2
```

这是怎么回事？为什么说一个字符有 2 个字符的长度？问题是 length 实际上并不返回字符串中的字符数，而是返回字符串中的代码单元数。

Javascript 使用 UTF-16 作为其字符串格式，这意味着每个字符都被编码为 16 位长的二进制数。或者一个代码单元。

但是 UTF-16 字符集只能容纳这么多字符。准确地说，理论上它最多能容纳 2 的 16 次方，也就是 65536 个字符。这听起来可能很多，但是你需要意识到我们不仅仅使用拉丁字母和数字。不同的语言有不同的字母和标点符号，甚至在它们的字母表中有一些额外的字母。还有很多很多其他的字母，例如希腊字母、阿拉伯字母、中文字母、阿兹布卡字母等等。突然之间 6 万 5 听起来不多了吧？

虽然你在现实生活中遇到这个问题的可能性很小，但能意识到这一点还是不错的。为了让你不会因此而失眠，我将向你展示两种不同的解决方案来解决这个问题，如果你将来遇到这种问题的话。

第一种选择是使用“三点”符号将字符串转换成数组。让我快速展示给你看:

```
let thumbsUpIcon = "👍";thumbsUpIcon.length                         // 2let stringToArray = [...thumbsUpIcon];stringToArray.length                        // 1
```

这样做的原因是应用于转换后的数组的迭代器遍历字符，而不是代码单元。因此，它计算的是我们真正想要计算的。

另一个选择是使用一个叫做 **match()** 的字符串对象方法。我就不赘述了，因为这个方法会得到自己的视频。请记住，这在这种情况下也会有所帮助。

```
thumbsUpIcon.match(/./gu).length;          // 1 as well
```

好，这就是字符串属性长度。我希望你今天学到了新东西。我确实在研究这个话题中学到了很多。

感谢您的时间，我们将在下一篇文章中再见。