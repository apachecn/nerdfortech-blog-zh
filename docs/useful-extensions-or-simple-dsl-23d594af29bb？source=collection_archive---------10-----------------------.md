# 有用的扩展或简单的 DSL

> 原文：<https://medium.com/nerd-for-tech/useful-extensions-or-simple-dsl-23d594af29bb?source=collection_archive---------10----------------------->

在这篇文章中，我们将考虑一些有用的扩展，允许创建某种 DSL…

在 C#中从开始。NET 2.0 有一些漂亮的构造。其中之一，我可以说字符串上的静态方法，即:[字符串。IsNullOrEmpty](https://docs.microsoft.com/en-us/dotnet/api/system.string.isnullorempty?view=net-5.0) 。对不起[弦。Empty](https://docs.microsoft.com/en-us/dotnet/api/system.string.empty?view=net-5.0) 返回空常量字符串:

哇，以前我不得不声明 empty 为计算属性，很难相信 empty 会被编译一次。显然，String 是一种结构，在 foundational framework 中声明，可以在库外扩展，所以我假设它是一个模块级的“常量”。

让我们考虑一些可选类型的有用扩展:

因此，我们可以使用一种新的方式，而不是与 nil 进行比较，或者使用合并运算符:

```
var item: Any? 
let oldWay1 = item != nil
let newWay1 = item.hasValue // new way instead of !=
assert(oldWay1 == newWay1)var str: String?let str1 = str ?? "item not a String"
let str2 = str.valueOrDefault("item not a String") //str.valueOrEmpty == str ?? ""
assert(str1 == str2)The issue with Swift 5.5 compiler is that long complex impressions might need brackets.For instance **items?.first?.description?.valueOrEmpty** - isn't compiled. Placing inside brackets makes code compilable.let description = items?.first?.description
description.valueOrEmpty - is OK.
(**items?.first?.description).valueOrEmpty -** is also fine.
```

在允许避免打字错误的语言中出现**关键路径**:

```
NSExpression(format: "flag")
NSExpression(forKeyPath: \CheckItem.flag) // better than 1st variant
```

但是并不是所有情况下都支持 key path，例如在**n predicate**构造函数方法中。但至少我们可以检查聊天键路径是否可用，即使不使用它:

```
NSExpression(format: "country.address.firstLine == %@", "Line")
_ = \Destination.country.address.firstLine // will be checked by compiler & raise compile error
```

顺便说一下，一些简化可以使用可选的 **map & flatMap** 方法存档:

Optional.flatMap 的用法