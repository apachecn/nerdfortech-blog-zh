# Javascript 字符串 localeCompare()的基础知识(方法)

> 原文：<https://medium.com/nerd-for-tech/basics-of-javascript-string-localecompare-method-b2aa50207706?source=collection_archive---------4----------------------->

![](img/c6f2621d6eb4737775f1e1f380c77cc3.png)

这篇文章是我在 youtube 上免费发表的关于网络开发基础的系列文章的抄本。如果你更喜欢看而不是读，请随时访问我的频道“Dev Newbs”。

你好，戴夫·纽布斯！我们今天要讲的方法对我来说非常重要，可能对你们这些来自字母表中使用特殊字符的国家的人来说也是如此。顺便说一下，我来自斯洛伐克，我们的单词和名字中有很多发音符号。因此，如果你像我一样，需要订购单词、名称或任何由字符串组成的东西，这就是适合你的方法。

LocaleCompare()方法返回一个数字，该数字指示一个引用字符串在字母顺序上是属于前面还是后面，或者在排序顺序上是否与给定的字符串相同。此方法比较当前区域设置中的两个字符串。区域设置基于浏览器的语言设置。

该方法的参数如下:

*   **比较字符串** —用于与引用字符串进行比较的字符串
*   **区域设置** —指定用于比较的语言的值
*   **选项** —包含允许进一步指定方法行为的参数的对象

在所有浏览器中，**区域设置** & **选项**可能不会以相同的方式实现。如果根本不执行，就会被忽略。有一种方法可以在区域设置和选项中指定相同的属性，但是有三个原因我不会教你如何在区域设置中这样做。第一个是已经提到的——它不是在每个浏览器中都实现的。第二，我不喜欢它，因为它很笨拙。第三，您可以在 options 对象中覆盖这些设置。

结果得到的数字是以下三个选项之一:

*   **负数** —如果参考字符串按字母顺序排在比较字符串之前
*   如果两个字符串相等
*   **正数**如果参考字符串按字母顺序排在比较字符串之后

因为 W3C 规范没有规定使用确切的数字，只规定了结果数的正负值，所以有的浏览器可以提供 1/-1，有的可以提供 2/-2 甚至完全不同的值。所以只检查返回值是多还是少或者等于零。

好了，定义到此为止。让我们看看这个方法的实际应用。

```
// 0123456789aAbBcCdDeEfFgGhHiIjJkKlLmMnNoOpPqQrRsStTuUvVwWxXyYzZ{
    let referenceStr = "0";
    let compareString = "9"; // '0' is alphabetically before '9'
    referenceStr.localeCompare(compareString)           // -1
}{
    let referenceStr = "9";
    let compareString = "a";// '9' is alphabetically before 'a'
    referenceStr.localeCompare(compareString)           // -1
}{
    let referenceStr = "a";
    let compareString = "A";// 'a' is alphabetically before 'A'
    referenceStr.localeCompare(compareString)           // -1
}{
    let referenceStr = "A";
    let compareString = "b";// 'A' is alphabetically before 'b'
    referenceStr.localeCompare(compareString, 'en')     // -1
}
```

我在示例的开头添加了一个注释，以显示最常见字符的排序顺序——即数字，然后是以小写字母开头的字母，最后是大写字母。四个示例结果证实了这一基本顺序。

现在，我们讨论了音调符号，以及一些语言中基本字母的不同变体。例如，德国人和瑞典人都知道字母“δ”。但是他们的语言环境处理这个字母的方式有所不同。我们将在第二个示例中了解这一点:

```
// a negative value: in German, ä sorts before z
'ä'.localeCompare('z', 'de')                               // -1// a positive value: in Swedish, ä sorts after z
'ä'.localeCompare('z', 'sv')                               // 1// in German, ä has a as the base letter
'ä'.localeCompare('a', 'de', { sensitivity: 'base' })      // 0// in Swedish, ä and a are separate base letters
'ä'.localeCompare('a', 'sv', { sensitivity: 'base' })      // 1
```

第一对结果告诉我们，对于德国人来说,“a”在字母“z”之前，但是对于瑞典人来说，他们将带有发音符号的字符放在字母表的基本字母集之后。

