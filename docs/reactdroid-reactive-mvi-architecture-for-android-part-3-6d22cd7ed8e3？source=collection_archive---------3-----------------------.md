# react droid——Android 的反应式 MVI 架构——第 3 部分

> 原文：<https://medium.com/nerd-for-tech/reactdroid-reactive-mvi-architecture-for-android-part-3-6d22cd7ed8e3?source=collection_archive---------3----------------------->

## 移动应用中的反应式编程——一次旅行

## 以异步方式管理应用程序全局状态的 Redux 存储的 Rx 实现

# 介绍

在[之前的文章](/nerd-for-tech/reactdroid-reactive-mvi-architecture-for-android-part-2-44c3c2810f52)中，我们学习了*如何*在一个高层次上实现一个 [*Redux* -like](/nerd-for-tech/react-redux-in-kotlin-for-truly-native-mobile-development-25229937d4f) 架构。我们剩下了*存储*的两个“黑盒”方法，我们现在将使用 [*RxKotlin*](https://github.com/ReactiveX/RxKotlin) 来实现它们。
这个*商店*是 [*Reactdroid*](https://github.com/GuyMichael/Reactdroid) 库的一部分，这个库是[在 *GitHub*](https://github.com/GuyMichael/Reactdroid) 上提供的。

> 注意:Kotlin 有一个官方的 Redux，如果你愿意，你可以使用它，在这种情况下，本文将更多地学习如何构建你正在使用的工具(这是一个很好的实践)和学习[*RxKotlin*](https://github.com/ReactiveX/RxKotlin)*技术，而不是其他东西。*

# *商店的 API —订阅和派送*

*正如我们在[上一篇文章](/nerd-for-tech/reactdroid-reactive-mvi-architecture-for-android-part-2-44c3c2810f52)中了解到的，商店的核心 API 由两个方法组成— *subscribe()* 和 *dispatch()* 。
一个给*订阅*的 [*全局状态*](/nerd-for-tech/reactdroid-reactive-mvi-architecture-for-android-part-2-44c3c2810f52#4d4e) 更新，另一个更新它。
这里是这两个 API 的(非常)简化的声明:*

*商店的 API。非常简化。*

*由于 *subscribe* 基本上包含标准的 *rx* 订阅样板文件，所以让我们从 *dispatch* 开始，后面是‘juice’——更新全局状态。*

## *派遣(行动)*

*所以*分派*函数接收一个 [*动作*](https://github.com/GuyMichael/Reactdroid/blob/master/reactdroid/src/main/java/com/guymichael/kotlinflux/model/actions/Action.kt) ，这基本上是一个*键-值*对。
*键* —指定您的 *Reducer-GlobalState* 键。
*值* —指定给定键的新值，为下一个全局状态设置。*

*派遣如何影响下一个州*

**调度*和*动作*有 3 个步骤:*

1.  **dispatchSubject* —一个[*RxKotlin Subject*](https://www.baeldung.com/kotlin/rxkotlin#1-combining-observable-emissions)即*发出* *当前状态到下一状态*的函数/映射器。*
2.  **stateChangeObservable —* 发出实际的变更(“下一个”*状态*)，例如发送到 *UI 组件*(参见上面的 *subscribe* 方法)。*
3.  **dispatch(Action)* —为 *dispatchSubject* 提供一个*当前状态到下一个状态的映射器。**

> **主减速器*将在所有*存储*的*减速器*上循环，每个减速器将提供其自己的全局状态部分。需要刷新你的记忆？[回到这里](/nerd-for-tech/reactdroid-reactive-mvi-architecture-for-android-part-2-44c3c2810f52#de24)。*
> 
> *注意:与官方的[](https://reduxkotlin.org/)*相反，这个默认的*动作*实现将一个特定的键入状态映射到一个特定的值，而不是要求你在你的*减速器*中构建一个[巨大的开关盒](https://reduxkotlin.org/basics/reducers)并自己构造下一个状态。这样，我们努力拥有一个可预测的(全局)状态—您的*缩减器*中的每个*键*都是一个[枚举](https://github.com/GuyMichael/Reactdroid#flux-core-redux---like---overview---store-and-global-app-state) / [*密封类*](https://kotlinlang.org/docs/sealed-classes.html) *-* 与- [1 对 1 关系到一个*类型化的* (！)值](https://github.com/GuyMichael/ReactdroidAppExample/blob/ddf85f22547f30f428d0e581ae22b05db7cd6cb9/app/src/main/java/com/guymichael/componentapplicationexample/store/reducers/GeneralReducer.kt#L22)。
> 也减少了很多[样板](https://redux.js.org/usage/reducing-boilerplate)。
> 无论如何，*动作*可以很容易地*扩展*以具有作为值的功能，从而模仿‘标准’Redux 方法。**

**因此，我们只剩下“果汁”—*state change observable—*哪个*观察* *dispatchSubject 的排放*；*将*【当前】*状态*映射到【下一个】*状态*；并且*发布* / *发射*下一个*状态。***

## ***状态更新机制— stateChangeObservable***

**准备好了吗？这是单向状态流的核心机制，只需几行代码。**

**Redux 的核心机制——如何将动作映射到下一个状态。简化了一点点。**

**让我们快速检查一下代码中的各个部分:**

1.  ***去抖缓冲器*——一个[定制的接收动作](https://github.com/GuyMichael/Reactdroid/blob/126f0255d804a20adfa682f402957feed0c81100/reactdroid/src/main/java/com/guymichael/kotlinflux/model/Store.kt#L266)(实际上相当棘手)，它结合了*缓冲器*和*去抖缓冲器*来批量处理*状态*的变化。这允许您一个接一个地编写多个 *dispatch()* 调用，而不会触发整个*状态*的改变，也不会为每个调用重新呈现*。
    *提示:您可以将* 0-timeout *(只批量调用当前执行队列中的调用，就像* JS *中的* setTimeout(0) *或* Android *中的* View.post() *)更改为一个更大的数字，这样会批量调用更多的*dispatch*，但会导致*状态*变化的延迟更大。****
2.  **从# 1-' mapBatchedActionsToNextState '开始的批处理*动作*上的一个 *for 循环*——所有*动作*按顺序执行，直到最后(' next') *状态*准备就绪。
    记住，每个*动作*都是一个函数——*当前状态到下一个状态映射器*。**
3.  **更新/替换*商店*的“当前”状态**
4.  **通知所有*订阅者*(例如我们的*反应式 UI 组件*)关于新的*状态。***

****就是这样！**我们现在有了一个*Redux*——类似于**状态机**，即**将*键值*** 对**转换为**(‘next’)***全局状态***；批量更新；在主线程之外完成。**

**我们只剩下订阅*存储*(*stateChangeObservable*)，这与其说是感兴趣的代码，不如说是样板文件。那么让我们快速浏览一遍。**

## **订阅()**

**我将上面的方法描述为接收一个实际的*组件*。嗯，我撒谎了:)
*商店*完全不知道任何'*反应*'——就像*组件*和 *UI* 一般。因此*订阅*的方式更加通用——使用一个*接口——store observer*，它从根本上定义了类似的概念，如 *Redux 的*'[*connect*](https://react-redux.js.org/api/connect)'—TL；dr — *mapStateToProps* 。**

**总之，*订阅*到*商店*需要你定义一些“道具” *P* 和一个函数/映射器——*mapStateToProps*——它接收全局*状态*并返回一个派生的 *P* 。
每当全局*状态*改变(来自*调度*)导致派生 *P* 也改变时，您的*观察器*会得到通知。就这么简单。**

**使用声明“mapStateToProps”的通用接口订阅存储。**

**好了，这就对了，通用*商店*订阅使用 rx，带有非主线程计算。**

> **注意:对于使用*反应式 UI 组件* 订阅的 [*，有另一个包装器为您做相关的样板工作，最终，您将从 Redux 获得相同的“*](https://github.com/GuyMichael/Reactdroid#flux-core-redux---like---overview---store-and-global-app-state) *[connect](https://react-redux.js.org/api/connect) 函数。***

# **总结反应-应用-旅程**

**这次，我们学习了如何创建一个*Redux*/*Flux*like*Store*，使用 [*RxKotlin*](https://github.com/ReactiveX/RxKotlin) 来实现两个核心方法——*subscribe(观察者)*和 *dispatch(动作)*
，这两个方法都具有 rx 内置功能，如脱离主线程计算和批处理 *dispatch* es。**

**至此，我们可以结束[我们的旅程](/nerd-for-tech/what-is-wrong-with-current-mobile-development-3675ce94ccf5)来改进我们编写移动应用程序的方式。我们现在知道如何为移动开发构建(或使用)一个功能性的、反应式的架构，该架构定义了两个基础——一个 *UI 组件*和一个连接*组件*的全局状态机( *Store)* 。我们现在可以编写可预测的、定义良好的、功能性的、线程安全的、易于编写和扩展的、符合单向状态流概念的移动应用程序代码。**

**和大多数航行一样，这只是下一次航行的开始。
我们只涉及了非常基础的东西，实际的日常代码需要更多的架构特性，其中许多已经包含在 [Reactdroid](https://github.com/GuyMichael/Reactdroid) 中。**

> **注意:在完成这个系列的时候，Android 终于发布了一个稳定版本的 [JetpackCompose](https://developer.android.com/jetpack/compose) ，它基本上模仿了 React 的功能组件。再加上[的合作例程](https://kotlinlang.org/docs/coroutines-overview.html)和[的科特林-雷杜](https://reduxkotlin.org/)，这是一个强大的“包”，经过一些努力，它可以取代[的 Reactdroid](https://github.com/GuyMichael/Reactdroid) 。
> 虽然所有这些都是事实(您应该熟悉上述技术)，但我们在本系列中学到的概念现在比以往任何时候都更加重要和相关——编写移动应用程序的“标准”方式越来越接近提到的概念。
> 此外，虽然 [Reactdroid](https://github.com/GuyMichael/Reactdroid) 的核心将随着时间的推移而改变，以便在内部使用 [JetpackCompose](https://developer.android.com/jetpack/compose) ，但它的“整体”性质(例如 API、动画、缓存和数据库管理)仍然令人感兴趣(参见 [ReactdroidApp](https://github.com/GuyMichael/ReactdroidApp) ),并且仍然是 Android/iOS 核心 IMHO 所缺少的。**

**谢谢你的阅读，我希望我能教你一些东西。
请随时联系我本人或在评论中留言。
祝大家编码有成效。**