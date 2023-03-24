# Javascript String replace()的基础知识(方法)

> 原文：<https://medium.com/nerd-for-tech/basics-of-javascript-string-replace-method-49ec8dbd2d76?source=collection_archive---------24----------------------->

![](img/8d5001efaca3eb21742edd362a0b629e.png)

这篇文章是我在 youtube 上免费发表的关于网络开发基础的系列文章的抄本。如果你更喜欢看而不是读，请随时访问我的频道“Dev Newbs”。

你好 devs！这一天的方法可以简单易用，但同时，它也可能是一个真正的背部疼痛。我们今天会更换物品。Meet 方法 replace()。让我们开始吧。

replace()方法返回一个新的字符串，其中一个模式的部分或全部匹配被替换。模式是第一个参数，它可以是正则表达式或简单的字符串值。替换是第二个参数，它可以是一个简单的字符串值，也可以是为每个匹配调用的函数。如果模式是字符串值或者正则表达式不包含全局标志，则只替换第一个匹配项。

让我们涵盖第一个例子中的所有组合。

```
const str = "The blue sweater itches. I'll wear a red t-shirt and a blue jeans.";let regExp = /blue/g;
let regExp2 = /blue|red/gi;// PATTERN:     string
// REPLACEMENT: string
"S  -> S  : " + str.replace("blue", "green") // OUTPUT:
// S  -> S  : The green sweater itches. I'll wear a red t-shirt and // a blue jeans.// PATTERN:     RexExp
// REPLACEMENT: string
"RE -> S  : " + str.replace(regExp, "green")// OUTPUT:
// RE -> S  : The green sweater itches. I'll wear a red t-shirt and // a green jeans.// PATTERN:     string
// REPLACEMENT: function
console.log
(
    "S  -> fn : " +
    str.replace
    (
        "sweater", 
        (x) => {
            return x.toUpperCase()
        }
    )
);// OUTPUT: 
// S  -> fn : The blue SWEATER itches. I'll wear a red t-shirt and a // blue jeans.// PATTERN:     RexExp
// REPLACEMENT: function
console.log
(
    "RE -> fn : " +
    str.replace(
        regExp2, 
        function (x) 
        {
            return x.toUpperCase()
        }
    )
);// OUTPUT:
// RE -> fn : The BLUE sweater itches. I'll wear a RED t-shirt and a // BLUE jeans.
```

有四种可用的“模式”和“替换”类型值的组合。

第一个是最简单的。我们正在寻找一个字符串“蓝色”并用字符串“绿色”替换它。第一个找到的字符串将被替换。不是全部。

第二种情况是使用正则表达式找到一个匹配项，用简单的字符串值“green”替换。因为我们在正则表达式中使用了全局标志，所以我们将替换所有的匹配，而不仅仅是第一个。

第三种情况是一个简单的字符串“毛衣”被一个函数代替。我们的函数将任何匹配转换为所有大写字符。因为我们使用一个简单的字符串作为模式，所以只替换第一个匹配。

最后一种情况——第四种情况——是正则表达式匹配被函数替换。我们再次使用了一个全局标志，所以我们找到了所有的匹配项，而不仅仅是第一个，我们将它们替换为匹配单词的大写形式。

我将最后两种情况分成了多行，这样更容易阅读，但是如果您愿意，也可以将所有内容放在一行中。真的没什么区别。

你可以用 replace 方法做很多很酷的事情。我挑选了其中的一些，向你展示所有的可能性。

第二个示例向您展示了如何轻松地切换名字和姓氏的顺序。如果你有一个名字的数组或列表，这真的很方便。

```
let re = /(\w+)\s(\w+)/;
let fullName = 'John Smith';
let newstr = fullName.replace(re, '$2, $1');
newstr                                              // Smith, John
```

我们的正则表达式指定由空格分隔的两组字符。您可以使用美元符号和组的创建顺序来访问该组，从第一个开始。我们有两个组$1 和$2。First 表示名，第二个表示姓。我们需要做的就是改变它们在输出中的顺序。

如果您需要在 CSS 样式和 JS 样式参数之间进行解析，下一个技巧会派上用场。Javascript 允许您使用 style 方法来设置元素的样式，但是它接受 camelcase 格式的 CSS 样式，而不是连字符格式。因此，如果您想更改左填充值，您应该键入“paddingLeft”而不是“padding-left”。幸运的是，借助 replace()方法，有一种简单的方法可以将 camelcase 格式转换为连字符格式。

```
function upperToHyphenLower(match, offset, string) {
    return (offset > 0 ? '-' : '') + match.toLowerCase();
}function camelCaseToHyphenFormat(propertyName) {
    return propertyName.replace(/[A-Z]/g, upperToHyphenLower);
}camelCaseToHyphenFormat("marginBottom")         // margin-bottom
```

转换的整个原理是我们在给定的字符串中寻找大写字母。如果我们找到一个，我们不仅用它的小写版本替换它，还用连字符作为前缀。仅此而已。

最后一个技巧可以将温度的物理单位从华氏温度转换为摄氏温度。我稍微调整了函数的原始版本，使其接受值，即使数字和表示华氏温度的字母 F 之间有空格。它还将结果数字和摄氏度之间的空间。

```
function f2c(x) {
    function convert(str, p1, offset, s) {
      return ((p1 - 32) * 5/9).toFixed(2) + ' C';
    }
    let s = String(x);
    let test = /(-?\d+(?:\.\d*)?)( )*F\b/g;
    return s.replace(test, convert);
  }f2c("212 F")                                          // 100.00 C
f2c("0F")                                             // -17.78 C
```

您可以简单地通过提供后跟字母“F”的任何数值来调用该方法。结果是相同的摄氏温度。

好吧，这真的很有趣，但是天下没有不散的宴席。今天就这样了。一如既往，感谢您的关注，我们下次再以另一种方式见面。