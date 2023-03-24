# 另一个依赖注入

> 原文：<https://medium.com/nerd-for-tech/another-dependency-injection-81371b3434e6?source=collection_archive---------0----------------------->

![](img/27e8cafff3802392903b0ff521726c5e.png)

你好 iOS 介质阅读器！

今天我想给大家介绍一下 Swift 中**依赖注入** ( **D.I.** )的另一种方式。我知道有很多关于 **D.I.** 的文章，比如:
[https://www . swiftbysundell . com/articles/dependency-injection-using-factories-in-swift/](https://www.swiftbysundell.com/articles/dependency-injection-using-factories-in-swift/)
或者库:
[https://github.com/Swinject/Swinject](https://github.com/Swinject/Swinject)。

那么问题来了:**我为什么选择这个题目。再次……**

答:我选择这个主题是因为它非常广泛，并且它的实现依赖于对创作者的解决方案的了解。

我想展示的是轻量级的 **D.I.** 用于在视图控制器之间传递协议，或者其他东西，这取决于你的架构。

为此，我们需要用关联类型和注入方法的声明来声明协议，这将注入所需的协议。

让我们称这个协议为**可注射的:**

现在让我们创建一个能够注射**可注射**协议的东西。举个简单的例子，我将使用视图控制器。

让我们更进一步，创建 **BaseViewController** ，它包含一个我们可以注入协议的方法。为此，我将使用[泛型机制](https://docs.swift.org/swift-book/LanguageGuide/Generics.html):

好吧。现在我们有了基本组件，让我们用它们做一些工作，然后我们将看到结果。

首先，我们需要创建一个我们想要使用协议的场景。让我们假设我们的 **FirstViewController** 正在从**核心数据**中获取一些对象。因此，视图控制器的实现将如下所示:

CoreDataProvider 的实现如下所示:

现在我们需要做的是将 **CoreDataProvider** 注入到 **FirstViewController** 中。为此，我们创建一个对象来实现**可注入**协议:

**第一视图注入器:**

好，让我们把这些积木连接起来！

**第一视图控制器**:

现在注入我们的核心数据服务并显示 **FirstViewController** 。

**AppDelegate:**

如果我们现在运行我们的代码，我们会看到一个黄色的场景，Xcode 也会打印:"*【first view controller】****从核心数据*中获取一些不错的模型"。至少应该是这样的…希望是！**

**让我们继续，添加第二个场景，它将从**核心数据**中获取我们惊人的模型，并从 REST API 自己的注入器中获得一些不可思议的东西！**

****第二视图注入器:****

****网络服务提供商:****

****第二场:****

**在 **viewDidAppear()** 方法中为 FirstViewController 添加以下代码:**

**我想你已经注意到我们有两个协议要注入到 **SecondViewController** 中，所以在 **SecondViewInjector** 中，我使用了[元组](https://www.hackingwithswift.com/example-code/language/what-is-a-tuple)来注入两种不同类型的协议，多亏了这个解决方案，我们可以轻松地注入它们。**

**现在，如果我们运行整个代码，我们应该看到:**

*   **" *[FirstViewController]从核心数据中获取一些不错的模型*"**
*   **" *[SecondViewController]从核心数据中获取一些不错的模型*"**
*   **" *[SecondViewController]从 REST API 中获得非常棒的东西*"**

****结论:****

**只用了几行代码，我们就实现了真正的***【d . I .】**协议注入，因此我们不需要外部库。它也可以很容易地在单元测试中用于存根响应。***

***请以此为激励，花点时间自己开发解决方案，而不是立即添加外部依赖:)***

***感谢阅读，祝**好运**！***

*****完整代码:** [https://github.com/Rafal-Prazynski/D.I.Medium](https://github.com/Rafal-Prazynski/D.I.Medium)***