# 我在每个项目中使用的 React 本地库

> 原文：<https://medium.com/nerd-for-tech/react-native-libraries-i-use-in-every-project-ed85933880fc?source=collection_archive---------9----------------------->

嗯，几乎在每个项目中。

![](img/ce6230b46eae5bfb08c47299556274ab.png)

由[克里斯·劳顿](https://unsplash.com/@chrislawton?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/books?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

# 介绍

对于一些开发者来说，开发应用程序相当困难。尤其是如果你是一个 web 开发人员，你需要用 Java 或 Swift 编程。但多亏了 React Native，我们不必如此。

[React Native](https://reactnative.dev/) 是一个使用 React 和 Javascript 构建移动应用的框架。我们可以定义 JSX 元素并用与 CSS 非常相似的语法来设计它们，而不是用 XML 来设计样式。设置它也很容易。我们可以轻松地在几分钟内开始创建，而不是编写无尽的文件、类和视图。

但是本机代码和 React 之间的许多联系并不是在 React Native 中建立的。这就是图书馆出现的原因。不用重新创建轮子，我们可以使用 NPM 安装包，就像在普通的 JavaScript 项目中一样使用它们。

# 反应导航

几乎每个手机应用都使用某种导航。不幸的是，这还不是现成的，这就是 [React 导航](https://reactnavigation.org/docs/getting-started/)的用武之地。它使创建新页面和路由变得轻而易举，它甚至支持标签导航。

可能性几乎是无限的。您可以将标签和堆栈导航器结合起来，立即创建具有多屏幕的本机外观的应用程序。

# 矢量图标

我从来没见过一个 app 没有任何图标我觉得。也许如果它只是一个操作系统的默认图标，但仍然有图标。这些图标大部分来自社区或公司的大量收藏，如 Material Icons、Font Awesome 或 IonIcons。

在 React Native 中使用这些图标可能会很麻烦。但是[矢量图标](https://github.com/oblador/react-native-vector-icons)库使得添加图标变得轻而易举。我在标签导航、自定义导航和自定义按钮中使用了这个图标。

# 领域

目前，移动应用程序在本地数据库上的选择非常有限。SQLite 是一个不错的选择，但是编写查询可能是一个安全问题，而且很麻烦。

[Realm](https://docs.mongodb.com/realm-legacy/docs/javascript/latest/) 是 MongoDB 之类的非 SQL 数据库。如果您的数据集足够小，那么它很容易设置和使用。该项目正在慢慢与 MongoDB 本身集成，因此支持将持续一段时间。

我写过一篇关于如何开始使用 Realm 作为移动应用项目的数据库的文章。如果你愿意，你可以在这里查看。

# 反应本地 IAP

现在的 App 大多都是免费的。但这也带来了隐性成本。应用内购买。 [React Native IAP](https://github.com/dooboolab/react-native-iap) 让创建这些产品并将其集成到你的应用中变得简单。我以前在一个应用程序中结合 RevenueCat 使用过它来跟踪收入。

使用 Google Play 或 App Store 进行设置可能需要一些时间，但毕竟，为了获得一点额外的无广告体验或类似的东西，这样做是值得的。

# 使模式化

这个列表中的最后一个包， [Modalize](https://jeremybarbet.github.io/react-native-modalize/#/) 是一个用于创建自然底层的库。这些大多是 iOS 组件，但它也适用于 Android。

我也没怎么用过这个，但我觉得它很适合任何需要底层的应用。用于在付款后显示收据，在需要时提供额外的信息，或者创建更动态的 UI。

# 结论

对于一个普通的 React 项目，我并不真的使用很多包。但我觉得今年会有所改变。我会越来越熟悉上面的库，并且我会在我做的每个项目中学习新的库。

非常感谢您的阅读，祝您有美好的一天。