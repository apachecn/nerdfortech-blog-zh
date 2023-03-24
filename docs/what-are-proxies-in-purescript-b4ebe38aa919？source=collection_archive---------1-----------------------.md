# Purescript 中的代理是什么？

> 原文：<https://medium.com/nerd-for-tech/what-are-proxies-in-purescript-b4ebe38aa919?source=collection_archive---------1----------------------->

你有没有遇到过这样的情况，你想传递一个类型而不是值，或者你被我刚才说的搞糊涂了？让我解释一下。

![](img/b27e76dcc511a6bb46472635e284abe0.png)

[Abolfazl Ranjbar](https://unsplash.com/@ranjbarpic?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/paper-rocket?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍照

让我们先了解一下，类型和值之间有什么区别。

```
myFavoriteNumber :: Int
myFavoriteNumber = 3
```

```
myName :: String
myName = "Saravanan"
```

```
newtype Email = Email String

myEmail :: Email 
myEmail = Email "saravanan.m@gmail.com"
```

在这里，所有出现在`::'后面的东西称为类型，出现在左边的东西称为值。

如果考虑`myFavoriteNumber`，`3`是数值，`Int`是类型。

我们总是传递值给一个函数，如果我们简单地传递`Int` 给它，而不是指定它的值，函数会做什么？(两个整数相加，输出是多少？)

有没有什么地方我们想传递类型而不是值？这可能吗？是的，这在很多情况下都是可能的和有用的。

我希望你们都熟悉类型类`[Bounded](https://pursuit.purescript.org/packages/purescript-prelude/6.0.1/docs/Data.Bounded#t:Bounded)`，它有两个值`top`和`bottom`，分别给出定义它的相应类型的最高值和最低值。(不知道也没什么大不了，继续看就好)

假设您想要创建一个函数来打印特定类型的最低和最高可能值

```
getRange :: forall a. Bounded a => {lowest :: a , highest :: a }
getRange  = { lowest :  bottom
            , highest :  top
            }
```

但是这个不行，为什么？编译器如何决定我们想要哪个顶部，哪个底部..？我们想要 String 或 Int 或任何其他类型的顶部？我们如何在这里指定类型呢？

我们可以这样做，

```
getRange :: forall a.(Bounded a) => a -> {lowest :: a,highest :: a }
getRange _ = { lowest :  bottom
             ,  highest :  top
             }
```

我们可以这样调用这个函数

```
>>> getRange 1
{lowest: -2147483648, highest: 2147483647 }

>>> getRange false
{lowest: false, highest: true }

>>> getRange 1.0 
```

现在 purescript 编译器知道我们想要哪个`top`，`bottom`。

但是传递一个值在这里没有意义，我们甚至没有使用这个值。那么为什么我们在这里传递一个像`1`、`false`、`1.0`这样的值呢？

只能通过`Int`、`Boolean`、`Number`类型吗？

是的，我们可以，这正是代理人的作用。让我们看看代理的定义

```
data Proxy a = Proxy
```

代理只有一个没有参数的构造函数(Proxy)，有趣的部分是左边的`a`，通过这个`a`(称为[幻影类型](https://wiki.haskell.org/Phantom_type))我们可以`encode`类型信息。

我这里说的`encode`到底是什么意思？

代理将充当 T 类型的持有者，你可以在其中编码(持有)一个类型，请看下面的例子

```
_intProxy :: Proxy Int
_intProxy = Proxy_numberProxy :: Proxy Number
_numberProxy = Proxy
```

这里`_intProxy`和`_numberProxy`的值是相同的(**代理**)(是的，它们实际上是没用的)

有趣的是他们的类型。

它们的类型以一种非常微妙的方式不同(**代理<类型>类型**)这里的 ***<类型>*** 帮助携带*额外的类型信息。*

对于`_intProxy`其`Int`对于`_numerProxy`其`Number`

那么我们如何在上面的例子中使用编码的类型信息呢？

在上面的函数`getRange`中，我们可以不传递`a`，而是传递`Proxy a`

```
getRange :: forall a. (Bounded a) 
           -> Proxy a
           => {lowest :: a , highest :: a }
getRange _ = { lowest :  bottom
             ,  highest :  top
             }
```

现在你可以做一些事情

```
>>> getRange _intProxy 
{lowest: -2147483648, highest: 2147483647 }
```

```
>>> getRange (Proxy :: Proxy Boolean)-- boolean proxy defined inline
{lowest: false, highest: true }
```

```
>>> getRange 1.0 
{ lowest: -Infinity, highest: -Infinity }
```

> 注意:我们必须明确指定代理的类型，以表明代理中的`a`是什么类型。

代理只不过是传递类型信息的一种方式，它们广泛用于类型信息足以做出决定的地方，比如类型类实例选择。

还有其他类型的代理，如 SProxy(捕获符号)、RLProxy(用于行类型)等。但是这些都被否决了，所以知道`Proxy`就足够了😉