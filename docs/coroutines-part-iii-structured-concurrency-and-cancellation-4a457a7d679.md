# 协同程序(第三部分)——结构化并发和取消

> 原文：<https://medium.com/nerd-for-tech/coroutines-part-iii-structured-concurrency-and-cancellation-4a457a7d679?source=collection_archive---------7----------------------->

![](img/7ac1391a06200c2c812fd2b2345ba4b5.png)

这是关于协程的 3 篇系列文章的第 3 篇，也是最后一篇。如果你没有读过前两篇文章，我强烈建议你读一读:

[](https://victorbrandalise.com/coroutines-part-i-grasping-the-fundamentals/) [## 协程(第一部分)-掌握基本原则

### 你读了 20 篇文章，看了 6 个视频，问了你的大学，不知何故你还是不明白…

victorbrandalise.com](https://victorbrandalise.com/coroutines-part-i-grasping-the-fundamentals/) [](https://victorbrandalise.com/coroutines-part-ii-job-supervisorjob-launch-and-async/) [## 协同程序(第二部分)-作业、管理作业、启动和异步

### 这是关于协程的 3 部分系列的第 2 篇文章。如果你没有看过第一篇文章，我强烈建议…

victorbrandalise.com](https://victorbrandalise.com/coroutines-part-ii-job-supervisorjob-launch-and-async/) 

# 处理取消

通过抛出一个`CancellationException`协同取消协程。捕获像`Throwable`这样的顶级异常的异常处理程序将会捕获这个异常。如果您在异常处理程序中吞掉异常或者从不挂起，那么协程将处于半取消状态。

如果你在 try catch 块上捕捉到`Exception`，确保你再次抛出`CancellationException`。这样，调用代码也会得到取消通知。

# CoroutineExceptionHandler

另一种处理异常的方法是在`CoroutineContext`上附加一个异常处理器。

`CoroutineExceptionHandler`是全球“一网打尽”行为的最后手段。您无法从`CoroutineExceptionHandler`中的异常中恢复。

所有子协程将异常处理委托给它们的父协程，父协程也委托给父协程，依此类推，直到根协程，**所以安装在它们上下文中的** `**CoroutineExceptionHandler**` **永远不会被使用**。

`CoroutineExceptionHandler`仅在未捕获的异常时调用。

`CoroutineExceptionHandler`对`async`没有影响。使用 async 创建的协同例程总是捕获所有异常，并将它们存储在生成的延迟对象中，因此它不会导致未捕获的异常。

当一个协同例程的多个子例程因异常而失败时，第一个异常得到处理。

# “我取消了协程，但它没有停止”

调用`cancel`并不能保证协程操作将被终止。如果您正在运行一个有点大的操作，比如从许多文件中读取，没有什么会立即停止您的代码执行。

所有`kotlinx.coroutine`暂停功能均可取消。所以，如果你正在使用他们，你不需要检查取消。但是，如果您不使用它们，您有两种方法可以使您的协程代码协同工作:

*   检查`job.isActive`或`ensureActive()`。根据文档:“*确保当前范围有效。如果作业不再处于活动状态，将引发 CancellationException。*

*   使用`yield()`让其他工作运行。根据文档:“*如果可能的话，将当前协程调度程序的线程(或线程池)让给同一调度程序上的其他协程来运行。*

你可能认为你不应该在你的代码中写`while(true)`，但是当你使用协程时，这是很常见的。看看下面的代码，只有当调用的协程没有被取消时，循环才会永远运行，这可能是有意的行为。但是，如果您取消调用的协同程序，这个循环将会停止，因为`delay`会检查取消情况，如果协同程序被取消，就会抛出`CancellationException`。

# 取消过程

我们知道协程是通过抛出`CancellationException`来取消的，但是取消过程是如何发生的呢？

如果你使用的是`Job`，协程:

1.  取消它的其余子级
2.  取消自己
3.  将异常向上传播到其父级

如果您使用的是`SupervisorJob`，那么协程:

如果异常没有被处理并且`CoroutineContext`没有`CoroutineExceptionHandler`，它将到达默认线程的`ExceptionHandler`。

# 处理取消的副作用

假设当一个协程被取消时，你需要做某个动作，比如关闭你可能正在使用的任何资源，记录取消，或者运行一些清理代码。有几种方法可以做到这一点:

*   检查！isActive ( [协同范围. isActive](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines/is-active.html)
*   try catch finally
    ——如果我们需要执行的清理任务正在挂起，上面的代码将不再起作用，因为协程在取消状态时不能挂起。
    -为了能够在协程被取消时运行暂停功能，我们必须将清理工作改为在`NonCancellable`协程上下文中运行。

`[suspendCancellableCoroutine](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines/suspend-cancellable-coroutine.html)`和调用取消

# 小心不可取消的

`NonCancellable`创建一个永不取消且始终处于活动状态的作业。它旨在与`withContext`函数一起使用，以防止取消那些必须在没有取消的情况下执行的代码块。

这个对象不打算和协程构建器一起使用，比如`launch`、`async`等等。写`launch(NonCancellable){ ... }`的话，不仅刚启动的作业不会因为取消父作业而终止，反而会破坏掉整个父子之间的父子关系。父进程不会等待子进程完成，也不会在子进程崩溃时被取消。

![](img/6ce90e22c4281f977f7cf9690dbd4997.png)

# 结构化并发

您将在现实世界的应用程序中启动许多协程。结构化并发保证它们不会丢失或泄露。它是一个语言特性和最佳实践的集合，当结合起来时，允许您跟踪在协程中完成的所有工作。

除了`runBlocking`，所有协程构建器都声明为`CoroutineScope`的扩展，我们来了解一下为什么。

结构化并发的主要用途是:

1.  取消不再需要的工作。
2.  在工作运行时跟踪工作。
3.  协程失败时发出错误信号。

如果你遵循这些指南，你可以肯定你会避免许多可能因不遵循它们而出现的令人困惑的问题。

# 取消使用范围的工作

取消对于避免做多余的工作很重要，这样会浪费内存和电池寿命；正确的异常处理是良好用户体验的关键。

如果一个动作需要很长时间才能完成，普通用户要么离开相关的 UI 元素，继续前进，要么更糟，重新打开这个 UI，再次尝试这个动作。如果不采取任何措施，前面的操作仍将在后台运行，从而导致泄漏。

在 Android 上，将一个`CoroutineScope`与用户屏幕相关联通常是有意义的。如果您在活动、片段或视图模型上使用协程，请使用我们之前看到的作用域(`lifecycleScope`和`viewModelScope`)。这样做的好处是，当用户离开屏幕时，您不需要担心取消工作。

但是记住，**你的协程只有在合作的情况下才会停止运行。**

TL；DR: *当一个作用域取消时，它也会取消它所有的协程。*

# 跟踪工作

结构化并发保证当挂起函数返回时，它的所有工作都已完成。

`coroutineScope`和`supervisorScope`让您可以安全地从挂起函数中启动协程，并且只在它的所有子进程都完成时才返回。

TL；当一个挂起的 fun 返回时，它已经完成了所有的工作。。

# 信号误差

结构化并发保证当协程出错时，它的调用者或作用域会得到通知。

一个`suspend`函数的异常将通过 resume 被重新抛出给调用者。就像常规函数一样，您并不局限于使用 try/catch 来处理错误，如果您愿意，您可以构建抽象来用其他样式执行错误处理。

如果一个由`coroutineScope`启动的协程抛出异常，`coroutineScope`可以将它抛出给调用者。由于我们使用的是`coroutineScope`而不是`supervisorScope`，当异常抛出时，它也会立即取消所有其他的子进程。

您*可以通过引入一个新的不相关的`CoroutineScope`(注意大写的`C`)或者通过使用一个名为`GlobalScope`的全局作用域*来创建非结构化并发，但是您应该只在极少数情况下考虑非结构化并发，当您需要协同程序比调用作用域存在更长时间的时候。然后自己添加结构是一个好主意，这样可以确保跟踪非结构化的协程，处理错误，并有一个好的取消故事。

TL；DR: *当一个协程失败时，调用者或者协程的作用域被警告*。

# 调度员。Main .立即

如果您浏览 kotlinx.coroutines，您可能会看到类似于`Dispatchers.Main.immediate`的内容。`immediate`是做什么的？它用于立即执行协程，而无需将工作重新分派给适当的线程。在 Android 中`launch(Dispatchers.Main)`在处理程序中发布一个 Runnable，所以它的代码执行不是立即的。让我们看看实际情况

输出:

```
Before After Inside
```

通过切换到`Dispatchers.Main.immediate`，代码按预期工作

输出:

```
Before Inside After
```

# 协程最佳实践

将调度程序注入类中

*   简化测试，因为它们可以在单元和仪器测试期间被替换。

表示层下面的层应该公开挂起功能和流

*   调用者(通常是表示层)可以控制在这些级别上完成的工作的执行和生命周期，并且可以在必要时取消。

避免 GlobalScope

*   **推广硬编码价值观**。如果你直接使用`GlobalScope`的话，硬编码`Dispatchers`可能很有诱惑力。
*   这使得测试变得非常困难。您将无法控制由您的代码启动的工作的执行，因为它将在不受控制的范围内进行。

当您需要在当前作用域之外执行任何东西时，最好在应用程序类中创建一个新的作用域，并在其中运行协程。对于这类工作，避免使用`GlobalScope`和`NonCancellable`。

这是协程系列的第 3 篇也是最后一篇文章。这肯定有很多内容需要处理，但写完这篇文章后，我对我学到的东西感到非常高兴，我希望你也是。协程还有很多，但我已经厌倦了😆现在是我冒险尝试新东西的时候了。

如果您有任何疑问，请随时联系我。

# 进一步学习

[结构化并发](https://elizarov.medium.com/structured-concurrency-722d765aa952)

[协同程序中的取消](/androiddevelopers/cancellation-in-coroutines-aa6b90163629)

[协程&不应该取消的工作模式](/androiddevelopers/coroutines-patterns-for-work-that-shouldnt-be-cancelled-e26c40f142ad)

[我对 Kotlin 协程取消和异常的误解](https://mbrizic.com/blog/coroutine-cancellation-exceptions/)

[Alain Pham](https://unsplash.com/@alain_pham?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/structured?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片