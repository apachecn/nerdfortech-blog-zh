# DefaultValues-凭空创建默认值-Exploring-pure script-modules # 4

> 原文：<https://medium.com/nerd-for-tech/defaultvalues-create-default-values-out-of-thin-air-exploring-purescript-modules-4-a80f1210d06b?source=collection_archive---------7----------------------->

您是否曾经想要创建占位符值，比如一个大记录，因此您创建了一个保存虚拟值的变量，并将其用于`fromMaybe`或解码失败等？

例如，让我们假设您有一个记录，其中包含一个人的信息，

```
type Person = 
      { name :: String
      , age :: Int 
      , height :: Number 
      , ....
      }
```

假设您想为您的表单组件创建一个初始状态，上面的所有细节都将被填充，那么初始状态会是什么呢？

```
initialState :: Person
initialState = 
      { name : ""
      , age : 0
      , height : 0.0
      , ....
      }
```

> 注意:理想情况下，它们应该用 Maybe 类型包装，但出于解释目的，请原谅我。

我们可以创建一个包含大约 5-10 个字段的虚拟/占位符值的记录，但是如果字段的数量超过 10 或 20 或更多呢？我们必须编写这些样板代码来创建虚拟值吗？如果某个包只给我们用默认值填充的值不是更好吗？这难道不会节省开发人员大量时间吗？

有没有什么包可以做到这一点？是的，有一个名为[**pure script-default-values**](https://pursuit.purescript.org/packages/purescript-default-values/1.0.1)的包，它就是用来创建默认值的。我们将在本文中探讨如何使用这个包。

![](img/8a25fd26e571b1aa882dd04584f73a0d.png)

[约根·哈兰](https://unsplash.com/@jhaland?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/twins?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

这是一个非常简单的模块，它有一个名为`DefaultValue`的类型类，该类型类有一个名为`defaultValue`的方法，调用这个特定的方法将为我们提供该特定类型的默认值。

```
myDefaultInt :: Int
myDefaultInt = defaultValue myDefaultString :: String
myDefaultString = defaultValue>>> myDefaultInt
>>> 0>>> myDefaultString
>>> ""
```

现在让我们假设您有一个`Maybe Int`值，并且您正在使用 fromMaybe 从 Maybe 包装器中获取实际的整数。最有可能的是，我们会给 0 作为万一什么都没有发生时的后备，而不是硬编码`0`我们可以这样做，

```
>>> fromMaybe defaultValue (Just 100)
>>> 100>>> fromMaybe defaultValue (Nothing :: Maybe Int)
>>> 0
```

*注意:如果使用的是 repl，则需要类型注释* `*Nothing*` *，否则编译器无法推断类型。*

当涉及到 Int、String、Number 等基本类型时，提供默认值并不麻烦。但是这个包在处理大量嵌套的记录时非常出色。

```
type PersonInfo = 
      { name :: String
      , age :: Int 
      , height :: Number
      , father :: Person 
      , mother :: Person
      , degrees :: Array String
      , address : 
            { city :: String
            , pincode :: Int
            , street : 
                 { line1 :: String  
                 , line2 :: String
                 , ....
                 }
            }
      , ..... 
      }
```

如果你试图将一个`foreign`解码成上述类型，你很可能会使用`[decode](https://pursuit.purescript.org/packages/purescript-foreign-generic/10.0.0/docs/Foreign.Generic.Class#v:decode)`。如果您无论如何都要返回上述类型的值，那么在失败的情况下，您是否必须创建一个所有字段都用空值填充的虚拟记录？绝对不用，你可以直接用`defaultValue`

```
decodePerson :: Foreign -> PersonInfo
decodePerson fgn = case decode fgn of 
               Right person -> person 
               Left _ -> defaultValue
```

当解码失败时，`defaultValue`将为您提供所有字段都填充有虚拟值的记录。

但是，如果您想为自定义创建的类型提供默认值，该怎么办呢？

```
data Color = RED 
           | YELLOW
           | GREEN
```

要在这个类型上使用`defaultValue`，您只需为这个特定类型创建一个`DefaultValue`实例。

```
instance defaultColor :: DefaultValue Color where 
    defaultValue = GREEN>>> defaultValue :: Color
>>> GREEN
```

**奖励提示**

有些情况下，我们希望为已经在此模块中定义的类型提供不同的默认值。

例如，我们希望我们的`age`字段的默认值为 18，因为它是一个 Int defaultValue，将产生`0`。因此，我们可以将 Int 封装在 newtype 中，并为它提供我们自己的 DefaultValue 实现。

```
newtype Age = Age Int
instance defaultAge :: DefaultValue Age where 
    defaultValue = 18>>> defaultValue :: Age
>>> 18
```

希望你喜欢这篇文章，并发现这是有帮助的，让我知道它鼓掌👏还是通过评论。