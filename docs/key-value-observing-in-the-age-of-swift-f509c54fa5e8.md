# Swift 时代的键值观察

> 原文：<https://medium.com/nerd-for-tech/key-value-observing-in-the-age-of-swift-f509c54fa5e8?source=collection_archive---------8----------------------->

## 抛弃陈旧的语法，用快捷的方式编写键值观察

![](img/64c43b290cf47c5c8e0b06a65b1913bf.png)

@titopixel 的双筒望远镜插图

**键值观察**模式，俗称 **KVO** 模式，从 Cocoa 成立之初就已经存在。它用于通知对象有关其他对象属性的更改。

当涉及到在你的应用程序的逻辑分离的部分之间交流变化时，我们有几个选择。最广泛使用的一种是`Delegation`模式，如下例所示:

## 授权的优势

1.  **自证**。看着代码，你就会确切地知道你应该如何使用它，它做什么。
2.  **可重复使用**。只要你想要一对一的关系。

## 授权的限制

当您能够访问代码的实际实现时，委托是最有用的。在上面的例子中，我们在子类中一个非常特殊的操作点上使用了`delegate?.doSomething()`,因为我们可以访问 Child 的实现，这样我们就可以把委托调用放在我们想要的地方。

然而，有些情况下授权是无法实现的。

例如，如果你正在使用系统提供的类，比如`AVFoundation`中常见的`AVPlayer`，我相信每个玩家都使用的特性是观察玩家的状态(播放，暂停，等等。).要做到这一点，由于我们不能修改`AVPlayerItem`的源代码，我们只能要求在`[addObserver(_:forKeyPath:options:context:)](https://developer.apple.com/documentation/avfoundation/media_playback_and_selection/observing_playback_state)`之前得到通知。*甚至苹果官方文档代码样本也是用旧语法编写的*

即使是在 Swift 中的语法，我们也从老的 Objective-C 时代继承了 KVO API 的设计。即使我们写得越来越快，它也非常强大，是每个开发人员必须知道的。

## 旧的键值观察

在上面的代码示例中，有两个难点，每个都用注释标记:

1.  观察被设置为一个单独的步骤。如果您要设置多个观察，每个观察将占用一个`addObserver`来设置。
2.  通知在一个地方处理。假设您设置了多个观察，在这里您必须用`if keyPath == #keyPath(Your.KeyPath)`检查每个通知。并且用于从通知中提取有用信息的语法写起来很乏味并且难以阅读。
3.  为了避免崩溃，必须明确删除您设置的任何观察值。

## 新的键值观察

在 Swift 中，我们可以向旧的机制注入新的血液。

*这个“\ .”syntax 是提取 keyPath 的快速语法。约翰·桑德尔有一篇关于此* *的* [*优秀文章。通读文章的前半部分应该会给你足够的介绍。*](https://www.swiftbysundell.com/articles/the-power-of-key-paths-in-swift/)

同样是我们添加观察者的地方，这次语法变成了`.observe(_keyPath:options:changeHandler:)`。参数`options`指定当调用`changeHandler`时，您是想要旧值、新值，还是两者都要。就像旧语法一样。

在回调中，您将获得两个参数供您使用:

1.  被观察的物体。
2.  `change`。将会有一个`change.oldValue`和`change.newValue`取决于你指定的选项。

需要记住的重要一点是:

> *您必须保存观察结果，因为它将从内存中释放，如果不再被引用，观察者将自动从对象中移除*。

因此，你可以看到我一直保存着调用`.observe()`的结果，直到视图控制器的生命周期结束。并且您不再需要显式调用 deInit 中的`removeObserve`。