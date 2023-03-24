# profuctor-lens-探索-pure script-模块#4

> 原文：<https://medium.com/nerd-for-tech/profunctor-lens-exploring-purescript-modules-4-89aba69405ef?source=collection_archive---------7----------------------->

假设您有以下复杂的数据结构，

```
type NestedData =
  Maybe (Array 
     { foo :: Tuple String (Either Int Boolean)
     , bar :: String })
```

如果我告诉你翻转特定索引处的深度嵌套布尔值，你会怎么做？

```
flipBool :: Int -> NestedData -> NestedData
flipBool index = case _ of 
    Nothing -> Nothing 
    Just arr -> modifyAt index modifyFooFoo arr
    where
      modifyFooFoo rec = rec { foo = modifyTuple rec.foo}
      modifyTuple (Tuple first sec) = Tuple first (modifyEither sec)
      modifyEither ei = not <$> ei
```

我们需要写一个像上面这样的复杂函数吗？如果`NestedData`的结构发生变化会怎样？我们必须再次对我们的功能进行大量的修改。那么，有什么简单的方法来修改&检索这些复杂数据 structures🧐里面的数据呢？是的，有，它们叫做光学。有了光学，你可以像这样做

```
flipBool :: Int -> NestedData -> NestedData
flipBool index nestedData = over boolGetter not nestedData
    where
       boolGetter = (_Just <<< ix index <<< _foo <<< _2 <<< _Right)
```

看到代码行减少了 50%,即使我没有解释任何关于上面的语法，你可以很容易地通过查看上面的代码块图片由 [James Bold](https://unsplash.com/@jamesbold?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/lens?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上

以下内容主要来自于[托马斯·霍尼曼](https://thomashoneyman.com/articles/practical-profunctor-lenses-optics/)发表的这篇文章。

![](img/08465fb5e712f6990951eb242a9af2c3.png)

照片由[詹姆斯·博尔德](https://unsplash.com/@jamesbold?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/lens?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

# 光学器件的类型

在深入研究上述函数是如何工作的之前，我们先了解一下光学中的一些核心概念。

经常使用的光学系统有四种。分别是棱镜，镜头，遍历，Iso。每个光学元件描述一个结构`s`和一个零或者多个类型`a`值之间的关系。

```
Optic s a 
// s => Structure like Maybe a, Either a, Array a etc
// a => (one or more) Value (the `a` inside the above types)
```

`Prism`代表表示结构`s` 的光学元件，从该光学元件中我们可能得到也可能得不到`a`类型的值。例如，一个可能类型。

`Lens`表示光学，我们可以从结构`s`中获取类型`a` 的值。例如，一个元组

`Traversal`表示从结构`s`中获取零个或多个类型`a` 值的光学。例如，一个数组

`Iso`表示光学，其中结构`s`和值`a`彼此同构。例如，一个新类型

```
newtype Name = Name String
// here `Name` and `String` are isomorphic to each other
// one of them are enough to create the other
```

## 剖析示例

```
(_Just <<< ix index <<< _foo <<< _2 <<< _Right)
```

*   我们可以用光学做的一件强有力的事情就是把它们组合起来。
*   光学通常以下划线为前缀，需要从左到右阅读(当我们左组合它时)。
*   当我们向右移动时，我们将深入结构的一层。从上面的光学，我们可以得出结论

```
1st Layer - A Maybe
2nd Layer - Array like structure
3rd Layer - A Record
4th Layer - A tuple
5th Layer - An Either 
```

如果你记得那正是我们的`NestedData`，

```
type NestedData =
  Maybe                      -- 1 : _Just
   (Array                    -- 2 : ix index
     { foo ::                -- 3 : _foo
        Tuple String         -- 4 : _2
        (Either Int Boolean) -- 5 : _Right
     , bar :: String })
```

除了`_foo`也很容易定义外，上述大多数光学元件都在[深孔透镜](https://pursuit.purescript.org/packages/purescript-profunctor-lenses/8.0.0)模块中预定义。

```
import Data.Lens.Record(props) _foo = prop (Proxy :: Proxy "foo")
```

# 潜水深度

让我们逐一了解四种光学，

## 1.镜头

我们对元组和记录等产品类型使用镜头。lets 让我们获得并修改类型为`a`的值，当它肯定存在于结构`s`中时。示例镜头`_2`将获取元组的第二个元素，我们确信它总是出现在任何给定的元组中。

```
-- This lens focuses on the second element of a tuple and is implemented
-- in the Data.Lens.Lens.Tuple module.
_2 :: forall a b. Lens' (Tuple a b) b

-- This lens focuses on the "name" field of a record; we have to construct
-- this one ourselves.
_name :: forall a r. Lens' { name :: a | r } a
_name = prop (SProxy :: SProxy "name")
```

您可以使用类似于`view`的函数从结构`s`中获取值`a`，并使用类似于`set`和`over`的函数修改其中的值。

考虑到你有这种类型

```
type Person = {name :: String, age :: Int}
```

并且你已经创建了一个在球场上工作的镜头`name`

```
_name :: forall a r. Lens' { name :: a | r } a
_name = prop (SProxy :: SProxy "name")
```

如果你想得到`name`字段的值，你可以使用`view`

```
>>> view _name {name : "Saravanan", age : "22"}
```

如果您想覆盖该值，您可以使用`set`

```
setName :: Person -> Person
setName p = set _name "Saravanan M" p>>> setName {name : "Saravanan", age : "22"}
>>> {name : "Saravanan M", age : "22"}
```

如果你想覆盖先前的值，那么你可以使用`over`

```
setName' :: Person -> Person
setName' p = over _name (\name -> "Mr." <> name) p>>> setName' {name : "Saravanan", age : "22"}
>>> {name : "Mr.Saravanan", age : "22"}
```

## 2.棱镜

与透镜不同，透镜作用于产品类型，并且它所作用的结构`s`总是具有值`a`，棱镜作用于总和类型(如可能、任一等)，并且它所作用的结构`s`可能具有也可能不具有它所要求的值`a`。

让我们假设我们正在处理类型

```
type APIResponse = Either String Person
```

要获得左右值，您可以分别使用预定义的棱镜`Left`和`_Right`。

```
getErrorResponse :: APIResponse -> Maybe String
getErrorResponse  =  preview _Left getSuccessResponse :: APIResponse -> Maybe Person
getSuccessResponse = preview _Right>>> getErrorResponse (Left "Failed")
>>> Just "Failed">>> getSuccessResponse (Left "Failed")
>>> Nothing
```

> 注意:从**预览**返回的值是**可能是**不像`view`返回`a`

您也可以将`over`、`set`用于棱镜，就像用于镜头一样。

如果你想了解剩余的光学遍历，Iso，也想阅读更多关于这个主题的内容，我强烈建议你阅读托马斯·霍尼曼的[这个精彩的博客](https://thomashoneyman.com/articles/practical-profunctor-lenses-optics/)。

我希望你喜欢这个博客，拍手👏，分享👥如果你觉得这有帮助。我将在下一篇文章中看到另一个令人敬畏的 purescript 模块。