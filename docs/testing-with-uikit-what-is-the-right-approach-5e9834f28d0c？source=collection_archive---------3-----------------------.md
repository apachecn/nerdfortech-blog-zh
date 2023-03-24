# 用 UIKit 测试——什么是正确的方法？

> 原文：<https://medium.com/nerd-for-tech/testing-with-uikit-what-is-the-right-approach-5e9834f28d0c?source=collection_archive---------3----------------------->

在进入正题之前，我想喝杯咖啡，回忆一下我们的老朋友:UIKit。SwiftUI 的优势相当明显，性能更好，UI 代码更干净，更容易理解但仍然不够可靠，不足以说服所有人。更不用说大多数项目通常从低于 13 的 iOS 版本开始，所以，不管喜欢与否，UIKit 在未来几年内仍然会伴随着我们。所以今天，我将通过围绕这个旧框架讨论与测试相关的问题来回顾它

![](img/c87de97de18929253b774bb043abec98.png)

图片鸣谢: [@timbennettcreative](https://unsplash.com/@timbennettcreative)

# 困难和没有标准的方法

UIKit 是一种苹果的内置框架，我们无法控制它。我们几乎不知道它是如何工作的，不得不相信它。听起来熟悉吗？是的，听起来我在谈论第三方框架。对于这些类型的框架，我们通常会感到困惑，不知道如何测试这一点。

我见过最多的流行方法是快照测试。这意味着我们要确保 UI 如我们所期望的那样出现，或者检查流程是否正确。但这就足够了吗？另一个问题是，有时我们甚至会迷失在框架测试中——这不是我们的工作。

此外，网上还有很多关于应该用什么设计模式来代替 MVC 的文章、教程和争论。他们可能是 MVVM，MVP，或者毒蛇等等。但他们中的大多数都缺少最重要的部分，测试，而且令人痛苦的是，没有人关心这个问题。显然，我们——iOS 开发者，遗漏了一些东西。

# 行为测试方法

以下是我在接近 UIKit 测试时的一些经验和知识。除了快照测试之外，我们可以在将实现注入这个框架时测试它们。那就是所谓的 ***行为测试*** 。我们应该 ***测试与框架*** 的交互，而不是测试框架如何工作。例如，
-我们不测试:如果 viewDidLoad 实际上被调用❌
-我们应该测试:当框架告诉我们视图被加载✅时，我们要做什么

这种方法非常困难，需要花费大量的时间和精力。为什么？因为我们要比别人学得更多——不仅是它在生产上的 API，还有测试阶段对应的交互方法。通常，这些方法是我们很少使用的。

例如:
-我们希望在视图加载后向服务器发送一个请求- > viewDidLoad()
-我们需要一种方法在任务完成时获得这个信号——所以我们必须知道:loadViewIfNeeded()

```
func test_doSomething_afterViewDidLoadGetsCalled() {
  let myService = Service()  
  let sut = MyViewController(service: myService)
  sut.loadViewIfNeeded()
  XCAssertEqual(myService.callCount, 1)
}
```

另一个例子，当我想在拉至刷新后测试我的行为特性时，我需要了解更多信息，而不仅仅是产品代码。

```
refreshControl?.allTargets.forEach { target in
  refreshControl?.actions(forTarget: target,
                          forControlEvent: .valueChanged)?.forEach {
                  (target as NSObject).perform(Selector($0))
                 }
  }
```

# 命名约定

最后但同样重要的是，命名惯例。有时候，你需要以一种通用的方式命名一个函数或者一段代码，或者它可以在更高的层次上显示用户的行为，而不是一个非常具体的名称。这将把测试部分与实现细节分开。

例如，要知道用户的出生日期，我们有两个选项，*日期选择器*或*用户手动填写文本字段*。所以我们不应该像
-*SUT . selectfromdatepicker*或者*SUT . getvaluefromtextfield*❌
-我们应该命名:***SUT . simulateusergetsdateofbirthvalue***✅

而***simulateUserGetsDateOfBirthValue***是我们组件的扩展。因此，如果客户端有变化，我们只需要改变扩展部分——不需要改变测试套件中的任何东西。

```
private extension MyViewController {
 func simulateUserGetsDateOfBirthValue() {
   textfield.getText()
 }
```

# 结论

以上只是挡住我们 UIKit 测试路径的其中一块巨石。我们需要克服许多其他障碍。

如你所见，这是不容易获得和学习的知识。我认为光靠我自己是不够的，在这一点上，我真的需要一个能向我展示这些东西的导师。幸运的是，我找到了一个叫做[基本 iOS 开发者](https://www.essentialdeveloper.com/)的团队，这两个人是:卡伊奥和迈克。我没有为任何人做广告，我只是说出事实。他们是我遇到过的最好的导师。它们极大地改变了我的心态。

好了，就这些。我希望你喜欢这篇文章，并分享给你的朋友。我总是公开讨论任何与 iOS 相关的问题。下一篇文章再见。