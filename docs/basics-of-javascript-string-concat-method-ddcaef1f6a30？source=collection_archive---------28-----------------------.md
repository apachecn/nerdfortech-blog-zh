# Javascript String concat()的基础知识(方法)

> 原文：<https://medium.com/nerd-for-tech/basics-of-javascript-string-concat-method-ddcaef1f6a30?source=collection_archive---------28----------------------->

![](img/f2bae8044bc34a1066d41fd38795e965.png)

这篇文章是我在 youtube 上免费发表的关于网络开发基础的系列文章的抄本。如果你更喜欢看而不是读，请随时访问我的频道“Dev Newbs”。

你好，我的新手伙伴们。另一天，另一个字符串方法覆盖。今天，它将成为一种有助于将事物整合在一起的方法。介绍 concat()方法。

concat()方法将两个或多个字符串连接在一起。该方法的结果是一个全新的字符串，它不会改变用作该方法参数的现有字符串。

这意味着对原始字符串或返回字符串的更改不会相互影响。

让我们看看如何在第一个例子中将两个或更多的字符串连接在一起。

```
let endOfSentence = "newbs!";
let conStr = "Hello ".concat(endOfSentence);conStr                                     // Hello newbs!let str01 = "My ";
let str03 = "is ";
let str04 = "Jacob!";
let resultStr = str01.concat("name ", str03, str04, " 🙂");resultStr                                  // My name is Jacob! 🙂
```

正如我们在示例中看到的，concat 不区分变量中的字符串和使用引号的表达式。它们可以互换使用。此外，表情符号和特殊字符根本不是问题。

要连接多个字符串，只需添加更多的字符串作为方法的参数。仅此而已。

如果参数不是字符串类型，它们将在连接之前被转换为字符串值。

```
let prefix = "Concat can deal with: ";
let ctr = prefix.concat(45, " - ", true," - ", ["a", 147.5, false]);ctr               // Concat can deal with: 45 - true - a,147.5,false
```

Concat()可以处理任何数值、布尔值甚至数组。

然而，有一些特殊情况值得一提。一如既往，百闻不如一见，这里再举一个例子。

```
"".concat({})            // [object Object]
"".concat([])            // ""
"".concat(null)          // null
"".concat(NaN)           // NaN
"".concat(undefined)     // undefined
"".concat(false)         // true
"".concat(1, 1)          // 11
```

Concat()不能转换一个对象，所以它只是用表示该对象已被提供的字符串替换该对象。空数组被替换为空字符串，像 null、NaN 或 undefined 这样的常量被转换为字符串，没有任何问题，就像前面的例子一样。

Concat()并不是将字符串组合在一起的唯一选项。事实上，还有更好的东西，你已经知道了。你可以使用赋值操作符——是的，这就是他们所说的加号。

看看下一个例子中 concat()方法和赋值操作符之间的比较。

```
// concat
{
  let i = 3;
  let s1 = "Number ".concat(i," times 2 equals ");
  let s2 = 2*i;
  let r = s1.concat(s2);
  console.log(r);               // Number 3 times 2 equals 6
}// assignment operators
{
  let i = 3;
  let s1 = "Number " + i + " times 2 equals ";
  let s2 = 2*i;
  let r = s1 + s2;
  console.log(r);               // Number 3 times 2 equals 6
}
```

好的，concat()可以做基本相同的事情。那么用哪个比较好呢？除了赋值操作符更容易编写之外，由于性能问题，强烈建议我们使用它而不是 concat()方法。

平均而言，赋值运算符的速度是 concat()方法的两倍。这是因为 concat()是一个字符串对象的方法，所以它实际上是一个函数。

如果你不相信我，看看我为你准备的最后一个例子。就像前面的例子一样，我们使用 concat()和 plus 操作符试图获得相同的结果字符串。但是，如果我们只运行一次命令，持续时间的差异将太小，无法进行比较。因此，我们将为每个选项运行相同的操作集一百万次，我们通过在完成百万次迭代后从时间戳开始之前减去时间戳来获得差异。之后，我们输出每个选项的持续时间。

```
// CONCAT
let startTime = new Date().getTime();for(let i = 0; i < 1000000; i++){
  let str1 = "Number ".concat(i," times 2 is ");
  let str2 = 2*i;
  let resStr = str1.concat(str2);
}let endTime = new Date().getTime();
endTime - startTime                        // for example 239// ASSIGNMENT OPERATOR
let startTime2 = new Date().getTime();for(let i = 0; i < 1000000; i++){
  let str1 = "Number " + i + " times 2 is ";
  let str2 = 2*i;
  let resStr = str1 + str2;
}let endTime2 = new Date().getTime();
endTime2 - startTime2                      // for example 95
```

就这样，我们可以看到，赋值确实快了一倍。这在单次执行中不会有什么不同，但在现实生活中，您可能会对大量数据进行一些复杂的字符串修改，然后会有很大的不同。所以最好在将来记住这一点。

一如既往，非常感谢您的宝贵时间，我希望很快能在下一篇方法的文章中见到您。