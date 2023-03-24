# 作为 iOS 开发者在 Swift 中遇到字符串

> 原文：<https://medium.com/nerd-for-tech/meet-strings-in-swift-as-ios-developer-48bc9272937d?source=collection_archive---------11----------------------->

![](img/7c506864a4b25fcfd23255c3c1571ce6.png)

米卡·鲍梅斯特在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

swift 中的字符串是 iOS 开发者非常常用的。我们用弦乐的力量解决了发展中的各种问题。但是我们通常忽略了字符串中的一些重要函数。在这篇文章中，我试图收集这样的函数，并尽可能添加一些扩展。

# 从字符串中分离子字符串

在开发中，我们会遇到读取 csv 文件和分隔用户输入的标签。在这些场景中，我们需要从由分隔符分隔的字符串创建一个字符串数组。这里可以使用 swift 中的组件功能。

```
let res = str.components(separatedBy: ",")
```

输出是由原始字符串中的'，'分隔的字符串数组。

# 基于索引的用例

字符串中的字符可以通过字符串的索引来访问。与其他编程语言不同，我们不会在 swift 中使用基于整数的访问。

一个突出的原因是 swift 中字符的长度不一致。组成字符的 unicode 点的组合各不相同。所以每次索引都是相对于字符串计算的。

## 索引…之后

有时，我们需要获取字符串的某个部分/索引之后的字符的索引，即从第二个字符开始的“elephant”中“e”的索引(“lephant”)。下面是可以帮到你的扩展。

```
**extension** Array **where** Element == Character { **func** index(of character: Character, after index: Int) -> Int? { **for** i **in** index..<**self**.count **where** **self**[i] == character { **return** i } **return** **nil** }}
```

# 部分替换

假设您正在制作一个 IDE 或一些编辑器，在那里用户可以用另一部分替换字符串的一部分。我们可以使用下面的方法。

```
str.replacingOccurrences(of: "xcode", with: "Xcode")
```

同样，如果我们想替换两个给定字符或子字符串之间的字符串，我们可以使用下面的。

```
guard let startIndex = str.firstIndex(of: "a") else { return false}
guard let endIndex = str.firstIndex(of: "o") else { return false}str.replaceSubrange(startIndex...endIndex, with: "oil")
```

# 边界测试

测试子字符串是否存在于字符串的开始或结尾。

## 从开始

```
str.hasPrefix("i")// testing that a string is lexicographically greater than otherstr.lexicographicallyPrecedes("zzz")
```

## 从结束

```
str.hasSuffix("i")
```

# 两根弦的区别

作为开发人员，我们可能会遇到这样一个问题:str1 要变成 str2 需要做多少修改(插入和删除)。

```
str = "oil"**let** dif = str.difference(from: "oii")print(dif) /* **CollectionDifference<Character>(insertions: [Swift.CollectionDifference<Swift.Character>.Change.insert(offset: 2, element: "l", associatedWith: nil)], removals: [Swift.CollectionDifference<Swift.Character>.Change.remove(offset: 2, element: "i", associatedWith: nil)]) */**print(dif.insertions.count+dif.removals.count) // 2
```

# 最高的

在一个字符串中寻找最高的字符并不复杂。但是字符串中的 max()有些模糊。以下有助于澄清这一点

```
str = "aAB"str.max() // a
```

请注意，小写字母被视为最大值。

# 小数计数/小写字母/大写字母/…(字符集)

正如标题所说，从字母数字串中只计算字母、小数、小写字母甚至更多数字会很有帮助。下面是这样做的小扩展。

```
**extension** String { **func** count(of characterSet: CharacterSet) -> Int { **var** count = 0 **var** range = **self**.rangeOfCharacter(from: characterSet) **while** range != **nil** { count = count + 1 **guard** **let** ub = range?.upperBound **else** { **break** } range = **self**[ub...].rangeOfCharacter(from: characterSet) } **return** count  }} 
```

就是这样…

感谢阅读…