# 相等性测试

> 原文：<https://medium.com/nerd-for-tech/testing-for-equality-e33559fea07?source=collection_archive---------17----------------------->

上个月在我的模拟 JavaScript 面试中提出的另一个话题是关于两个比较运算符的区别: [*==* 、和](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Equality) [*===*](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Strict_equality) 。我的采访者问我，根据所使用的操作符，特定代码的输出会是什么。这是一个非常基本的问题，但是在实际的 JavaScript 面试中，你很有可能会被问到这个问题。

**在哪些方面平等？**

一、 *==* 和 *===* 有什么区别？第一个称为相等运算符，用于比较值。第二个称为严格相等运算符，比较值和数据类型。

看一下下面的代码。你认为输出会是什么？(提示:想想布尔。)

```
let num = 75let str = “75”console.log(num == str)
```

控制台的输出将是… *真*。为什么对这些变量使用 *typeof* 操作符可以确认我们的 *num* 变量保存一个数字，我们的 *str* 变量保存一个字符串。那么，为什么相等运算符显示它们具有相同的值呢？

如果我们使用严格的等式操作符呢？

```
let num = 75let str = “75”console.log(num === str)
```

这次的输出是*假*。是什么导致了这些不同的结果呢？

这是由于 JavaScript 的两个特征:

1.  JavaScript 是一种弱类型语言，这意味着您可以将任何值赋给变量，而不必声明数据类型。

2.因此，JavaScript 允许类型强制，通过类型强制，一种数据类型的值可以自动(或者隐式地)转换为另一种数据类型。

在我们的第一个例子中，相等运算符将字符串“75”转换成一个数字，然后进行比较，从而显示出保存在 *num* 和 *str* 中的值相等。在第二个例子中，我们看到严格相等运算符在进行比较时同时考虑了值*和*数据类型；因此，它声明 *num* 和 *str* 不相等。

因此，例如，如果你在一行代码中看到 *x == y* ，代码会问，“x*x*的值是否等于 y ？”如果你看到 *x === y* ，它在问，“是不是 *x* 等于*和*类型等于 *y* ？”

**检查*不等式*和**

你可能遇到过*！JavaScript 中的*(称为*逻辑非运算符*)。顾名思义，它的目的是表示逻辑否定，即某事是*而不是*的情况。

遇到 [*怎么办！=*](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Inequality) 或者 [*！==*](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Strict_inequality) 在 JavaScript 代码中？这些检查不平等。第一个是不等运算符，它的目的是检查不相等的值。第二个是严格的不等运算符，检查不等的值和不等的数据类型。

让我们用前面的例子来尝试一下。

使用不等式运算符:

```
let num = 75let str = “75”console.log(num != str)
```

我们的产量？这次是*假*。我们的代码会问，“num*和 *str* 的值不相等吗？”像等号运算符一样，它使用类型强制进行比较，发现 *num* 和 *str* 的值相等，返回 *false* 。*

使用严格不等式运算符:

```
let num = 75let str = “75”console.log(num !== str)
```

因为*！==* ，就像 *===* 一样，同时考虑值和数据类型，我们的代码在这种情况下返回 *true* 。严格不等式运算符问道:“num*num*在值和数据类型上是否不等于 str*str*？”它不会像*一样执行类型强制！=* 会，所以它发现我们的两个变量的值和类型不相等，从而返回 *true* 。