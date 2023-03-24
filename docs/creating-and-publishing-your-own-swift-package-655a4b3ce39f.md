# 创建并发布您自己的 Swift 包。

> 原文：<https://medium.com/nerd-for-tech/creating-and-publishing-your-own-swift-package-655a4b3ce39f?source=collection_archive---------2----------------------->

![](img/e4849b4b0a7abab973dc87a873787a47.png)

由 [Unsplash](https://unsplash.com/s/photos/box?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的[品牌盒子](https://unsplash.com/@brandablebox?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍摄

Swift Package Manager(也称为 SPM)已经存在了很长时间。这是苹果公司的一个非常酷的功能，允许我们以非常有效的方式管理、添加和创建依赖关系。

SPM 与 Swift 构建系统相集成，可自动执行下载、编译和链接依赖关系的流程。我们已经有了像 Cocoa Pods 和 Carthage 这样受欢迎的包管理器，但是在本文中，我们将了解 SPM 有多酷。

假设你在一个团队中工作，你的团队想要一个包含应用程序主要功能的独立私有框架。
因此，你可以单独创建一个只需要导入就可以运行的依赖项/框架，而不是每次都管理应用程序中的类/代码并来回操作。
你也可以为社区创建一个独立的公共依赖，与他们分享酷的东西。

![](img/692ae02099f22922deef38cd08395df1.png)

事不宜迟，让我们开始吧！！:D

首先，我们需要创建一个 Xcode 项目。创建 Xcode 项目后，让我们向我们的项目添加一个新的 Swift 包管理器。

![](img/41b1f5b02511fcbbed7c8902677403dc.png)

在您将新的 Swift 包添加到您的项目后，您现在将拥有这些文件。现在让我们来看一下这些文件。

![](img/96bc4ed260f84353b49ee2852d30beba.png)

1.  ReadMe.md:这里是您记录软件包的地方。
2.  Package.swift 包含您的包的所有配置。比如外部依赖、你的目标等等。
    我们不会向我们的产品包添加任何外部依赖，因此我们现在就可以继续前进。

![](img/fca14825977c9fae65c7256dc5c0b299.png)

这是您的 Package.swift 文件。

![](img/54b9dfdb174ed4e750a1a39071ef4b93.png)

我们必须确保 sources 文件夹下的文件夹和目标名称是相同的。这里是 APIManagerPackage！

现在是时候在 ApiManagerPacakge 文件中添加您的代码了，在您的情况下，您可以有一个不同的文件名。如果你正在跟进，你可以使用类似的代码。所以我们的 APIManager 已经准备好了。现在下一步。

现在转到目标->通用->框架库和嵌入式内容，点击+按钮。

![](img/90724102a52109ff776f126022862f6e.png)![](img/f0d4c9e315ac9bd696b67b0c248ddafd.png)

这是我们的**新包装。**添加它:D

我们已经完成创建我们的包，让我们检查它是否工作正常。

转到任何类并尝试导入 **APIManagerPackage。**

![](img/b87617dd0e85c66bddc3ae47f87d696d.png)

现在你可以在你的代码库中使用我们定制的包了。就像这样。

![](img/26f9f8e3ddb95c0b854d8c10afa4b0f4.png)

如上所述，让我们发布我们的包。
为了发布，您需要做的第一步是将您的软件包管理器复制到任何其他目录中，以便我们可以单独打开它。我把我的拷贝到了桌面上。你可以抄你喜欢的任何地方。使用 Xcode 打开您的 SPM。这就是它看起来的样子。

![](img/3f180b2eef9616f2c30b0e177afb45ad.png)

现在转到源代码控制并单击 New git Repository。

![](img/871654f258ddf13a432a72fa643724d4.png)![](img/17db24f8bde5ad17f0c86e379c9ccb88.png)

您将看到这个窗口，确保选中您的复选标记，然后按“创建”。之后，转到 Xcode 的左下方，你会看到一个设置图标。选择新的“APIManagerPackage”遥控器。

![](img/065877e34b2c6ba3ec50584e57aa8213.png)

只要你点击你就会得到这个信息窗口。就我而言，我在这里有我的 GitHub 回购设置。
您可以通过 Xcode- >偏好设置- >帐户将帐户链接到您的 Xcode。
所以现在我们可以为我们的包创建一个远程，在我的情况下，我会做一个公共回购，因为我会继续工作；)!

![](img/6c978614f906ec2c7929cc59b9dfad96.png)

在您创建了一个新的存储库之后，您必须像这样向您的分支添加一个标记。您可以在 SPM 上的 wwdc 2019 会议上了解更多关于如何标记您的分支机构的信息。

![](img/7ad036d9e4a7adb71ca7fc36295560a0.png)![](img/16ed33e8735e6bf912d82e82ce037225.png)

现在，我们的本地回购协议已创建，分支标记已设置，现在是我们的最后一步。
将我们的代码推送到 GitHub :D

![](img/059e2922800089ae39bc5e3687104867.png)

一旦你选择了推送，你的包就会被推送，并且会出现在 GitHub 上！

我们去看看。

![](img/36a7e1b19d6a5b226ad81f34cfef1daa.png)![](img/32bced5351771470e5a2d8c1215c8e42.png)[](https://github.com/shazy112/APIManagerPackage) [## shazy112/APIManagerPackage

### 通过在 GitHub 上创建一个帐户，为 shazy112/APIManagerPackage 开发做出贡献。

github.com](https://github.com/shazy112/APIManagerPackage) 

我们的套餐是:D 直播！！！

![](img/84dba2d0dfa082317c8b5cf777a84594.png)

现在让我们检查一下我们是否可以将这个 Swift 包添加到我们的项目中？！！

![](img/5dcd3a86224f3c986b40b2f27981b2ea.png)

在 URL 部分粘贴 URL，您将能够看到您的新包。

![](img/c0ee08d0c52170edb15502f63d019636.png)![](img/0528e8b27af7bfd0c6d014812dd3223b.png)![](img/6174697e5ecc4e8d692f1af37b59a025.png)![](img/1c58c30b56eb1848c8c54893ed0a764f.png)

这里是我们从 Github 添加了新的包管理器！！

本文到此为止，希望对大家有所帮助。如果你们有任何问题，请告诉我。谢谢！

快乐编码。

希拉兹·艾哈迈德！