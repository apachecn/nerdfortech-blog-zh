# 2021 年 Android 安全改造调用 kotlin 协同程序扩展—第三部分

> 原文：<https://medium.com/nerd-for-tech/safe-retrofit-calls-extension-with-kotlin-coroutines-for-android-in-2021-part-iii-583249b0e86b?source=collection_archive---------3----------------------->

![](img/ae7cee4861952fceedb224c4061c31cf.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上[的照片](https://unsplash.com/@ffstop?utm_source=medium&utm_medium=referral)

这就是了。这个小编的结尾。如果你还没有看过前两部分，这里有它的链接。

[](https://christopher-elias.medium.com/safe-retrofit-calls-extension-with-kotlin-coroutines-for-android-in-2021-part-i-d47e9e2962ad) [## 2021 年 Android 安全改造调用 kotlin 协同程序扩展—第一部分

christopher-elias.medium.com](https://christopher-elias.medium.com/safe-retrofit-calls-extension-with-kotlin-coroutines-for-android-in-2021-part-i-d47e9e2962ad) [](https://christopher-elias.medium.com/safe-retrofit-calls-extension-with-kotlin-coroutines-for-android-in-2021-part-ii-fd55842951cf) [## 2021 年 Android 安全改造调用 kotlin 协程扩展—第二部分

christopher-elias.medium.com](https://christopher-elias.medium.com/safe-retrofit-calls-extension-with-kotlin-coroutines-for-android-in-2021-part-ii-fd55842951cf) 

# 快速回顾一下！

首先，祝贺你取得了这么大的进步👏。

*   我们需要一个分机来拨打✔️.安全改造电话
*   我们需要一种方法来解析异常并将它们返回给人类可读的 objects✔️.
*   我们需要把成功或失败作为 result✔️.的回报

现在我们将添加我们的中间件。这个功能将允许我们**阻止**执行改造调用，如果它的任何条件没有被提供的话。大约

```
if(middlewareConditionsAreSupplied) executeRetrofitCall else NOT!
```

# 中间件需求

*   **可扩展的** —我们的应用可以有多个中间件。我们可以有一个验证网络连接的中间件，另一个验证一些特定的特性参数，等等。任何你能想象到的东西来阻止改造电话的执行。
*   **易于注入** —我们需要创建一个对象，它可以易于注入到我们的安全改进调用包装器扩展中。
*   **易于模仿** —我们需要以某种方式使这个中间件易于模仿，以防止我们的单元测试变得难以测试。
*   **检索所有的** —我们需要一种方法来检索我们所有的中间件，并验证它们中的每一个。

# 代码时间🔥

既然我们已经设置了中间件的需求，我们就可以编码了。让我们从第一部分开始。我们计划将该功能集成到我们的调用包装器扩展中，因此，这意味着中间件的失败将返回一个**失败**对象。这是我们的起点。

```
class NetworkMiddlewareFailure(
    val middleWareExceptionMessage: String,
) : Failure.CustomFailure()
```

简单的蛋糕。让我们进入下一个陡坡。我们的应用程序可以有多个中间件对象，其中任何一个都需要以某种方式进行验证，让我们使用继承原则来完成这一步。

这个类将完成这项工作。我们想要创建的每一个中间件都将**扩展**这个类，**覆盖**这个`isValid()`方法(*这里是放置我们的逻辑*和`failure`变量的地方，这些变量都是从上面的自定义失败中扩展而来的。

那么让我们创建一个简单的中间件吧！

验证手机是否连接到互联网的中间件。

上面的类负责在`connectivityUtils`接口的帮助下告诉我们是否连接到互联网(实现类隐藏在某处)。

如果`isValid()`方法返回 false，那么在`resourceProvider` 接口的帮助下，我们将使用`NetworkMiddlewareFailure`和来自 res/strings 文件夹的自定义消息。如您所见，我们的实现非常灵活，允许我们使用任何想要的东西来创建中间件。

好了，我们可以创建许多中间件，现在呢……？我们需要某种方法来提供我们所有的中间件，并在每个中间件上运行`isValid()`方法🤯！

嘿嘿嘿👊💥！不要惊慌。我们可以用**依赖倒置原则**非常容易地解决这个问题。对我们来说，依赖抽象比依赖实现更好。让我们创建一个能做我们想做的事情的界面。

接口的一个方法来检索所有的 BaseNetworkMiddleware 实例。

现在是实施的时候了。我们将创建一个类来实现我们的`MiddlewareProvider`接口并覆盖那个`getAll()`方法，以便返回我们所有的中间件对象。我们可以在这里用一个构建器模式来做一点花哨的东西。

在私有列表中存储所有中间件实例的 MiddlewareProvider。

正如你所看到的，上面的类将添加所有你想要的中间件。你只需要在你的依赖注入图中提供这个类，它可以来自 dagger 或者 koin 或者其他什么。

关于如何向您的 DI 图提供您的 MiddlewareProvider 的示例。

现在我们有了一个中间件提供者的单一实例。让我们修改调用包装器扩展！

干得好，我的朋友👏！您已经为我们的扩展实现了一个中间件，并使它更加安全！现在，您可以按照我在本系列的第一部分中向您展示的那样使用它了。

我希望你喜欢它。如有任何建议、改进、投诉、笑话等。添加评论，文件和问题，生成公关，等等！注意安全！

看看这个实现是如何在这个项目中使用的

[](https://github.com/ChristopherME/movies-android) [## Christopher me/电影-android

### Movies 是一个简单的项目，可以学习和使用一些 android 组件、架构和 Android 工具…

github.com](https://github.com/ChristopherME/movies-android) 

回头见！👋