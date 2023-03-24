# 马特的花絮# 98——真相会让你自由(除非你使用 JavaScript)

> 原文：<https://medium.com/nerd-for-tech/matts-tidbits-98-the-truth-will-set-you-free-unless-you-re-using-javascript-46f841583636?source=collection_archive---------19----------------------->

![](img/38aac84142469fb3e19295382d341213.png)

[上次我分享了一个关于运行单个单元测试的快速花絮。](/nerd-for-tech/matts-tidbits-97-running-debugging-individual-jest-unit-tests-36f7464fbaf9)这一次，我想分享我在调试一个失败的单元测试时学到的一些有趣的事情。

如果您使用 JavaScript 已经有一段时间了，那么您可能会遇到它在布尔表达式中稍微有些特殊的行为。在许多其他语言中，`if`语句只能包含布尔表达式，要么是布尔变量，要么是返回布尔值的其他表达式，如:大于、小于、等于、不等于等。

然而，在 JavaScript 中，所有变量本身都是真/假的——这被称为“真”，你可以在这里查看完整的表格:[https://developer.mozilla.org/en-US/docs/Glossary/Truthy](https://developer.mozilla.org/en-US/docs/Glossary/Truthy)

这变得有问题的地方是很容易误用它或者在你的代码中有细微的错误。在其他语言中，如果你把一个字符串放在一个`if`语句中，你会得到一个编译错误，因为它不是一个布尔表达式。但是在 JavaScript 中，您可以做到这一点而不会出现任何错误——甚至在运行时也不会。相反，每个值都被“强制”为布尔值。而且，规则并不总是直观的。

需要记住的重要一点是，只有以下值的计算结果为`false`——所有其他值的计算结果为`true:`

*   错误的
*   0
*   -0
*   0n (BigInt 0)
*   " "(空字符串)
*   空
*   不明确的
*   NaN(非数字)

坦白地说，这个列表看起来并没有那么糟糕。这是有道理的(不要让我从`-0`(负零)开始——那只是写满了危险)，但是让一个空字符串求值为`false`也是有道理的。然而，没有意义的是，空数组`[]`的计算结果是`true`。来吧伙计们——如果你要有一个非常灵活/动态的类型系统，至少要保持一致！数组和字符串都可以是`null`或`undefined`——那么为什么空字符串和空数组被区别对待呢？

答案在于 JavaScript 语言的微妙之处——字符串是一种特殊的类型，而数组是对象。在 JavaScript 中，所有对象引用(只要不是`null`或`undefined`)的计算结果都是`true`。

无论如何，正如你现在可能已经收集到的，我经历了这个确切的问题，我期望一个空数组是`false`，但它实际上是`true`，我通过调试器运行单元测试来解决它。

这个故事的寓意是，如果你不熟悉/不熟悉某种语言，确保你仔细阅读(并重读)文档，并在调试时挑战你的假设。

你最喜欢的 JavaScript“真实”故事是什么？请在评论中告诉我！

有兴趣和我一起在埃森哲出色的数字产品团队工作吗？我们正在招聘[本地移动开发者](https://www.accenture.com/us-en/careers/jobdetails?id=00960587_en&title=Native+Mobile+Developer)、[混合移动开发者](https://www.accenture.com/us-en/careers/jobdetails?id=00960583_en&title=Hybrid+Mobile+Developer)、[网络开发者](https://www.accenture.com/us-en/careers/jobdetails?id=00906120_en&title=Web+Developer)，以及[更多](https://www.accenture.com/us-en/careers/jobsearch?jk=%22Digital%20Products%22&sb=0&pg=1&is_rj=0&ct=ma%20-%20cambridge|ny%20-%20new%20york)！