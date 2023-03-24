# EffectFnAff —让我们在 purescript 中创建异步外部函数—探索 purescript 模块#3

> 原文：<https://medium.com/nerd-for-tech/effectfnaff-lets-create-async-foreign-functions-in-purescript-exploring-purescript-modules-3-b0e168547851?source=collection_archive---------5----------------------->

这是我在***#探索 ps 模块系列中的第三篇文章。在这篇文章中，我们将探索一个非常酷的 purescript 模块。***

让我们接受这一点，我们不能单独用 purescript 做所有的事情，有时我们需要 javascript 的帮助，为此我们使用 FFI(外部函数接口),它支持 PureScript 代码与 JavaScript 代码之间的通信，反之亦然。

![](img/6a291adcb717c3f58a14311b75ff92c0.png)

[Adrien céSARD](https://unsplash.com/@adriencesard?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/bridge?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

假设您想要实现`Euclidean algorithm`来寻找两个数字的 GCD，但是您不想用 purescript 编写它，所以您用 JS 创建了这个函数。

```
//example.jsexports["gcd"] = function (a){
  return function(b){
        //..... 
        return result;
    }
}
```

然后我们从 purescript 端对外导入

```
//example.purs
foreign import gcd :: Int -> Int -> Int 
```

这很简单。但是你试过国外导入异步函数吗？

```
foreign import myAsyncGCDFunction :: Int -> Int -> Aff Int
```

我们可以使用`Aff`作为国外进口的退货类型吗？

不，我们不能这么做。为什么？因为 Aff 是一个复杂的 purescript 数据结构，它不仅仅是一个我们可以从 JS 返回的普通函数。

对于这类用例，我们有一个叫做`EffectFnAff`的特殊类型，它可以让我们为异步 javascript 函数创建一个 purescript 接口。

出于演示目的，我们将创建一个异步函数`addWithDelay`，它将接受两个整数并在 1 秒钟后返回结果。

```
function adddWithDelay(a,b) {
   return new Promise((resolve, reject) => {
           setTimeout(() => resolve(a+b) , 1000);
          });
}
```

> **注意:**我没有导入这个函数

如何将这个转换成与 purescript 兼容的外来函数？

好了，我们来一点一点的解剖一下上面的函数。

**第 4 行:** `EffectFnAff`会给我们两个回调`onError` & `onSuccess.`一个可以用错误调用，另一个可以用我们函数要返回的实际值调用。

**第 8 行:**我们打电话给`onSuccess`，承诺的结果是`r`

**第 10 行:** EffectFnAff 需要返回一个取消器，它是一个带有 3 个参数的函数，第一个参数是负责触发取消器的`error`，第二个参数是当我们不能取消操作时将被调用的函数，第三个参数是当我们完成取消异步操作(如清除资源、关闭数据库连接等)时必须调用的函数。

在上面的例子中，我们没有处理任何取消错误，而是直接调用`onCancelerSuccess()`来告知我们已经成功取消了操作。

既然我们已经理解了如何从 JS 端编写`EffectFnAff`，让我们从 purescript 端创建一个接口，这样我们就可以利用它了。

```
foreign import addWithDelay_ :: Int -> Int -> EffectFnAff Int
```

有了外来的导入函数，我们不得不写另一个 purescript 接口，可以将这个“无用的”`EffectFnAff`转换成`Aff.`如果你在 [pursuit](https://pursuit.purescript.org/packages/purescript-aff/7.1.0/docs/Effect.Aff.Compat#v:fromEffectFnAff) 上搜索一个转换`EffectFnAff -> Aff`的函数，那么你会找到一个名为`fromEffectFnAff`的函数

```
addWithDelay :: Int -> Int -> Aff Int
addWithDelay a b = fromEffectFnAff $ addWithDelay_ a b
```

是的，我的朋友，这就是把你漂亮的异步函数转换成一个你可以从 purescript 端使用的`Aff`的全部工作。

## **奖金**

如果你有兴趣看看`EffectFnAff`的取消器是如何工作的，只需杀死会自动调用取消器的`Aff`。

希望你喜欢这篇文章，并了解了我们如何在 async Javascript 和 Purescript 之间架起一座桥梁。

拍手声👏如果你喜欢这篇文章，分享给你的朋友来传播知识。让我们让 Purescript 流行起来💪。