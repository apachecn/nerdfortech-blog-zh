# Javascript String normalize()的基础知识(方法)

> 原文：<https://medium.com/nerd-for-tech/basics-of-javascript-string-normalize-method-f3241174c2d0?source=collection_archive---------8----------------------->

![](img/67b538508f0f0d2007803367e6cb7d63.png)

这篇文章是我在 youtube 上免费发表的关于网络开发基础的系列文章的抄本。如果你更喜欢看而不是读，请随时访问我的频道“Dev Newbs”。

嗨，我所有的开发伙伴们！我们将再次混淆音调符号。今天的每日专题是 method normalize()，它用不同的方式来表示同一个字符。我们开始吧！

normalize()方法返回字符串的 Unicode 规范化形式。

我们需要提供的唯一参数是一个表单。表单可以有以下 4 个值之一:

*   **NFC** (规范分解，之后是规范合成)
*   **NFD** (规范分解)
*   **NFKC** (兼容性分解，接着是规范组合)
*   **NFKD** (兼容性分解)

如果省略该值或将其指定为未定义，则使用默认值“NFC”。

有很多缩写。让我们检查示例 1 中的前两个。

```
let comped =   '\u0045\u006c\u0020\u004e\u0069\u00f1\u006f';
let decomped = '\u0045\u006c\u0020\u004e\u0069\u006e\u0303\u006f';comped + " (" + comped.length + ")"              // El Niño (7)
decomped + " (" + decomped.length + ")"          // El Niño (8)
comped == decomped                               // falselet compedNFC = comped.normalize('NFC');
let decompedNFC = decomped.normalize('NFC');compedNFC + " (" + compedNFC.length + ")"        // El Niño (7)
decompedNFC + " (" + decompedNFC.length + ")"    // El Niño (7)
compedNFC == decompedNFC);                       // truelet compedNFD = comped.normalize('NFD');
let decompedNFD = decomped.normalize('NFD');compedNFD + " (" + compedNFD.length + ")"        // El Niño (8)
decompedNFD + " (" + decompedNFD.length + ")"    // El Niño (8)
compedNFD == decompedNFD                         // true
```

规范化的要点是，我们可以用它的专用码位值写同一个字符，但是我们也可以组合多个其他基本字符来得到特定的字符。正是西班牙语短语“厄尔尼诺”中“尼”字母的情况。我们可以使用专用的 unicode 值“\u00f1”来编写它，或者对基本字母“n”加上音调符号标记使用两个单独的 unicode 值—在本例中，字母“n”使用“\u006e”，字母“n”上方的音调符号标记颚化符使用“\u0303”。

作为结果的消费者，你和我不能直观地看出区别，但此时，我们开始使用结果字符串进行搜索或排序，我们可能会得到我们没有预料到的东西。因此，我们有了 normalize()方法，作为开发人员，它允许我们选择希望得到的字符串是什么样子，这样我们就可以在代码中正确地处理它。

第一部分显示两个字符串不相等，它们甚至有不同的长度。

第二部分将规范组合应用于两个变量。我们得到两个新的变量，它们都是相等的，并对特殊字符使用较短的组合形式。

第三部分应用相反的原理——对两个原始变量进行规范分解，我们得到另外两个新变量。这一次，它们都使用特殊字符的更长分解版本。

我使用了术语“规范的”,但没有更详细地解释它的确切含义。让我们现在就解决这个问题。在 unicode 中，如果两个代码点序列表示相同的抽象字符，并且它们应该总是具有相同的视觉外观和行为，例如以相同的方式排序，则它们具有“规范等价”。

有了“NFC”和“NFD”形式参数，我们可以保证两个规范上等价的字符串也将由彼此相等的相同字符串来表示。

然后，就有了“兼容性正常化”。在 Unicode 中，如果两个码位序列表示相同的抽象字符，则它们是兼容的，并且在某些(但不一定是所有)应用程序中应该被同等对待。

```
let str1 = '\uFB00';
let str2 = '\u0066\u0066';str1 + " (" + str1.length + ")")                // ﬀ (1)
str2 + " (" + str2.length + ")"                 // ff (2)
str1 === str2                                   // falselet str1_NFKC = str1.normalize('NFKC');         
let str2_NFKC = str2.normalize('NFKC'); str1_NFKC + " (" + str1_NFKC.length + ")"       // ff (2)       
str2_NFKC + " (" + str2_NFKC.length + ")"       // ff (2)
str1_NFKC === str2_NFKC                         // truelet str1_NFKD = str1.normalize('NFKD');
let str2_NFKD = str2.normalize('NFKD');str1_NFKD + " (" + str1_NFKD.length + ")"       // ff (2)     
str2_NFKD + " (" + str2_NFKD.length + ")"       // ff (2)
str1_NFKD === str2_NFKD                         // true
```

我们可以说，如果两个序列在规范上是等价的，那么它们也是相容的。但是我们不能总是说反之亦然。

在某些方面(比如排序),这两个序列应该被视为等价的——而在某些方面(比如视觉外观),它们不应该被视为等价的，因此它们在规范上是不等价的。

我们可以使用带有“NFKD”或“NFKC”参数的 normalize()来产生一种对所有兼容字符串都相同的字符串形式。

就像在这个例子中，我们看到“ﬀ”并不等同于两个字母“f”的组合。但是如果我们想对这个序列进行排序，我们会希望将这个符号视为两个字母“f”，但是我们仍然希望保持它们的视觉差异。因此，这两个序列是兼容的，但在规范上并不等同。

最后一个例子将展示我们将每种形式应用于相同的字符串序列所得到的不同结果。

```
// ORIGINAL SEQUENCE
// U+1E9B: LATIN SMALL LETTER LONG S WITH DOT ABOVE
// U+0323: COMBINING DOT BELOW
let str = '\u1E9B\u0323';// NFC
// U+1E9B: LATIN SMALL LETTER LONG S WITH DOT ABOVE
// U+0323: COMBINING DOT BELOW
let nfc = str.normalize('NFC');
nfc + " (" + nfc.length + ")"                  // ẛ̣ (2)
[...nfc]                                       // ["ẛ", "̣"]// NFD
// U+017F: LATIN SMALL LETTER LONG S
// U+0323: COMBINING DOT BELOW
// U+0307: COMBINING DOT ABOVE
let nfd = str.normalize('NFD');
nfd + " (" + nfd.length + ")"                  // ẛ̣ (3)
[...nfd]                                       // ["ſ", "̣", "̇"]// NFKC
// U+1E69: LATIN SMALL LETTER S WITH DOT BELOW AND DOT ABOVE
let nfkc = str.normalize('NFKC');
nfkc + " (" + nfkc.length + ")"                // ṩ (1)
[...nfkc]                                      // ["ṩ"]// NFKD
// U+0073: LATIN SMALL LETTER S
// U+0323: COMBINING DOT BELOW
// U+0307: COMBINING DOT ABOVE
let nfkd = str.normalize('NFKD');
nfkd + " (" + nfkd.length + ")"                // ṩ (3)
[...nfkd]                                      // ["s", "̣", "̇"]
```

每种形式产生的结果稍有不同。前两种形式遵循规范等价的规则，所以符号保持了它的视觉外观和行为。在保证兼容性的最后两种情况下，视觉发生了变化，但是这两种结果至少是相互兼容的，如果不是与前两种形式兼容的话。

好吧，这又是一个噩梦般的方法。这并不有趣，但是当你在网络的荒野中遇到它的时候，你会感谢我的。

一如既往——非常感谢您抽出时间，下次再见。