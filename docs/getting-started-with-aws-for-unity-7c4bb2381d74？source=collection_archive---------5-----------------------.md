# 开始使用 AWS for Unity

> 原文：<https://medium.com/nerd-for-tech/getting-started-with-aws-for-unity-7c4bb2381d74?source=collection_archive---------5----------------------->

![](img/464427ae2d8cb47a82b1cd16028e21b1.png)

要开始使用 AWS for Unity，您可以查阅使用 AWS 的先决条件文档。

 [## 为 Unity 设置 AWS Mobile SDK

### 要开始使用 AWS Mobile SDK for Unity，您可以设置 SDK 并开始构建一个新项目，或者您可以…

docs.aws.amazon.com](https://docs.aws.amazon.com/mobile/sdkforunity/developerguide/setup-unity.html) 

您需要有一个 AWS 帐户，因此您需要登录或按照链接创建您的帐户。

![](img/43586c96831174e2bb1b67c643124e98.png)

登录或创建帐户后，您可以单击链接下载包含使用 AWS 所需软件包的 zip 文件。

![](img/913f0082544e202e53f7ba3b485a5adb.png)

您可以在文件中创建一个新文件夹，并解压缩您下载的 zip 文件。

![](img/b96b962212f65b9b8b49aec5588a2f05.png)

我将使用 AWS SDK . s 3 . 3 . 3 . 113 . 2 . unity package。在您创建的 AWS 文件中找到您想要下载的包，打开并将其导入 unity。

![](img/5bbe7f1a23c293dee035414e5e5968fb.png)

安装好软件包后，创建一个新的名为 AWS_Manager 的空游戏对象。您将向游戏对象附加一个名为 AWS Manager 的新 C#脚本。

![](img/f7e4ace0ff7ed5cf28de98d92910a690.png)

您将打开该脚本并使用 Amazon 名称空间通过 void Awake 方法初始化 AWS。

![](img/ab98008007a1ba1f854a5c445ec8ce4f.png)

最后要做的是使用 Amazon Cognito 建立一个新的身份池。单击文档中的链接开始使用。

![](img/0cbba21bded65b8a28f672e6ccbc2699.png)

单击管理身份池按钮。

![](img/cb7e086f19f02cb0b38d95e68a3237ff.png)

单击创建新的身份池。

![](img/e6f183f514281c6de5c59c8ad6c4d924.png)

您将创建一个名称，并决定是否允许未经验证的帐户访问。

![](img/e49b33077cd382ab7993566a07aa1bdc.png)

创建池并允许未经身份验证和经过身份验证的帐户访问。

![](img/a11d20cec338c00d9f9b4e31acb96e5e.png)

一旦您允许，您就可以开始使用 Unity 中的 AWS 服务。