# 简化调度. main.async

> 原文：<https://medium.com/nerd-for-tech/simplifying-dispatch-main-async-ab9313b87293?source=collection_archive---------10----------------------->

有时我们需要将动作的结果从一个线程转移到另一个线程。

典型的情况是，当您在默认或后台队列上进行一些计算，并将更改传播到主队列(线程)时。

在不使用**反应式**方法或者**组合框架的情况下(**出现在 iOS 13，macOS 10.15)，我们需要将执行调度到另一个线程。

在许多情况下，我调用 **DispatchQueue** 的异步方法:

这样的回调导致“嵌套”。此外，我们不应该忘记使用弱或无主的自我，否则对象将被保留。不管怎样，上面的代码导致了某种重复:

```
DispatchQueue.main.async { [weak self] in             
    guard let self = self else { return } 
    // Inner code...}
```

哪个因为括号而让我不愉快:)。我们可以在 Xcode 中使用代码片段，让生活变得更简单。否则，可以在 UI 线程上创建一些协议(我称之为 **MainThreadRunnerType** )和智能调度方法( **runOnMain** ):

MainThreadRunnerType.runOnMain

```
So in such case "repetition" is going to look in the following way:runOnMain { [weak self] in
    self?.runSecond()
}runOnMain { [weak self] in
      guard let self = self else { return } self.runThird()
}
```

让我们给协议添加一个扩展，允许**弱**调用对象的方法。如果我没看错的话，我在 *RxCocoa* 看到了最初的想法:

在上述代码**中，自身**用于扩展，因此可以使用协议作为类型，并为对象创建弱引用。

让我们看一些例子:

还有“代码重复”模式，可以通过 Xcode 的代码片段解决。然而，它看起来更好:

```
runOnMainWeakly(method: type(of: self).displayEndMessage) runOnMainWeakly(method: type(of: self).displayMessage2(_:), 
                parameter: "Messsage 2nd")
```

在本文中，我们设法简化了 **Dispatch.main.async** 调用模式。