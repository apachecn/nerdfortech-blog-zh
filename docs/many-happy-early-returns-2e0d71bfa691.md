# 祝早日康复

> 原文：<https://medium.com/nerd-for-tech/many-happy-early-returns-2e0d71bfa691?source=collection_archive---------27----------------------->

![](img/66eee47946cedf4388fa9a5d4eded098.png)

我决定写这篇文章，因为我记得当我在 2013-2014 年左右开始学习 Scala 时，如何从循环中提前返回的问题确实在我身上发生过几次。我有一个集合，我想通过它来寻找满足某些要求的第一个条目，然后我想跳出这个循环，把那个合适的结果带走。当时我正在用 Java 开发一个时间管理 webapp，上面描述的情况经常发生在我身上:有一个很长的条目集合，记录了一个给定的人在给定的一天中在给定的项目上工作了多少时间，我对它们进行了复杂的查询。*“找出一天中团队工作超过 X 人时的事件”*。*“找出一个花了 Y 天以上修复的 bug 的例子”*。诸如此类。在 Java 中，我通常遵循同一个模式 **—** 我收集原始条目(姑且称之为`Foo`类型的`foos`),执行有时相当昂贵的到衍生条目(类型`Bar`的`bar`)的转换，然后对那个`bar`进行有时也相当复杂的验证，检查它是否满足要求，如果是，那么我返回它。如果没发现什么，就返回`null`。

```
Bar complexConversion(Foo foo) {
  ...
}bool complexValidation(Bar bar) {
  ...
}Bar findFirstValidBar(Collection<Foo> foos) {
  for(Foo foo : foos) {
    Bar bar = complexConversion(foo)
    if (complexValidation(bar)) return bar
  }
  return null
}
```

## 命令式方法

很自然，作为一名 Java 开发人员，我在 Scala 中的第一步就是将 Java 代码几乎一对一地逐字翻译成 Scala:

```
def complexConversion(foo: Foo): Bar = ...
def complexValidation(bar: Bar): Boolean = ...def findFirstValidBar(seq: Seq[Foo]): Option[Bar] = {
  for (foo <- seq) {
    val bar = complexConversion(foo)
    if (complexValidation(bar)) return Some(bar)
  }
  None
}
```

这里唯一真正的区别是我避免了`null`，而是使用了`[Option](https://www.scala-lang.org/api/current/scala/Option.html)`。对于我们这里的所有实际目的来说，`Option`是一个集合，它可以由零个元素或一个元素组成。要么是`Some(bar)`，然后我以后可以从中提取出那个`bar`，要么就是`None`。

但这不是好的 Scala 代码，远非如此。关键字`return`，即使它存在于 Scala 中，也是强烈不鼓励的。大多数情况下，开发人员使用`return`作为一个块中的最后一条语句。在 Scala 中，每个代码块都是一个表达式，它自动返回代码块中最后一个表达式的结果。因此，你可以简单地在最后一行写下`x`，而不是`return x`，但大多数情况下你甚至不会这么做 **—** 事实上，一切都是一个表达式，这给了你重新排序整个代码块的能力，这在 Java 中看起来非常笨拙，最终得到更加简洁的 Scala 代码。这就排除了 90%的退货。至于另外的 10%，即大部分来自循环的早期回报，嗯...早期回报被认为是一种代码气味。它们降低了表达式序列的可读性，因为程序员不再依赖最后一条语句是从块中返回结果的语句。如果可能的话，你应该以这样一种方式重新排序代码，使得`return`不再必要。

但在这种情况下，这并不容易。首先，我学习了一些半解决方案来绕过它，然后我终于学会了正确的方法，然后我就把它忘得一干二净了，因为那时我有太多的东西要在同一时间学习，这只是让我的大脑为其他新概念腾出空间，直到最近我才想起我不时编写的代码实际上是旧 Java 早期版本的 FP 等价物。因此这篇文章。答案是…

## 婴儿学步

…我们将一步一步来。或者你可以跳过接下来的几段。但是我想邀请你和我一起看一下方法`findFirstValidBar`的几个中间版本，这样最终你会更好地理解为什么最终的解决方案看起来是这样，以及你还可以用它做什么。