类似的情况出现在第二对结果中，这两个结果说明了两个地区在字母的“基础”方面的差异。对德国人来说,“a”只是一个带有附加符号的基本字母“a”。他们不认为这是一封单独的信。另一方面，瑞典人认为这是一封单独的信。

好吧，就这样。如果你认为地方不好，那就准备好享受吧。我们现在将讨论选项。

现在，options 对象有了更多可用的属性，但是我将只介绍那些对我有用并且实际上有意义的属性。请随意检查其余部分。

第一个也可能是最常用的一个是属性“敏感度”。您可以将其设置为以下四个值之一:

*   基础
*   口音
*   情况
*   变体(默认)

当我们使用敏感度选项时，对每对比较的字母进行三次检查:

*   两个基本字母是否应该视为相等
*   基本字母和带有音调符号标记的版本是否应视为相等
*   字母的大写和小写版本是否应视为相等

四个可用灵敏度选项中的每一个都设置了这些检查的不同组合。你可以在下面的例子中看到。

```
// options - sensitivity
// 'BASE: a ≠ b, a = á, a = A'
'a'.localeCompare('b', 'en', { sensitivity: 'base' })       // -1
'a'.localeCompare('á', 'en', { sensitivity: 'base' })       // 0
'a'.localeCompare('A', 'en', { sensitivity: 'base' })       // 0// 'ACCENT: a ≠ b, a ≠ á, a = A'
'a'.localeCompare('b', 'en', { sensitivity: 'accent' })     // -1
'a'.localeCompare('á', 'en', { sensitivity: 'accent' })     // -1
'a'.localeCompare('A', 'en', { sensitivity: 'accent' })     // 0// 'CASE: a ≠ b, a = á, a ≠ A'
'a'.localeCompare('b', 'en', { sensitivity: 'case' })       // -1
'a'.localeCompare('á', 'en', { sensitivity: 'case' })       // 0
'a'.localeCompare('A', 'en', { sensitivity: 'case' })       // -1// 'VARIANT (default): a ≠ b, a ≠ á, a ≠ A'
'a'.localeCompare('b', 'en', { sensitivity: 'variant' })    // -1
'a'.localeCompare('á', 'en', { sensitivity: 'variant' })    // -1
'a'.localeCompare('A', 'en', { sensitivity: 'variant' })    // -1
```

您可以在注释行中检查每个灵敏度值的组合设置。如果未提供敏感度，则默认为“变量”，这是最敏感的变量。

最后一个示例将显示另外两个属性，即“numeric ”,它指定是否比较两个字符串，就像它们表示一个数字一样。默认设置为 false。

有趣的是，即使你用值 false 定义了 numeric 属性，Chrome 仍然表现得好像你选择了“true”。在某种程度上，这是有意义的，因为当默认情况下 numeric 为 false 时，为什么要将它指定为 false 呢？但是我仍然认为——这是一种有点懒惰的实现方法。

我想提到的另一个属性是“caseFirst ”,它决定字母的小写/大写版本中哪一个先出现。

其值可以是:

*   大写字母—大写字母在前
*   小写——小写优先
*   false(默认)-使用区域设置的默认排序

好了，说够了，下面是例子:

```
// options - numeric
// 'NUMERIC: true, false (default)'
'2'.localeCompare('10', undefined)
'2'.localeCompare('10', undefined, { numeric: 'true' })
'2'.localeCompare('10', undefined, { numeric: 'false' })// options - caseFirst
// 'CASE FIRST: upper, lower, false (default)'
'a'.localeCompare('A', 'en', { caseFirst: 'upper' })
'a'.localeCompare('A', 'en', { caseFirst: 'lower' })
'a'.localeCompare('A', 'en', { caseFirst: 'false' })
```

请记住，如果您在不同的浏览器或相同的浏览器中运行这些示例，但版本不同，结果可能会有所不同。此外，如果您的默认区域设置与我的不同，结果可能会稍有不同。

今天到此为止。我知道这很难，但如果你来了，好好享受吧。结束了，你做得很好。

一如既往地感谢您的关注，我们将在下一篇文章中再见。