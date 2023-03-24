# 使用 JavaScripts 绑定、调用和应用

> 原文：<https://medium.com/nerd-for-tech/using-javascripts-bind-call-and-apply-868c4347cea9?source=collection_archive---------13----------------------->

![](img/30fa904a13326397da5c652e12df5a6f.png)

照片由[尼克](https://unsplash.com/@helloimnik)通过[unsplash.com](https://unsplash.com/photos/4iQ3cUsE42U)拍摄

***关于 JavaScript 介绍性面试主题的迷你系列的第 3 部分。***

我的上一篇文章， [JavaScript: this 和 arrow 函数](/nerd-for-tech/javascript-this-and-arrow-functions-215a90973a05)，探讨了 Arrow 函数如何使用它们的词法范围来确定`this`的值。这篇文章将深入讨论 bind()、call()和 apply()以及它们如何与我们的`this`系列相结合。

为了充分理解这些功能，我想向您展示它们的共同点、主要区别以及各自的一些示例。先从他们的共同点说起。

所有这三个函数有一个主要的共同点:它们给函数`this`赋值，这允许我们控制`this`所引用的上下文。这种相似性是我们到目前为止在这个迷你系列中所有其他帖子的联系。由于`this`可能会令人困惑，并且根据执行范围的不同而不同，我们可以使用 bind、call 和 apply 来简化一切。这三个让我们设定范围，为自己定义`this`应该是什么。

![](img/5f41cb672c6074f992035d8b67d7286b.png)

经由[imgflip.com](https://imgflip.com/memegenerator)

也就是说，它们都有一些不同的地方。让我们很快看看这些区别是什么。

# A 参数

这三个函数的第一个不同之处在于它们接收参数的方式。bind 和 call 都接受单个逗号分隔的参数。另一方面，Apply 接受一组参数。我记得 apply 和 array 都是以字母 a 开头的。

# 执行时间

另一个区别是它们执行的时间。调用时，call 和 apply 都会立即执行。另一方面，Bind 在它返回的函数被调用后执行。

时间不同的原因是因为我们的下一个不同点…

# 返回值

call 和 apply 立即执行的原因是，它们的返回值是调用它们的函数返回的值。为了返回那个值，他们需要立即执行。

Bind 返回一个新的函数，这个函数必须首先被调用，这就是为什么它不立即执行的原因。

比较结束后，让我们看一些例子。

```
Bind1\.  const person = {
2\.    name: 'Bob',
3\.    sayHi: function() { return this.name }
4\.  }5\.  const globalHello = person.sayHi
6\.  console.log(`Hello ${globalHello()}`) 
7\.  // logs 'Hello'8\.  const boundHello = globallHello.bind(person)
9\.  console.log(`Hello ${boundHello()}`)
10\. // logs 'Hello Bob'
```

如您所见,`globalHello`并没有记录您所期望的内容。从全局范围调用时，`this.name`实际上是指全局`this`，并且是未定义的。在`boundHello`中将`person`绑定到`globalHello`会将`this`的值分配给 person，因此当`sayHi`被调用时，它可以访问`person`并可以访问其`name`属性 Bob。

我发现 bind 对于事件和异步回调特别有用。因为您可以在添加事件侦听器或启动异步函数时绑定它，然后在事件被触发或承诺被返回时等待执行。

```
Call1\.  const person = {
2\.    sayHi: function(lastName) { return this.name + ' ' + lastName}
3\.  }4\.  const firstPerson = {
5\.    name: 'Bob'
6\.  }7\.  console.log(`Hi ${person.sayHi.call(firstPerson, 'Jones')}`)
8\.  // logs 'Hi Bob Jones'
```

用`call`指出两件事。首先像我们之前讨论的一样，`call`被立即执行，其次它接受逗号分隔的参数。如果我们想通过一个年龄和职业，它会是这样的:

```
person.sayHi.call(firstPerson, 'Jones', 25, 'Blogger')
```

为了简单起见，我将对 apply 使用相同的示例，因为它们是同时执行的，因此除了参数之外看起来是一样的。这是最后一个添加了年龄和职业的例子。

```
person.sayHi.apply(firstPerson, ['Jones', 25, 'Blogger'])
```

调用和应用可以互换使用，参数是最大的区别。

*热门提示:您可以使用 apply 从数组中找到最大值，这非常有用，因为 JavaScript 数组没有 max 方法。*

```
1\. let arr = [1, 2, 3, 4, 5]
2\. Math.max(arr) => NaN
3\. Math.max.apply(null, arr) => 5
```

记住，对于所有三个函数，bind、call 和 apply，第一个参数将是它的值，之后的所有参数都将传递给被调用的函数。

希望读完这篇文章后，你能更好地理解这三个函数是如何工作的，以及为什么它们在 JavaScript 中如此重要。没有它们`this`很容易混淆，尤其是在较大的程序中。tmed

一如既往地在下面留下我错过的任何东西或其他用例的评论！

[***第一部分这是什么？JavaScripts 这个关键字***](/nerd-for-tech/what-is-this-javascripts-this-keyword-23cc6fab741a)

[***第二部分:JavaScript:本和箭头功能***](/nerd-for-tech/javascript-this-and-arrow-functions-215a90973a05)