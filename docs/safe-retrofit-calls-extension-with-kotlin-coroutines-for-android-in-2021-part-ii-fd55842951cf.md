# 2021 年 Android 安全改造调用 kotlin 协程扩展—第二部分

> 原文：<https://medium.com/nerd-for-tech/safe-retrofit-calls-extension-with-kotlin-coroutines-for-android-in-2021-part-ii-fd55842951cf?source=collection_archive---------4----------------------->

![](img/e5bbe947275199afdfd849498540cf21.png)

照片由[拍摄于](https://unsplash.com/@ffstop?utm_source=medium&utm_medium=referral) [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的

又见面了👋，这是该系列的第二部分。如果你还没有读第一部分，我鼓励你去读一读，以便理解我们现在要做的所有事情。

[](https://christopher-elias.medium.com/safe-retrofit-calls-extension-with-kotlin-coroutines-for-android-in-2021-part-i-d47e9e2962ad) [## 2021 年 Android 安全改造调用 kotlin 协同程序扩展—第一部分

christopher-elias.medium.com](https://christopher-elias.medium.com/safe-retrofit-calls-extension-with-kotlin-coroutines-for-android-in-2021-part-i-d47e9e2962ad) 

现在我们知道了如何用协程处理异常，让我们来编码。

# 1.建立或项目⚒️

我们将为此使用改型和协同程序，因此我们将把以下依赖项添加到我们的应用程序(或 android/kotlin 库)模块 gradle 文件中

```
dependencies { // Current versions at the time
  def retrofit_version = "2.9.0" 
  def coroutines_version = "1.5.0"

  implementation "com.squareup.retrofit2:retrofit:$retrofit_version" implementation "com.squareup.retrofit2:converter-moshi:$retrofit_version"

  implementation "org.jetbrains.kotlinx:kotlinx-coroutines-core:$coroutines_version"}
```

# 2.解析例外♻️

执行网络调用时可能会出现一些常见的异常，如`SocketTimeoutException`、`SSLHandshakeException`、`HttpException`等。但是我们不会直接发送这些异常，相反，我们会将这些异常转换成可读的失败！我们开始吧！

首先，我们将从 [Failure](https://github.com/ChristopherME/movies-android/blob/master/functional-programming/src/main/java/com/christopher_elias/functional_programming/Failure.kt) 类扩展它们来创建或定制失败。

自定义故障的创建

既然我们已经得到了自定义的失败，是时候将异常映射到其中了。

将异常转换成自定义故障。`**ResponseError**`类只是一个表示已处理后端故障的结构，类似 4XX 错误。

# 3.改造调用执行线程安全⛑️

现在我们有了异常解析器扩展函数，我们可以用它来捕获和解析在执行网络调用时可能发生的任何异常。

安全翻新调用包装器

所以上面的 suspend 函数有一些关键的特性，我将向你解释。

*   **ioDispatcher** —协程调度程序，它应该是应用程序的 IO 调度程序，也是单元测试中的 TestCoroutineDispatcher。
*   **适配器** —它接受一个 JSON adapter<ResponseError>适配器，用于将 JSON 对象解析为所需的 response error 数据类
*   **retro fit call**——一个跨线挂起函数，它将在`withContext`块中接受您的服务并安全执行它。如果你没有内联函数的经验，可以看下面的视频，弗洛里纳·芒特内斯库解释了它
*   **to success()&to error**()—这些只是将任何对象包装到一个。成功与否。错误实例。

# 4.整合所有🧩

好了，就这样。你有一个安全的改造调用包装扩展功能！你可以像这样使用它

我使用`.let {…}`的最后一部分只是因为`actorsService.getActors()`返回了一个`ResponseItems<ActorResponse>`对象。我只是使用了`[Either.mapSuccess](https://github.com/ChristopherME/movies-android/blob/4140aeaa004d2dfb594e8b9da404037760ab8c93/functional-programming/src/main/java/com/christopher_elias/functional_programming/Either.kt#L66)`助手方法来提取结果，并按预期返回一个 ActorsResponse 列表。

所以那是什么…是吗？嗯……算是吧。我们可以通过添加一个中间件层来增加一点定制！中间件能为我们做什么？它可能会检查手机是否连接到互联网，或者你可以验证一些数据，等等。

让我们实现一个有趣的中间件。👉前往[第 3 部分](https://christopher-elias.medium.com/safe-retrofit-calls-extension-with-kotlin-coroutines-for-android-in-2021-part-iii-583249b0e86b)了解如何操作！

示例项目👇

[](https://github.com/ChristopherME/movies-android) [## Christopher me/电影-android

### Movies 是一个简单的项目，可以学习和使用一些 android 组件、架构和 Android 工具…

github.com](https://github.com/ChristopherME/movies-android) 

再见👋