首先，让我们沿着阻力最小的路线:Scala 中的每个标准集合都定义了方法`[find](https://www.scala-lang.org/api/current/scala/collection/Seq.html#find(p:A=%3EBoolean):Option[A])`和`[map](https://www.scala-lang.org/api/current/scala/collection/Seq.html#map[B](f:A=%3EB):CC[B])`。`find`返回集合中满足谓词的第一个元素作为选项 **—** 如果没有元素满足要求，将返回`None`。`map`将一种类型的元素集合转换为另一种类型的元素集合。由于`Option`也是一个集合，我们可以在这里使用它，因此将整个`findFirstValidBar`方法变成这样:

```
def findFirstValidBar(seq: Seq[Foo]): Option[Bar] = 
  seq.find(foo => complexValidation(complexConversion(foo)))
     .map(complexConversion)
```

对于`seq`中的每个`foo`，我们将其转换成一个`bar`，然后我们验证它，如果它通过了验证，那么我们停止对`seq`的迭代，并返回...嗯，`find`返回的是原`foo`，而不是导数`bar`，所以我们需要把那个`foo`再转换一次才能返回。如果转换是微不足道的，我们可以忽略这个缺点 **—** 毕竟，它只是一个额外的转换，或者如果我们没有`find`任何有效的元素就等于零 **—** 但是一般来说，我不喜欢满足于一个次优的解决方案，特别是如果一个更好的解决方案不是那么复杂的话。

在 Scala 标准集合库中有一个名为`[collect](https://www.scala-lang.org/api/current/scala/collection/Seq.html#collect[B](pf:PartialFunction[A,B]):CC[B])`的方法，它合并了`[filter](https://www.scala-lang.org/api/current/scala/collection/Seq.html#filter(pred:A=%3EBoolean):C)`和`map`的功能。过滤一个集合，然后将结果映射到其他东西的集合，这种情况非常普遍，因此用更短的代码编写会更有优势。同样，还有一个方法`[collectFirst](https://www.scala-lang.org/api/current/scala/collection/Seq.html#collectFirst[B](pf:PartialFunction[A,B]):Option[B])`合并了`find`和`map` **—** 毕竟`find`只是一个在找到第一个有效元素后停止的`filter`。因此，我们可以修改上面的版本，并将其编写为:

```
def findFirstValidBar(seq: Seq[Foo]): Option[Bar] =   
  seq.collectFirst {
    case foo if complexValidation(complexConversion(foo)) => complexConversion(foo) 
  }
```

花括号和以 case 开头的一行表示`collectFirst`接受一个参数 **—** 一个部分函数。只有满足条件(`complexValidation(complexConversion(foo))`)时，该功能才会产生结果(`complexConversion(foo)`)。如果产生了结果，`collectFirst`将停止对`seq`的迭代并返回。否则，它将尝试另一个`foo`。

## `unapply`

就其本身而言，`collectFirst`仍然不能解决我们的问题。为了检查条件是否满足，我们需要执行转换和验证，然后，如果验证成功，我们需要再次转换原始的`foo`，就像在带有`find`和`map`的版本中发生的一样。但是这次我们得到了一个提示。Scala 中的部分函数利用了模式匹配 **—** 就像我们做`match/case`一样，部分函数中的 case 可能被用来将元素`foo`解构为其他东西，而这种解构机制，也称为`unapply`方法，可以用于转换和验证。

但是等等，`unapply`不就是单纯用来从 case 类中提取字段值的吗？

没有。

```
object ValidBar {
  def unapply(foo: Foo): Option[Bar] = {
    val bar = complexConversion(foo)
    if (complexValidation(bar)) Some(bar) else None
  }
}
```

实际上，在代码的任何地方，我们都可以创建一个新的对象，将其命名为有意义的对象(注意，对象不一定总是类的伙伴对象)，并在其中实现一个`unapply`方法，该方法将接受一种类型的实例，并返回另一种类型的`Option`。在我们的例子中，它使用一个`foo`，转换它，验证它，如果验证成功，返回`Some(bar)`，否则返回`None`。之后，我们可以在任何地方使用`unapply`方法进行模式匹配，就像这样:

```
foo match {
  case ValidBar(bar) => // do something with our valid bar
  case _ => // ...
}
```

所以我们用在`collectFirst`里吧:

```
object ValidBar {
  def unapply(foo: Foo): Option[Bar] = {
    val bar = complexConversion(foo)
    if (complexValidation(bar)) Some(bar) else None
  }
}def findFirstValidBar(seq: Seq[Foo]): Option[Bar] =   
  seq.collectFirst {
    case ValidBar(bar) => bar 
  }
```

胜利！现在我们只有一次转换，一次验证，当我们找到第一个有效元素时，我们就完成了对`seq`的迭代。最重要的是，我们也可以在`collect`方法中使用它:简单的`seq.collect { case ValidBar(bar) => bar }`将使用所有通过验证的条。

然而，我们可能不喜欢这个解决方案的冗长之处。这不再是一句俏皮话了。其实比原来的`findFirstValidBar`命令版还要长。但这只是因为这个解决方案在开始时需要一定的开销:即使对于最简单的应用程序，我们也需要一个对象和一个`unapply`方法。但是随着应用程序 **—** 即转换和验证 **—** 变得更加复杂，开销变得不那么重要。我们只需要把`unapply`写一遍，然后就可以在`collectFirst`里到处用了。事实上，有一种情况下`unapply`甚至可能让我们节省几行。

想象一下，从`Foo`到`Bar`的转换并不总是可能的。我们需要用`def safeComplexConversion(foo: Foo): Option[Bar]`代替`def complexConversion(foo: Foo): Bar`，如果转换成功，我们返回`Some(bar)`，否则返回`None`。现在我们的命令式版本`findFirstValidBar`需要看起来像这样:

```
def safeComplexConversion(foo: Foo): Option[Bar] = ...
def complexValidation(bar: Bar): Boolean = ...def findFirstValidBar(seq: Seq[Foo]): Option[Bar] = {  
  for (foo <- seq) 
    safeComplexConversion(foo) match { 
      case Some(bar) if complexValidation(bar) => return Some(bar)    
      case _ => 
    } 
  None
}
```

它更复杂，也更丑陋。`return`关键字不仅嵌套在 for 循环中，还嵌套在`match/case`中。这里我们再次遇到了为什么不应该使用`return`的原因。在 Scala 代码中，多层嵌套非常常见**—**lambdas、匿名类和模式匹配等特性让我们的代码更加简洁。但是这些代码中的关键字`return`使得阅读起来更加困难。

## 防止不可能转换的安全

同时，由于我们的`unapply`方法处理选项，所以`findFirstValidBar`的上一版本中的变化使得代码比以前更加简单:

```
def safeComplexConversion(foo: Foo): Option[Bar] = ...
def complexValidation(bar: Bar): Boolean = ...object ValidBar {
  def unapply(foo: Foo): Option[Bar] =     
    safeComplexConversion(foo).find(complexValidation)
}def findFirstValidBar(seq: Seq[Foo]): Option[Bar] =   
  seq.collectFirst {
    case ValidBar(bar) => bar 
  }
```

此外，转换和验证方法也需要放在某个地方——如果它们总是一起使用，现在它们可以放在对象内的`unapply`旁边。这意味着如果`Foo`需要在您的代码中以多种方式进行解构——从一个原始数据结构中派生出一个以上的二级类型是很常见的——您将只需要关心那些派生的有意义的对象名称:

```
object ValidBar {
  def convert(foo: Foo): Option[Bar] = ...
  def validate(bar: Bar): Boolean = ...
  def unapply(foo: Foo): Option[Bar] = convert(foo).find(validate)
}
// use as case ValidBar(bar) => ...object ValidBaz {
  def convert(foo: Foo): Option[Baz] = ...
  def validate(baz: Baz): Boolean = ...
  def unapply(foo: Foo): Option[Baz] = convert(foo).find(validate)
}
// use as case ValidBaz(baz) => ...object BarValidInADifferentWay {
  def convert(foo: Foo): Option[Bar] = ...
  def validate(bar: Bar): Boolean = ...
  def unapply(foo: Foo): Option[Bar] = convert(foo).find(validate)
}
// use as case BarValidInADifferentWay(bar) => ...
```

这允许大量的代码重用。我们现在不仅可以在`collectFirst`和`collect`中使用那些`unapply`方法，还可以在`match/case`和 Scala collections 库中几乎任何接受部分函数的方法中使用。`map`、`flatMap`、`foreach` —随你所需。我们还可以在部分函数中组合它们，比如说，如果我们寻找一个可以以某种方式转换和验证的 foo:

```
seq.collectFirst {
  case ValidBar(bar) => bar
  case BarValidInADifferentWay(bar) => bar
} // stops at the first element valid in any of those ways
```

## 所有解构的共同特征

如果你习惯了这种模式，你可能会注意到`unapply`总是相同的，并提取出一个特征的共同逻辑:

```
// one such trait for the whole codebase
trait Deconstruct[From, To] {
  def convert(from: From): Option[To]
  def validate(to: To): Boolean
  def unapply(from: From): Option[To] = 
    convert(from).find(validate)
}// one for each implementation of convert and validate
object ValidBar extends Deconstruct[Foo, Bar] {
  override def convert(foo: Foo): Option[Bar] = ...
  override def validate(bar: Bar): Boolean = ...
}// for each use
def findFirstValidBar(seq: Seq[Foo]): Option[Bar] =   
  seq.collectFirst {
    case ValidBar(bar) => bar 
  }
```

## 懒惰的收藏

在我发表了这篇博客的最初版本后，我注意到至少有一种方法可以达到懒惰收集的相同效果(谢谢你，Balmung-san！).惰性集合是这样一种集合，它不是已经计算好元素并准备好访问，而是在需要时计算给定元素。这意味着，由于我们只在找到满足特定条件的第一个元素之前遍历集合，并且当我们找到它时，我们不再对访问任何其他元素感兴趣，因此集合不会为它们运行转换和验证方法。

Scala 中的一个懒惰集合是[迭代器](https://www.scala-lang.org/api/current/scala/collection/Iterator.html)。我们可以简单地通过编写`seq.iterator`从原始的`Seq[Foo]`创建一个迭代器。查找有效条形所需的全部代码如下所示:

```
def findFirstValidBar(seq: Seq[Foo]): Option[Bar] =
  seq.iterator
     .map(safeComplexConversion)
     .find(_.exists(complexValidation))
     .flatten
```

然而，这涉及到一些开销。保留迭代器是不安全的——我们每次都应该从原始的`seq`开始创建它。此外，代码的可重用性也降低了——如果我们有多种方法来转换和验证一个元素，我们将需要编写比`unapply` + `collectFirst`更多的代码。但总的来说，这是一个很好的例子，说明在 Scala 中，任何事情都可以用多种方式来完成，这取决于我们愿意接受什么样的权衡。

仅此而已。感谢阅读。希望这些都能对你有用。

## 几个链接:

*   一个包含所有例子的要点。
*   一个关于`unapply`方法的[视频](https://www.youtube.com/watch?v=83DmfRHQxfE)(和它的[同伴博客笔记](https://makingthematrix.wordpress.com/2021/02/09/programming-with-functions-4-the-unapply-method-and-the-newtype-pattern/))。
*   [另一个视频](https://www.youtube.com/watch?v=RX1_EJp9Vxk)和一个关于部分功能的[博客笔记](https://makingthematrix.wordpress.com/2020/12/15/programming-with-functions-2-functions-as-data/)。

[封面照片](https://www.flickr.com/photos/lucian-f/50409684038/in/photolist-2jNwNPq-5fGemX-2kfZ1fh-p6fR58-2cB5d9K-tbSUku-tRixxy-2jFX2fQ-26oFLhW-RV79UJ-tRiy93-VwPLqk-Zb3RuG-KCyzGP-cTxypj-FNu2ih-jgZLQu-cM6sg7-2irxaoQ-EYj6Wg-2kASpXv-e4XpVS-2i1aP32-2hVrG7g-2i8p7CE-6aqKYa-G8HLT4-2ktepSS-eSaj9W-2gx78WG-2aZLS5D-EntzFH-S8s5W5-wBXnkt-2jrwvu6-2k2dRGw-2hGWzmX-zqYien-4MuqbD-2huW4ev-2i6f37D-8CXUBh-2gk9DyB-aageGi-27FfkR4-2dfGbjY-224CPPX-L4MpDK-23McGB2-9Z7MNB)由 Lucian 从 Flickr 制作，[知识共享，版权所有](https://creativecommons.org/licenses/by-nc-nd/2.0/)。