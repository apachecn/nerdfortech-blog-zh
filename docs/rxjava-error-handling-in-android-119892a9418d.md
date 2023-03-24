# Android 中的 RxJava 错误处理

> 原文：<https://medium.com/nerd-for-tech/rxjava-error-handling-in-android-119892a9418d?source=collection_archive---------9----------------------->

![](img/f20b2bd89988c697dd020449cc1ea7f5.png)

来源:https://www.emojipng.com/preview/12037564

错误处理是构建健壮的 android 应用程序的基本要求之一。在本文中，我们将介绍一些错误处理案例，这些案例将通过使用 RxJava 来改进我们的代码。RxJava 库已经成为 android 开发中编写异步事件流的主流库。在 android 中，我们在后台线程中广泛使用 RxJava 对后端服务进行 API 调用，因此由于 RxJava，可以避免长时间等待 API 调用而阻塞主线程。RxJava 还通过使用`flatmap`、`map`和更多操作符的流链接能力来帮助避免回调地狱。

这里的第一个用户故事开始，用户可以在应用程序中获得他/她自己的个人资料数据，一旦从 API 获取数据，数据应该保存在应用程序的本地存储。在技术实现中，我们需要在调用 API 获取用户数据并最终保存之前获取用户 ID。下面是实现的例子:

通过运行上面的代码，它将产生以下输出:

这里有一个有趣的例子，如果我们想在调用下一个 API 调用之前验证用户 ID 不为空，该怎么办。如果用户 ID 不为空，那么我们可以继续下一个事件来获取用户数据，否则我们需要显示错误消息，指出用户 ID 无效。在第一次尝试中，我们可能会尝试通过使用下面的方法创建带有错误标志的响应的默认值，将错误标志从一个操作符传播到下一个操作符。

这是第一种方法的输出。

使用这种方法最终是可行的，但这是一种非常痛苦的方法，因为我们需要创建默认的错误对象，并总是检查响应中的错误标志，以便继续流，直到调用下一个订户方法。它还滥用 onNext 的用法，因为 onNext 应该被视为在事件流中执行的成功回调。

现在，我们知道在事件流中传递错误标志不是一个好的做法，而且在我看来会产生许多重复的不必要的验证，所以我们应该尝试不同的方法，以便在流中发生错误时立即结束流。为了立即结束一系列事件，我们应该抛出一个异常。等等……我们应该意识到，在 android 中，运行时抛出任何异常都会让应用崩溃。让应用崩溃绝对不是我们想要正确处理这个错误的方式。

等等…让我们确信 RxJava 能够很好地处理流中间抛出的异常事件。它将封装异常，然后结束流，最后在订阅中执行一个错误回调，而不会使应用程序崩溃。下面是一个例子:

以下是输出结果:

通过使用这种方法，当错误发生时流立即停止，并在 onError 回调中正确处理错误，同时 onNext 以正确的行为实现。

现在我们已经了解到抛出流中的异常被很好地封装，并且在异常发生时调用 onError 回调。让我们继续检查另一件有趣的事情。如果发生了异常并且从未声明过 onError，会发生什么？

输出:

`OnErrorNotImplementedException`从 RxJava 抛出，会让 app 崩溃。由于 RxJava 强制要求任何异常都发生在流中，因此应该在订阅端处理。除了在订阅端实现 onError 之外的其他实现使用了`onErrorReturnItem`操作符。`onErrorReturnItem`操作符帮助我们在传递流之前，当流中发生任何异常时，构造默认值。例如:

输出:

因此，通过使用`onErrorReturnItem`操作符，我们可以不在订阅端实现 onError 回调，并避免`OnErrorNotImplementedException`的发生，因为 onError 永远不会被调用，因为在任何错误发生时总是提供回退默认值，因此流将在下一个回调中结束。

在最后一个用例中，我们将尝试在流中抛出异常的不同情况，因为我们意识到 RxJava 强制要求 onError 应该在事件流的任何用例中实现。我们将设置一个用例，在调用 onComplete 后立即抛出异常，这意味着应该结束流，我们将看到会发生什么。

通过执行上面的代码，RxJava 再次抛出异常！哪个不是`OnErrorNotImplementedException`而是`UndeliverableException`

原来 RxJava 中的异常即使在可观察对象被处置/终止后也没有被吞掉，所以 RxJava 会把这个异常包装起来继续抛出，最终让 app 崩溃。文件里已经说明了。

> 不幸的是，RxJava 无法判断这些超出生命周期的、不可交付的异常中哪些应该或不应该使您的应用崩溃。识别这些异常的来源和原因可能是令人厌烦的，尤其是如果它们源自某个来源并被路由到链中较低的某个地方。
> 
> 因此，2.0.6 引入了特定的异常包装器来帮助区分和跟踪错误发生时发生了什么:
> 
> `UndeliverableException`:将由于生命周期限制而无法交付的原始异常包装在`Subscriber` / `Observer`上。

对此唯一的解决方案是设置一个全局 RxJava 错误处理程序，它最终将捕获这些类型的异常并相应地进行处理。

```
RxJavaPlugins.setErrorHandler(exception -> logger.log(exception));
```

关于使用 RxJava 库在流中处理异常的讨论到此结束。谢谢你。

参考资料:

[](https://github.com/ReactiveX/RxJava/wiki/What%27s-different-in-2.0#error-handling) [## react vex/rx Java

### RxJava 2.0 已经在 Reactive-Streams 规范的基础上完全重写。规格…

github.com](https://github.com/ReactiveX/RxJava/wiki/What%27s-different-in-2.0#error-handling) [](https://proandroiddev.com/rxjava2-undeliverableexception-f01d19d18048) [## RxJava2 无法交付异常

### 我想分享我以前没有注意到的不可交付异常的经历。

proandroiddev.com](https://proandroiddev.com/rxjava2-undeliverableexception-f01d19d18048) 

源代码

[](https://github.com/WendyYanto/Rxjava-error-handling/blob/master/app/src/test/java/dev/wendyyanto/rxjavaerrorhandling/RxJavaErrorUnitTest.kt) [## Wendy yanto/rx Java-错误处理

### 在 GitHub 上创建一个帐户，为 Wendy yanto/rx Java-错误处理开发做出贡献。

github.com](https://github.com/WendyYanto/Rxjava-error-handling/blob/master/app/src/test/java/dev/wendyyanto/rxjavaerrorhandling/RxJavaErrorUnitTest.kt) [](https://github.com/WendyYanto/Rxjava-error-handling/blob/master/app/src/test/java/dev/wendyyanto/rxjavaerrorhandling/RxJavaUnitTest.kt) [## Wendy yanto/rx Java-错误处理

### 在 GitHub 上创建一个帐户，为 Wendy yanto/rx Java-错误处理开发做出贡献。

github.com](https://github.com/WendyYanto/Rxjava-error-handling/blob/master/app/src/test/java/dev/wendyyanto/rxjavaerrorhandling/RxJavaUnitTest.kt)