# Matt 的花絮#99 —优雅地处理 null

> 原文：<https://medium.com/nerd-for-tech/matts-tidbits-99-handling-null-elegantly-6d95200dae4?source=collection_archive---------12----------------------->

![](img/38aac84142469fb3e19295382d341213.png)

上周我写了一些 JavaScript 中的真实边缘案例。这一次，我有一个优雅地处理空值/未定义值的快速花絮。

被它的发明者 C.A.R. Hoare 称为“十亿美元的错误”,大多数程序员可能对此非常熟悉(以及为什么它可能被归类为错误！)

我们肯定都写过这样的代码:

```
if(foo != null) {
  // Do something with foo
}
```

但是，如果`foo`是一个有多层嵌套对象的对象呢？您可能会同意这样写有点麻烦:

```
if(foo != null) {
  if(foo.bar != null) {
    if(foo.bar.baz != null) {
      // Now it's safe to use foo.bar.baz
    }
  }
}
```

一些更现代的语言(Kotlin，JavaScript 2020，Swift 等。)支持所谓的“安全调用/可选链接”，它看起来像这样:

`x = foo?.bar?.baz?.doSomething()`

`?`表示只有当左侧不是`null`时，才应该评估右侧。但是，如果这个表达式的任何一部分是`null`，那么`x`将是`null`。

如果我们想要指定在任何一个`null`检查失败的情况下`x`应该有什么值呢？很明显，我们可以通过检查语句后的`x`是否为`null`来实现这一点，然后给它分配一个不同的值，但是如果我们希望`x`为常量，这是不可能的。

有些语言支持三元运算符，所以您可以这样做:

`x = foo?.bar?.baz ? foo?.bar?.baz?.doSomething() : <fallback value>`

在我看来，这是重复的，也是容易出错的——如果`doSomething()`返回`null`，那么`x`仍然可能是`null`！您可以通过将`?.doSomething()`放在三元运算符的`?`之前来解决这个问题，但是我们确定多次调用`doSomething()`是安全的吗？它可能会产生副作用，在我们的代码中引入微妙的错误，或者占用大量资源，降低应用程序的性能。

相反，我想提出一种更好的方法——零(稍)合并运算符:

`x = foo?.bar?.baz?.doSomething() ?? <fallback value>`

这样，`x`仍然可以是一个常数，我们将捕捉整个链中任何可能的`null`值，并且我们只调用`doSomething()`一次(最多一次)。

现在，如果你明白了，为什么我称之为 null *(ish)* 合并操作符？这是因为在 JavaScript 中，可选的链接操作符和空合并操作符适用于`null`和`undefined`。很整洁，是吧？

我在科特林工作时就知道这个概念，在那里这个功能被称为“猫王操作员”。它被写成`?:`——这看起来有点像猫王的头顶(眼睛&他标志性的卷发)——另外，在 Kotlin 社区中，我们可以记住，当“猫王已经离开大楼”(也就是说，如果你遇到了`null`并离开了可选链)时，发生在操作符右边的任何事情都会发生，是的，我知道——程序员有时可能是真正的书呆子。；-)

现在我已经使用 TypeScript(构建在 JavaScript 之上)在 React Native 中工作，我发现这个功能也存在于其中。这让我发现 Swift 也有同样的概念——“零合并运算符”(用`??`表示)。

总之，我认为越来越多的现代编程语言正在融合并支持越来越相似的代码编写方式，这真的很令人兴奋。每种语言都有其独特之处(以及优点和缺点)，但我认为我们越来越接近这样一个世界，Android 开发人员可以通读 iOS 代码库并理解正在发生的事情(不是回到他们仍然用 Objective C 编写的时候)，或者传统的 web 开发人员可以在移动应用程序上工作(由于 React Native 等技术，这已经成为现实)。

知道其他支持可选链接/空合并的语言吗？我很想在评论中听到你的意见！

有兴趣和我一起在埃森哲出色的数字产品团队工作吗？我们正在招聘[移动开发人员](https://www.accenture.com/us-en/careers/jobdetails?id=00960587_en&title=Native+Mobile+Developer)、[网络开发人员](https://www.accenture.com/us-en/careers/jobdetails?id=00906120_en&title=Web+Developer)，以及[更多的](https://www.accenture.com/us-en/careers/jobsearch?jk=%22Digital%20Products%22&sb=0&pg=1&is_rj=0&ct=ma%20-%20cambridge|ny%20-%20new%20york)！