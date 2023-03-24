# 让我们玩玩异构数据类型——pure script-异构——探索 pure script-模块#2

> 原文：<https://medium.com/nerd-for-tech/lets-play-with-heterogeneous-data-types-purescript-heterogeneous-847391430d00?source=collection_archive---------11----------------------->

这是本系列的第二篇文章。在本文中，我们将研究专门为异构数据结构的映射和折叠开发的`purescript-heterogeneous`包。好的，我这里说的异构数据结构是什么意思？

所有一次可以保存多个值的类型被称为异构数据结构，或类型。例如，元组可以一次保存多个值，一个左值和一个右值。因此它们被称为异构数据类型。但是数组并不是异构的，因为所有的元素只能是一种类型。

![](img/679f43310a73a2a245412e959df14a20.png)

由[亚历山大·格雷](https://unsplash.com/@sharonmccutcheon?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/different-colors?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

我们在 purescript 中处理的最常见的异构类型是 Record。我一直想知道我们如何才能映射记录，然后我发现了这个奇妙的包`[purescript-heterogeneous](https://pursuit.purescript.org/packages/purescript-heterogeneous/0.3.0)`

首先，假设记录中的所有字段都是同类的(相同类型)

```
type Names = 
  {  father     :: String
  ,  mother     :: String
  ,  son        :: String
  ,  daughter   :: String
  }
```

你想让每个字段都大写，你能用普通的`map`做到吗？当然没有，为什么？因为 record 不是[函子](https://pursuit.purescript.org/packages/purescript-prelude/6.0.1/docs/Data.Functor#t:Functor)。

那么我们如何做到这一点呢？

```
capitalizeNames :: Names -> Names 
capitalizeNames names = hmap toUpper names
```

仔细看，我没用过`map`我用的是`hmap`，是套装提供的`purescript-heterogeneous.`

我遇到过这样一种情况，我有一个记录，我只想给记录中的每个值加上`Just`。我想知道我怎么能做到呢？我们能做类似上面的事情吗？

```
justRecords names = hmap Just names -- will throw error
```

不，我们不能，为什么？因为每个字段都可能是不同的类型，那么`Just`的类型会是什么呢？`String -> Maybe String`或`Int -> Maybe Int`等……理想情况下应该是`a -> Maybe a`

为了封装`a`,我们需要创建一个类型，(不要担心它非常简单)

首先创建一个类型，

```
data MakeMaybe = MakeMaybe
```

然后为异构包中定义的`Mapping`类派生一个实例。

```
instance makeMaybeMapping :: Mapping MakeMaybe a (Maybe a) 
 where
     mapping MakeMaybe = Just
```

映射类型类有 3 个参数，第一个是你创建的单态类型，第二个和第三个是映射函数`(a -> b).`中的`a`和`b`，在我们的例子中是`a -> Maybe a`

在上面的代码中，我告诉使用`Just`将类型`a`转换为`Maybe a`，你也可以用`Nothing`代替`Just`，这将把每个字段转换为`Nothing`(但是谁会想要这样的函数呢？😑)

使用它也很容易，你只需要使用你已经创建的类型。

```
makeMaybe names = hmap MakeMaybe names
```

makeMaybe 的类型签名将类似于，

```
type MaybeNames = {  father     :: Maybe String
                  ,  mother     :: Maybe String
                  ,  son        :: Maybe String
                  ,  daughter   :: Maybe String
                  }makeMaybe :: Names -> MaybeNames 
makeMaybe = ....
```

> 注意:在上面的例子中，字段可以是任何类型，直到它们用 Maybe 包装起来。

请看，`hmap`已经将我们的记录类型转变为全新的类型。我们可以用`hmap`做很多事情，例如，如果你想把所有的字段类型转换成字符串，你可以这样做

```
--- create a type
data MakeString = MakeString--- create the Mapping instance 
instance makeMakeString :: (Show a) => Mapping MakeString a String
  where
     mapping MakeString = show--- create a method that does the mapping
makeString names = hmap MakeString names
```

太容易了，不是吗？

但是如果不能像上面那样统一类型(通过使用类似`Show`的约束说所有字段都有 show instance 或者盲目使用类似`forall a`的全称量词)怎么办？

如果您希望每个字段都有单独的映射函数，该怎么办？我们能用这个包做到吗？是的，我的朋友！！

但是你在创建实例的时候已经写了一些模板，但是请相信我，它非常简单&你会很快习惯的。

首先，像往常一样，让我们创建一个包装记录的数据类型(在本例中是函数)。

```
data ZipProps fns = ZipProps { | fns}
```

之前我们创建了`Mapping` typeclass 实例，但是在这种情况下我们将使用`MappingWithIndex`，它通过`Proxy`为我们提供字段名

```
instance zipProps ::
  (IsSymbol sym, Row.Cons sym (a -> b) tail fns) =>
  MappingWithIndex (ZipProps fns) (Proxy sym) a b where
  mappingWithIndex (ZipProps fns) prop = Record.get prop fns
```

好吧，这可能会令人困惑，让我来分析一下

MapWithIndex 采用 4 个参数

1.  `ZipProps fns` —您已经创建的数据类型，在我们的例子中是它的 ZipProps，它包装了函数的记录。
2.  `Proxy sym` —这给出了字段的名称。
3.  `a` —字段的类型
4.  `b` —用函数转换后的字段类型。

```
IsSymbol sym, Row.Cons sym (a -> b) tail fns
```

现在让我们看看约束条件，

1.  `IsSymbol sym` —表示代理中的`sym`是一个符号(S [符号](https://pursuit.purescript.org/packages/purescript-prelude/6.0.1/docs/Data.Symbol)只是类型级别的字符串，您可以使用`reflectSymbol`将符号转换成字符串)
2.  `Row.Cons sym (a -> b) tail fns` —该约束告诉字段名`sym`具有类型`a->b`(从 a 到 b 的函数)以及剩余的行`tail`将构成整个记录`fns`。它翻译成类似下面的块

```
fns = { sym :: (a -> b)
      , ... tail 
      }
```

现在让我们看看这个定义是什么意思，

```
mappingWithIndex (ZipProps fns) prop = Record.get prop fns
```

`Record.get`接受两个参数，一个以字段名作为符号的代理和实际记录，并返回值。就像访问

```
fns.prop -- where fns is an object and prop is the field name
```

上面的行将返回一个从`a -> b`开始的函数，其中同名的对应字段的类型是`a`。例如，如果`father`字段属于`String`类型，那么 fns 中的`father`字段是来自`String -> sometype`的函数(它必须采用与参数相同的类型)

现在让我们创建一个压缩函数，它获取函数的记录并将它们应用到具有相同字段名的元素的记录中。

```
--- the zipping function
zipRecord fns record = hmapWithIndex (ZipProps fns) record--- the mapping function 
fns =  { aNumber :  \num  -> num + 1
       , aString. : \str  -> str <> "!!"
       , aBool    : \bool -> not bool 
       }--- the record to be transformed
num = { aNumber : 2 
      , aString:"Hello"
      , aBool : false }>>> zipRecord fns num
{ aNumber: 3, aString: "Hello!!", aBool: true }
```

> 注意:字段名应该相互匹配

异构包还提供了一种折叠记录的方法，但是我们不打算在本文中讨论它，那将是另外一个话题。

我希望你学到了一些很酷的东西，并且知道有多酷😎异构包是，这可以使工作与记录超级灵活和容易。

将在下一篇文章中看到其他一些很酷的包。拍手声👏如果你喜欢这篇文章，如果你觉得这篇文章有帮助，就分享给你的朋友。