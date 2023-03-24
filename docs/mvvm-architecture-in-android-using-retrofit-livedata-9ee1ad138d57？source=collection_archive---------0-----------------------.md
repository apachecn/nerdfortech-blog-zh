# Android 中的 MVVM 架构(使用翻新、LiveData)

> 原文：<https://medium.com/nerd-for-tech/mvvm-architecture-in-android-using-retrofit-livedata-9ee1ad138d57?source=collection_archive---------0----------------------->

![](img/7ea2e56fa1e62720863374ef4af74f31.png)

来自 [Pexels](https://www.pexels.com/photo/boy-in-white-t-shirt-sitting-on-chair-in-front-of-computer-4709285/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 的 [cottonbro](https://www.pexels.com/@cottonbro?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 摄影

## 什么是建筑，我们为什么需要建筑？

架构是你构建 android 代码和文件的方式。让你的项目具有可扩展性和可维护性是非常重要的。开发人员花更多的时间维护一个项目，而不是创建它，因此架构应该是干净和易于维护的。通常，从头开始创建一个项目需要几个月的时间，但是根据需求的不同，在几年内会添加各种形式的错误修复和新功能。

## MVC VS MVVM？

MVC 是 android 的默认架构，视图是 xml 布局文件，控制器是活动类。在 MVVM 中，活动和 xml 文件都被视为视图，视图模型类包含所有业务逻辑。应用程序的用户界面和业务逻辑之间没有任何联系。

## MVP VS MVVM？

在 MVP 中，演示者知道视图，视图也知道演示者。他们通过一个界面相互交流。在 MVVM，只有视图知道视图模型。viewmodel 不知道视图。

## 这里的目标是什么？

将所有代码都写在`[Activity](https://developer.android.com/reference/android/app/Activity)`或`[Fragment](https://developer.android.com/reference/android/app/Fragment)`中是一个常见的错误。这些基于 UI 的类应该只包含处理 UI 和操作系统交互的逻辑。通过保持这些类尽可能精简，您可以避免许多与生命周期相关的问题。

要遵循的最重要的原则是关注点分离，也就是说，您的业务逻辑、UI 和数据模型应该位于不同的位置。另一个是代码的解耦:每一段代码都应该作为一个黑盒，这样改变一个类中的任何东西都不会对你的代码库的另一部分产生任何影响。

# 什么是 MVVM？

MVVM 架构是一个模型-视图-视图模型架构，它消除了每个组件之间的紧密耦合。最重要的是，在这种架构中，子节点没有对父节点的直接引用，它们只有通过可观察对象的引用。

**模型:** 一个模型代表 app 的数据和业务逻辑。推荐的实现之一是通过 observable 公开它的数据。与常规的可观察对象不同， **LiveData** 尊重其他应用组件的生命周期，如活动和片段。

ViewModel:
ViewModel 类旨在以生命周期感知的方式存储和管理 UI 相关数据。ViewModel 不应该知道正在交互的视图。ViewModel 类允许数据在配置更改(如屏幕旋转)后仍然存在

**视图:**
视图是指应用程序的 UI 组件，即活动和片段。视图在这个模式中的角色是观察一个 ViewModel 可观察对象，以获取相应更新 UI 元素的数据。这部分是在我们的 MainActivity 类中实现的，在这个类中，我们观察来自 ViewModel 的数据，并在适配器上设置观察到的数据。

## 什么是 Android 仓库？

大多数应用程序可以从本地存储器或远程服务器保存和检索数据。Android 存储库是决定数据应该来自服务器还是本地存储的类，将您的存储逻辑与外部类解耦。

现在我们已经熟悉了我们将在项目中使用的各种术语。让我们看看实际运行的代码

# 实施:

在这个例子中，我们将使用一个简单的项目来展示 MVVM 的概念，在这个项目中，我们将使用 reform 从 API 中获取数据，并在我们的应用程序中显示这些数据

## 第一步

我们将为我们的响应创建模型类。你也知道这是 POJO 类。这表示来自 API 的响应

我们还使用了 ResponseAPI 类，稍后您会看到。

第二步:
为了从 API 获取数据，我们将使用一种改进。你可以在我之前的一篇文章 [**中找到实现，这里**](/nerd-for-tech/using-retrofit-2-for-api-calls-674f59355383)

**第三步:** 现在我们将创建我们的模型视图类。LiveData 类旨在以生命周期意识的方式存储和管理与 UI 相关的数据。注意我们是如何在模型视图类中使用 init 方法从 API 获取客户列表的。因此，每当我们初始化 CustomerViewModel 类时，它都会自动从服务器获取数据。
存储库类是我们进行 API 调用和使用 Livedata 观察 API 响应的地方

需要注意的一点是，我们使用 postValue 方法向我们的观察者发送更新。
有两种选择，即你可以在主线程上更新数据，也可以使用后台线程更新数据。因此，`setValue`和`postValue`的用例只取决于这两种情况。

当使用主线程更改数据时，您应该使用 MutableLiveData 类的`setValue`方法；当使用后台线程更改 LiveData 时，您应该使用 MutableLiveData 类的`postValue`方法。

## 第四步:

现在让我们看看我们的活动类，我们将在其中声明我们的 livedata 对象并在 UI 中显示数据

*暂时就这样吧！！
感谢阅读，别忘了分享给你的开发伙伴:)【CodeTheraphy.com】本帖最初发布于*[](https://www.codetheraphy.com/) **。**

**更多文章关注我上* [*中*](/@nandishswarup) *和*[*CodeTheraphy.com*](https://www.codetheraphy.com/)*你也可以联系我上*[*LinkedIn*](http://www.linkedin.com/in/nandish-swarup)*