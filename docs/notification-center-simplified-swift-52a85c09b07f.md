# 简化的通知中心。

> 原文：<https://medium.com/nerd-for-tech/notification-center-simplified-swift-52a85c09b07f?source=collection_archive---------10----------------------->

你听说过在古代鸽子可以帮助将信息和数据传送到世界的任何一个角落吗？让我们借助这个古老的概念来理解通知中心这个概念。听起来很有趣，不是吗？让我们开始吧！！！！

NSNotificationCenter 是 Xcode 的鸽子，它帮助你在整个应用程序中以“通知”的形式传递信息。简而言之，我们可以说 NSNotificationCenter 是一个通过应用程序交流信息的工具。与推送或本地通知不同，在推送或本地通知中，您可以显示希望接收用户的通知，NSNotificationCenter 允许我们基于应用程序中发生的操作在类和/或结构之间发送和接收信息。我们可以把整个陈述变得更有趣，让我们开始吧。

***“我们可以把 NSNotificationCenter 想象成一只鸽子，我们可以把它调到不同的地方发送或获取信息。”***

![](img/081ad7802a122dadbf3d926ecef7d962.png)

这只鸽子会全程帮助我们，只要和他在一起。

## 通知中心的工作原理:

看一下鸽子背的袋子，里面储存了所有的信息和数据，其中一些已经收集，一些将被发送。在 NotificationCenter 的概念中，包是 NotificationCenter.default，我们必须从这里检索数据或者将数据推送到这里。让我们潜入更深的地方。

鸽子必须为每个信息有一些 id，这样信息/数据将被传递，不应该被错误地传达，对吧！！！。在处理通知中心时，我们还必须为将要发送或接收的每个通知提供一些特定的标识符。

在这里，我为大家做了一个简单的项目，非常简洁易懂，只要和鸽子在一起😇。

![](img/6d6d4054495dfab74f5df6dcef08f853.png)

双视图控制器预览

我使用了两个视图控制器，一个是 PetVC，另一个是 ChoosePetVC。创建了一个白色按钮“选择你最喜欢的宠物”和一个灰色标签“宠物目的地”。当我们点击“选择您最喜欢的宠物”按钮时，您将导航到“ChoosePetVC”，然后我们将点击“ChoosePetVC”中的任何按钮，根据我们要发送的数据，这些数据将显示在 PetVC 中的“宠物目的地”标签中，是不是很酷😎。

首先在 PetVC 中设置扩展名 Notification.name，以便为所有发送或接收的通知提供唯一的标识符。

![](img/e6f2ebc5553c6eb6c0e7f32cd392989b.png)

通知。唯一标识的名称。

然后在 PetVC 的 viewDidLoad()中编写如下所示的“addObserver()”方法，用于提取数据。

![](img/50de8031d8b0d00b459ad9944320be69.png)

获取信息的 addObserver 方法。

为将在 addObserver 方法中传递的每个通知编写选择器方法，在 PetVC 中，我们可以在代码中添加我们的操作。

![](img/c0938ddf6c3d8fdfd3a510da6eca8316.png)

在 addObserver 方法中传递的 Selector 方法请看上面。

这是为 PetVC 做的所有工作，让我们在 ChoosePetVC 中做一些代码，然后我们将完成我们的迷你项目。

在按钮的 ChoosePetVC add IBActions 中，通过单击该按钮，我们可以发布包含要发送的对象的通知。

![](img/a1265b8ecf678177dcd15a7adf16e48b.png)

在 IBAction 块中发布通知

通过单击任何按钮“导航控制器”。。popViewController(animated:true)"只需将您带到 PetVC，在 Pet 目的地标签中，将显示您已通过 IBAction 块的通知。在这里我们发送字符串“爱猫”的对象，你可以尝试不同的东西。🤓

因此，在 ChoosePetVC 中，我们发布通知，在 PetVC 中，我们在通知的帮助下获得通知。Name()，使它们具有唯一性和可识别性。

完成所有代码后，它看起来会像这个✌🏻

![](img/fbdd7d3ead5125e1cd851f0032908fc8.png)

第一视图控制器

![](img/2d54e4c7bf70e0f5dd01aa42f438e6cd.png)

ChoosePetVC(第二视图控制器)

希望你们喜欢鸽子之旅😄。