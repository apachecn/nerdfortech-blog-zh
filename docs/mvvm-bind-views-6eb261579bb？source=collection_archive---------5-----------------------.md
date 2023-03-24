# MVVM —绑定视图

> 原文：<https://medium.com/nerd-for-tech/mvvm-bind-views-6eb261579bb?source=collection_archive---------5----------------------->

## 一种使用香草雨燕的方法

![](img/261b6649f355e1d7594a9e3020a029fb.png)

照片由[格雷格·罗森克](https://unsplash.com/@greg_rosenke?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/bind?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

嗨，开发者。我将要向您展示的是我通常使用 Swift 将`ViewModel`属性与`Views/ViewControllers`挂钩的方式。如果你的项目使用`SwiftUI and/or Combine`，这可能与你无关。然而，将视图绑定到属性更改的概念非常相似。我将尽可能地清晰和简洁，所以像将 UIViews 添加到层次结构或 DispatchQueues 之类的细节都会被考虑。

在我继续下去之前，一个快速的免责声明*——我发现这是一个有用的方法，这并不意味着它是对的或错的。*

也就是说，这些是我们将在这篇文章中看到的演员:

*   男— *人*
*   V — *视图控制器*
*   虚拟机— *视图模型*

要构建的特性包括获取这个人的信息并在标签上显示它——这个概念可以推广到任何类型的视图、表格、集合等。下面是一个 MVVM 模式的基本实现。

*关于 MVVM 工作的更多细节，这里有一个不错的帖子【链接】*

L et's say `ViewModel`需要从一个 API 或任何外部服务中获取这个人的信息(我在这篇文章中没有深入探讨)。一旦检索到数据，`ViewModel`执行块`onPersonChanged`来通知视图数据已经更新。

```
class ViewModel {

    var onPersonChanged: ((Person) -> Void)?

    func fetchPosts() {
        // these values are hard-coded. Not relevant(to this post)
        // where this data is coming from.
        let person = Person(id: 1, name: "Cristhian", age: 26)
        onPersonChanged?(person)
    }
}
```

这意味着，`ViewController`需要设置我们上面提到的那个块，所以当这个人的信息改变时，视图可以“刷新”UI 组件值。

函数`refreshUI`简单地获取一个人并将值设置到标签上。另一方面，`addObservers`函数设置人员数据更新时的回调。

> 到目前为止，这个实现没有什么不好，但是，当视图增长时，即使一些值没有改变，也会调用函数 refreshUI。

这就是为什么我现在的计划是挑选出 viewModel 的每一个属性，以获得完全的独立性和可测试性。

现在你可能想知道——那个`Dynamic`类是什么？嗯，这就是这篇文章的重点。`Dynamic`是一个泛型类，它允许我们对其值的变化做出反应。这意味着，`ViewController`可以观察到`ViewModel`的动态属性的变化，然后对其进行操作，但首先，让我们看看类。

正如您所看到的，这是一个非常简单但功能强大的类，只要设置了值，它就会执行块`listener`。`Dynamic`还公开了一个名为`bind`的函数来设置监听器。现在让我们看看`ViewController`是如何挂钩到`ViewModel`属性的。

```
private func addObservers() {
    viewModel.personName.bind { [weak self] name in
        self?.nameLabel.text = name
    }

    viewModel.personAge.bind { [weak self] age in
        self?.ageLabel.text = "\(age)"
    }
}
```

*感谢阅读。我希望你喜欢这段代码，如果它对你有用，不要害羞👏关于这篇文章。下次见。*