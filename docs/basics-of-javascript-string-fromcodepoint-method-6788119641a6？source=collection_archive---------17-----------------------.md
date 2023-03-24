# CodePoint()的 Javascript 字符串基础(方法)

> 原文：<https://medium.com/nerd-for-tech/basics-of-javascript-string-fromcodepoint-method-6788119641a6?source=collection_archive---------17----------------------->

![](img/f098a84b6603335a89e831ac351bc924.png)

这篇文章是我在 youtube 上免费发表的关于网络开发基础的系列文章的抄本。如果你更喜欢看而不是读，请随时访问我的频道“Dev Newbs”。

你好，我亲爱的新手。今天，我们将讨论 ECMAScript 2015 版本中引入的一种方法。但是因为浏览器厂商需要很长时间来实现多年前的标准，所以这个标准可以被认为是一个全新的标准。所以事不宜迟，让我们开始吧。

这个方法是一个静态方法，就像上一篇文章中它的前身一样。它的工作是返回使用指定的代码点序列创建的字符串。由于这是 fromCharCode()方法的更新和改进版本，它可以做几乎相同的事情，但也有一些额外的事情。

让我们看看它在示例 1 中是如何工作的。

```
// 'X' -> 88 or 0x58
// 'x' -> 120 or 0x78// single unicode value
String.fromCodePoint(88)                       // X
String.fromCodePoint(0x58)                     // X// multiple unicode values at once
String.fromCharCode(120, 88, 120)              // xXx
String.fromCharCode(0x78, 0x58, 0x78)          // xXx
```

如您所见，我可以使用 unicode 码位值一次创建单个字符或多个字符。任何数字格式都可以，但为了方便起见，大多数情况下您会希望使用十进制或十六进制格式。

这个方法中的一个消息是 RangeError，如果参数不是 0 或正整数，就会抛出这个消息。

以下值将引发 RangeError:

*   任何不是单个空格字符的字符串值
*   常数:无穷大、未定义、NaN
*   负整数
*   非整数(浮点数、科学符号等)

让我们在示例 2 中看看其中的一些。

```
try {
    String.fromCodePoint("X").charCodeAt() 
}
catch(e) {
    console.log("ERROR: " + e);           // Invalid code point NaN
}

String.fromCodePoint(true).charCodeAt()   // 1
String.fromCodePoint(false).charCodeAt()  // 0
String.fromCodePoint(null).charCodeAt()   // 0

try {
    String.fromCodePoint({}).charCodeAt()
}
catch(e) {
    console.log("ERROR: " + e);           // Invalid code point NaN
}

try {
    String.fromCodePoint([1,2]).charCodeAt()
}
catch(e) {
    console.log("ERROR: " + e);           // Invalid code point NaN
}
```

字符串、对象和数组也会导致抛出 RangeError。奇怪的是，null 常量被接受为零。布尔值 false 也会发生同样的情况。True 解释为一。

总之，fromCharCode()方法在这个部门中要宽容得多。但这是 Javascript 的发展方向，我们要么接受它，要么离开。

最后一个例子将展示我们可以像 fromCharCode()方法一样使用这个方法，但是同时，由于它也可以处理代码点，所以它可以做得更多。

```
// '臘' -> 63782 or 0xF926 or \uf926
// 
// '🙂' -> 55357 + 56898 or 0xD83D + 0xDE42 or \uD83D\uDE42
// '🙂' -> 128578 or 0x1F642

// '臘' -> 63782 or 0xF926 or \uf926
String.fromCodePoint(63782));                      // 臘
String.fromCodePoint(0xF926));                     // 臘
"\uf926"                                           // 臘

// '🙂' -> 55357 + 56898 or 0xD83D + 0xDE42 or \uD83D\uDE42
String.fromCodePoint(55357, 56898)                 // 🙂
String.fromCodePoint(0xD83D, 0xDE42)               // 🙂
"\uD83D\uDE42"                                     // 🙂

// '🙂' -> 128578 or 0x1F642 or \u1F642
String.fromCodePoint(128578)                       // 🙂
String.fromCodePoint(0x1F642)                      // 🙂

// TRY IT YOURSELF :)
// '🤦' -> 55358 + 56614 or 0xD83E + 0xDD26 or \uD83E\uDD26
// '🤦' -> 129318 or 0x1F926 or \u1F926
```

例 3 的第一部分显示，对于来自基本多语言平面的字符，没有任何变化。哦，如果你忘了，这些都是可以放入一个代码单元的字符，或者换句话说，unicode 值小于 65536 的字符。

就像第一部分一样，第二部分展示了如何使用代码单元值来做事情，只是这次使用了由两个代码单元组成的表情符号。

真正有趣的东西从第三部分开始。每个字符都有自己的数字 unicode 值，这个值可以用一个代码点来表示。这意味着，如果我们知道代码点的值，我们可以直接使用它，根本不用关心代理代码单元。

唯一的限制是，如果你想使用“\u ”,你必须坚持使用代码单元。Javascript 只在编写代码单元时接受这种符号，如果你在大于 65536 或十六进制 FFFF 的值上使用这种符号，它会忽略多余的数字。

好了，fromCodePoint()方法到此为止。

一如既往，非常感谢您的关注，下一篇文章再见。