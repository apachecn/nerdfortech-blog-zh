# swift 中的属性包装

> 原文：<https://medium.com/nerd-for-tech/property-wrapper-in-swift-e56e3854650b?source=collection_archive---------0----------------------->

![](img/db9574f6a20433cf24ca86a1c0a77324.png)

让我们来理解它们为什么存在。我们如何利用它们来提高代码的可重用性和可读性。

**我们为什么需要？**

当处理属性时，当属性值初始化和改变时，我们需要关联相同的逻辑。例如，根据规则集验证值或在赋值前更改值。一种方法是使用属性观察器(willSet/DidSet)。但似乎每一个物业都没有坚持反复干。有更好的方法吗？是的，什么属性包装器存在。以下是官方文件对此的描述。

*属性包装器在管理属性存储方式的代码和定义属性的代码之间增加了一层分离。例如，如果您有提供线程安全检查的属性，或者将它们的底层数据存储在数据库中，那么您必须在每个属性上编写代码。使用属性包装时，只需在定义包装时编写一次管理代码，然后通过将该管理代码应用于多个属性来重用它。* [*来源*](https://docs.swift.org/swift-book/LanguageGuide/Properties.html)

每个属性包装都以 word @propertyWrapper 开头，并存储了 wrappedValue 属性。在 wrappedValue 的 didSet 上，我们可以放置通用的验证或更改逻辑。wrappedValue 的类型应该与我们包装的属性的类型相同，这样才有意义。

注意:在上面的例子中，我们需要显式地大写传递给我们的初始化器的任何字符串。因为属性观察器仅在值或对象被初始化后被触发。

**用法:**

太酷了😎没错。你可能想知道什么是投影值。它可以通过定义一个*预计值*属性*来公开更多的功能。*您可以使用 *$variableName 进行访问。*可以有不同的逻辑和类型。让我们举另一个需要经常修剪琴弦例子。

现在你不需要到处调用 trimmingCharacters 函数。无论何时访问属性，在属性前面添加@Trimmed 会自动完成。

让我们再举一个例子，我们如何修改现有的用法或用户默认设置。里面有很多样板代码。[信用](https://www.swiftbysundell.com/articles/property-wrappers-in-swift/)

您可以看到如何在 userDefault 中检索和存储值。这是更优雅和快捷的做法。

**已知问题**

*   尚不支持错误处理，可能会包含在未来的 swift 版本中。在这里你可以尝试从属性包装抛出。
*   组合在属性包装中很难。因为最外面的包装作用于最里面的包装类型的值。[求更](https://nshipster.com/propertywrapper/#property-wrappers-are-difficult-to-compose)。

**结论**

请在您的项目中尝试属性包装。这将是你工具箱中的一个很好的工具。因为它是 swiftUI 自带的，所以将来你可能会经常用到它，为什么不试试呢？感谢阅读！

**资源**

[SwiftBySundell](https://www.swiftbysundell.com/articles/property-wrappers-in-swift/)

[NSHipster](https://nshipster.com/propertywrapper/)

[Swift Doc](https://docs.swift.org/swift-book/LanguageGuide/Properties.html)

# 感谢您的阅读！🚀如果你喜欢这篇文章，请👏所以其他人也可以看:)

***跟我上*** [***推特***](https://twitter.com/suryakan84) ***了解更多。***