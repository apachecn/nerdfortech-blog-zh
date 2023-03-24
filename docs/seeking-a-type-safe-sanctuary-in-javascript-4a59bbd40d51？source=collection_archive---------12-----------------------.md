# 在 JavaScript 中寻找类型安全避难所

> 原文：<https://medium.com/nerd-for-tech/seeking-a-type-safe-sanctuary-in-javascript-4a59bbd40d51?source=collection_archive---------12----------------------->

越来越多的强类型脚本语言将文件转换成 JavaScript，其中 Typescript 处于领先地位。在编译阶段捕捉类型问题的诱惑有时非常强烈，以至于我们需要一个更加类型安全的工具。但是我们可以通过另一种技术来获得类型安全(或确定性)——保护您的函数。围绕保护你的功能的概念有一些教程，很多年前我发现了 Mike Stay 的博客(最终进入范畴理论)。

熟悉不断发展的 JavaScript 库生态系统是一项艰巨的任务。 [Sanctuary](https://sanctuary.js.org/) 是 2015 年推出的一个库(仍在积极开发中)，它结合了许多来自 [Ramda](http://ramdajs.com/) 、 [Fantasy Land](https://github.com/fantasyland/fantasy-land) 兼容类型和类型安全的“功能性”助手。

在我们做任何事情之前，我们需要包括各种库位:

```
const { cond, equals, T } = require('ramda');
const { pipe, Left, Right, EitherType } = require('sanctuary');
const $ = require('sanctuary-def');
const def = $.create({ checkTypes: true, env: $.env });
```

# 保护功能划分的类型

这个代码片段中有趣的是 **def** 函数。当我们用它来定义一个函数时，它将定义这个函数并设置类型保护。有趣的是第三和第四个参数。第三个是类型数组，数组中的最后一项是函数的返回类型。第四是执行。我们可以利用这一点来定义鸿沟:

```
const divideImpl =  (n, d) => cond([
    [equals(0), (_) => { throw new Error("division by zero" }) ],
    [T,         (d) => n/d ]
])(d)//    divide =  Number -> Number -> Number
const divide =  def(
   'divide', 
   {}, 
   [$.Number, $.Number, $.Number],
   divideImpl
)
```

Divide 接受一个分子和一个分母，用零来计算分母，它将抛出一个异常，否则函数返回商(令人震惊——对吗？).

```
const x = divide(10)(2) *//=> 5*
﻿const x = divide(10)(0) *//=> Error: divsion by zero*
```

如果我们更改 divide 的类型签名以返回一个字符串并调用它，我们可以捕获一个详细的错误消息。

```
//    divide =  Number -> Number -> String
const divide =  def(
   'divide', 
   {}, 
   [$.Number, $.Number, **$.String**],
   divideImpl
)divide(10)(2) 

*/*
TypeError: Invalid value

divide :: Number -> Number -> String
                              ^^^^^^
                                1

1)  5 :: Number

The value at position 1 is not a member of ‘String’.

*/*
```

在上面的例子中，TypeError 告诉我们函数试图返回数值 5，但是函数签名需要一个字符串。让我们将返回类型还原为 Number，并考虑抛出异常的危险。

# 处理异常

异常的主要问题是，如果不小心处理，它们经常会带来流控制和展开问题。他们引入了退出函数的第二种方式，这违反了我们在 101 课程中所学的大部分内容。在修改后的方法中，二进制类型要么非常适合捕获方法的输出或异常，允许函数的单点进入和退出。我们可以创建一个更高阶的函数来调用一个函数并代表我们捕获异常，而不是将它编码到函数本身中——类似于

```
const a = $.TypeVariable('a');
const l = $.TypeVariable('l');
const r = $.TypeVariable('r');const fromThrowableImpl = (f, a) => {
        try{
            return Right(f.call({}, a))
        }
        catch(l) {
            return Left(l)
        }
    }*//    fromThrowable :: ( a -> r) -> a -> Either l r*
const fromThrowable =  def(
    'fromThrowable', 
    {}, 
    [$.Function([a, r]), a, EitherType(l, r)],
    fromThrowableImpl
);
```

常数 a、l 和 r 表示在整个类型定义过程中保持不变的泛型类型。 **fromThrowable** 试图调用一个函数，捕捉任何异常并返回。Right(通常在 Success 中使用的实例，封装了返回值)或。左(通常在封装了错误代码的故障中使用的任一实例)。fromThrowable 本身有两个参数。第一个是接受一个类型并返回另一个类型的函数。第二个是满足该函数输入的变量。

```
fromThrowable(divide(10), 2) 
//=> Right(5)fromThrowable(divide(10), 0) 
//=> Left(new Error("Division by zero"))
```

幸运的是，通过提供**套装**，Sanctuary 为我们省去了定义 throubles 的麻烦。需要注意的是，装箱要么需要一个额外的函数来转换任何错误。

```
encaseEither((x)=>x, divide(10), 0) 
//=> Left(new Error("division by zero"))
```

# 更好的守卫

这很好，可以应用于任何抛出异常的函数。当利用遗留 JavaScript 库时，这是一个很好的选择。在这个项目中，我们可以通过使用更好的类型定义来提供更多的保护，通过在避难所环境中引入新的类型，

```
**const isIntegerNeqZero = x => $.test([], $.Integer, x) && x != 0**const NonZeroInteger = $.NullaryType(
    'my-test/NonZeroInteger',
    'http://engieering.somewhere.com/my-test#NonZeroInteger',
    **isIntegerNeqZero**
);
```

NonZeroInteger 确保任何值都是整数并且不等于零。从这里我们可以重新定义 divide，这将防止零被传入。

```
const divide =  def(
    'divide', 
    {}, 
    [$.Number, NonZeroInteger, $.Number],
    divideImpl
)

divide(10)(0)

*/*
TypeError: Invalid value

divide :: Number -> NonZeroInteger -> Number
                    ^^^^^^^^^^^^^^
                          1

1)  0 :: Number

The value at position 1 is not a member of ‘NonZeroInteger’.
*/*
```

这是一件美好的事情。

如果你对探索函数式编程感兴趣，请联系我们。跟我一起编码。

[](https://www.linkedin.com/in/tb02118/) [## Todd Brown-Liberty Mutual 创新和敏捷工程副总裁兼高级总监…

### Todd 在软件行业有 20 多年的经验，专注于架构、安全和…

www.linkedin.com](https://www.linkedin.com/in/tb02118/) 

【https://www.linkedin.com】最初发表于[](https://www.linkedin.com/pulse/seeking-type-safe-sancutary-javascript-todd-brown/)**。**