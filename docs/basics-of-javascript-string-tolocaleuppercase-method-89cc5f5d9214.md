# Javascript String to localeuppercase()的基础知识(方法)

> 原文：<https://medium.com/nerd-for-tech/basics-of-javascript-string-tolocaleuppercase-method-89cc5f5d9214?source=collection_archive---------16----------------------->

![](img/9ac53a9d9bdedc6b119499c9e513303c.png)

这篇文章是我在 youtube 上免费发表的关于网络开发基础的系列文章的抄本。如果你更喜欢看而不是读，请随时访问我的频道“Dev Newbs”。

你好，我的新开发者伙伴们！如果你不知道，那是“新手开发者”的捷径。好吧，又一个奇怪方法的奇怪开始。检查。让我们进入正题。

toLocaleUpperCase()方法根据任何特定于区域设置的大小写映射，返回转换为大写的调用字符串值。

通常，该方法返回与 toUpperCase()方法相同的结果。但是，对于某些语言环境，如果发生与常规 Unicode 大小写映射的语言冲突(如土耳其语)，结果可能会有所不同。

locale 参数指示用于根据任何特定于区域设置的大小写映射转换为大写的区域设置。如果一个数组中给定了多个区域设置，则使用最佳的可用区域设置。

如果您没有提供任何区域设置作为第一个参数，它会根据主机的当前区域设置将字符串转换为大写字母。

```
const str = "istanbul";// get default locale for my browser
"My default locale: '" + navigator.language + "'"// OUTPUT: 
// My default locale: 'sk-SK'// my default (slovak) browser's locale
str.toLocaleUpperCase()                     // ISTANBUL// english US locale
str.toLocaleUpperCase('en-US')              // ISTANBUL// Turkish locale
str.toLocaleUpperCase('tr')                 // İSTANBUL// compare the values of uppercase results with different locales
str.toLocaleUpperCase('en-US') === str.toLocaleUpperCase()// OUTPUT: truestr.toLocaleUpperCase() === str.toLocaleUpperCase('tr')// OUTPUT: falsestr.toLocaleUpperCase('tr') === str.toLocaleUpperCase('en-US')// OUTPUT: false
```

大写单词“伊斯坦布尔”的土耳其语版本在视觉上不同于默认和英语区域设置转换的单词。它与其他两个的比较也导致 false。

转换的结果也有可能是一个异常。准确地说，我们可以得到两种不同类型的异常。

第一个是 RangeError，当 locale 参数无效时抛出。第二个是 TypeError，如果 locales 数组中的项不是 string 类型。让我们在示例 2 中看看它们的作用。

```
let locales = ['tr-TR', 2];try {
    // RangeError
    console.log(str.toLocaleUpperCase('engl'));
} catch (err){
    console.log(err);
}// OUTPUT: 
// RangeError: Incorrect locale information provided
// at String.toLocaleUpperCase (<anonymous>)try {
    // TypeError
    console.log(str.toLocaleUpperCase([2, 'en-US']));
} catch (err){
    console.log(err);
}// OUTPUT:
// TypeError: Language ID should be string or object.
// at String.toLocaleUpperCase (<anonymous>)
```

我们得到一个 RangeError，因为提供的字符串“engl”不代表任何有效的区域设置。类似地，我们得到 TypeError，因为数字 2 不属于 locales 数组中允许的输入类型。

它涵盖了该方法的其他区域设置版本。下一次，我们将看看这个方法的非本地版本。

感谢大家关注并坚持这一集到最后。我很感激。我会很快看到你的另一篇文章。