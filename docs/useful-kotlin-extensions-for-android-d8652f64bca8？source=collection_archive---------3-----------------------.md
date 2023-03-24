# 适用于 Android 的 Kotlin 扩展

> 原文：<https://medium.com/nerd-for-tech/useful-kotlin-extensions-for-android-d8652f64bca8?source=collection_archive---------3----------------------->

![](img/f98f17a433c6c355b81c236d06d0b861.png)

Kotlin 有许多令人惊叹的特性，但其中最突出的是扩展。扩展用于向现有类添加功能。它们在帮助减少样板代码方面非常有用。好的一面是你可以把它们添加到任何类中，包括那些你不能修改的来自第三方库的类。大多数时候我们看到的是*扩展函数*，但是 Kotlin 也支持*扩展属性*。

# 有用的扩展功能

让我们从一些使代码更容易阅读的扩展开始，这些扩展通过减少样板代码来提高代码质量。

# 语境

Android API 会随着时间的推移而变化，像获取给定 id 的颜色这样简单的事情会变得不那么简单。下面的扩展有助于减少与检索给定 ID 的资源相关的样板代码。

## 使用

我们都知道检查用户是否授予了应用程序所需的权限是多么的重复。下面的扩展可以帮助你做到这一点。它不能解决整个问题，但有助于减少代码量。

## 使用

复制一些文本或网址到剪贴板应该是非常简单的事情。它只适用于纯文本，但这是我们大多数时候使用的，如果你需要支持其他格式，你可以改变它。

你的 app 里有没有用户需要打开浏览器才能查看的网址？您是否正在处理浏览器或同等应用程序可能丢失的情况？下一个扩展抽象了大部分需要的东西。你只需要给它一个 Uri，并做一些事情，比如如果没有应用程序来解析意图，就显示一个错误。

## 使用

# 日期

如果您需要反复记录一个日期对象或者通过 API 调用发送它，这个扩展非常有用。这里需要注意的是，ISO 格式是通用的，它不会因国家而异，稍后我将说明这一点的重要性。

## 使用

其他一些可能有用的扩展

![](img/dc9da9d47a5bbf00087dfd9bcebd87c0.png)

照片由[阿图尔·图马斯扬](https://unsplash.com/@arturtumasjan?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/bad-lego?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄

# 闪光的未必都是金子

包括扩展在内的很多东西都可能被滥用。有些看起来不错，但实际上是伪装的代码气味。

让我们从一个无用的扩展开始。前几天在看一篇关于扩展的文章，发现了这个。

1.  使用扩展不会减少代码。
2.  通过使用扩展，编译器将认为这是一个正常的函数调用，并尽可能避免优化空情况。
3.  这是 OOP 的一个不好的用法，我们给对象添加方法来“询问”他们一些事情，null 的定义就是缺少一些东西。一个对象从不为空，为空的是对一个对象的引用。

# 小心本地化

对我们大多数人来说，下面的扩展可能看起来很合理。它将双精度值格式化为十进制格式。

这种代码的危险之处在于，在世界范围内，货币的格式是不一样的。在一些国家，你会看到 **1，200.00** (逗号在前，点号在后)，在其他国家 **1，200，00** (点号在前，逗号在后)。如果该应用程序正在/将要在许多国家使用，这只是一个问题。

我的解决方案是将格式化模式添加到`strings.xml`中，并在`Context`上创建一个扩展来处理它。这样，您可以为每种语言定义货币格式，并且仍然使用相同的函数。

# 扩展属性

Kotlin 还允许我们添加属性作为扩展。我还没有找到它们的用例，但是知道有可能使用它们是很有用的。

感谢您的阅读。如果您有任何不同意的地方，请在下面留下评论，这样我们就可以讨论它并找到更好的解决方案。

# 其他文章

[Android 生命周期场景——单个和多个活动](https://victorbrandalise.com/android-lifecycle-scenarios-single-and-multi-activities/)

[安卓生命周期修改](https://victorbrandalise.com/android-lifecycle-revised/)

由 [Fran Jacquier](https://unsplash.com/@fran_?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/lego?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的封面照片

*原载于 2021 年 5 月 13 日 victorbrandalise.com*[](https://victorbrandalise.com/useful-kotlin-extensions-for-android/)**。**