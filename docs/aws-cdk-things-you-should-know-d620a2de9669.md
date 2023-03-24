# AWS CDK:你应该知道的事情

> 原文：<https://medium.com/nerd-for-tech/aws-cdk-things-you-should-know-d620a2de9669?source=collection_archive---------9----------------------->

![](img/2211fb29fec3727861d7dba06d6f2b89.png)

如果你正在使用 AWS，你应该已经开始使用或者至少听说过 CDK；这是他们的基础设施，作为市场上其他参与者的代码替代，包括他们自己的基于 yaml 的“CloudFormation”。

虽然，我在 CDK 的经历一直很愉快，但有几件事我很想早点知道。有些是我在没有阅读每一篇文档的情况下采用某项技术时遗漏的；而有些是可能没有明确说明的缺失元素。

# 在当地与 CDK 合作

像大多数 IAC 工具一样，AWS CDK 也有一个命令行实用程序，你可以用它在 AWS 上执行一些操作，包括引导一个帐户，合成一个 CloudFormation 模板等等。

如果您所在的组织已经启用了 SSO，那么您可能需要执行一些额外的步骤来实现这一点。我说的问题是 [AWS SSO 命名配置文件支持](https://github.com/aws/aws-cdk/issues/5455)。在这一点上，社区已经有了多种解决方法，这实际上不是 CDK 的问题；但是 JS SDK 直到几天前还不支持 SSO。希望这一个成为过去的事情，一旦变化滚滚而来。

我最终使用了其中一个解决方案[这里](https://www.matscloud.com/blog/2020/06/25/how-to-use-aws-cdk-with-aws-sso-profiles/)。

# AWS 服务的命名约定

熟悉 AWS 服务使用的命名约定。或多或少围绕着“大写名字允许吗？”。我遇到了一些问题，我计算的带有项目前缀的服务名有大写字符，而 CDK 悄悄地把它们转换成了小写。

*   DB 子网组只能有小写名称。
*   如果您将一个数据库集群与子网组相关联，CloudFormation 控制台将继续失败，并显示消息“<name>不存在”。</name>

**注意**:我后来注意到 AWS 控制台的行为也是如此。

# 自动气象站/ CDK/运行时

如果您完全不熟悉 AWS，并且正在努力掌握您不熟悉的 CDK 和运行时；以下是我的建议:

*   开始关注你将要使用的 AWS 服务。
*   通过在 AWS 控制台中使用它来理解服务的细微差别。
*   除非你知道它的存在，否则你不会知道在 CDK 该怎么做。
*   文档和示例在以后会更有意义

# Python CDK 文档

python CDK 文档有一些印刷错误。有几个来自 typescript 的例子和一些缺失的更新。如果您使用的是 VSCode 或 PyCharm 之类的 IDE，**查找 AWS 工具包扩展。**

[](https://aws.amazon.com/pycharm/) [## PyCharm 的 AWS 工具包

### AWS Toolkit for PyCharm 是 PyCharm IDE 的一个开源插件，它使创建、调试和管理变得更加容易

aws.amazon.com](https://aws.amazon.com/pycharm/) [](https://aws.amazon.com/visualstudiocode/) [## AWS Visual Studio 代码工具包

### AWS Toolkit for Visual Studio Code 是一个针对 Visual Studio 代码的开源插件，它将使……

aws.amazon.com](https://aws.amazon.com/visualstudiocode/) 

# **CDK 管道:上下文查找**

如果您计划在 CICD 流程中使用 CDK 管道，您应该意识到这一点。您不会遇到这种限制，直到您进入一些需要 VPC 的资源，如 Aurora 无服务器集群。

**参考**:[https://docs . AWS . Amazon . com/CDK/API/latest/docs/pipelines-readme . html # current-limits](https://docs.aws.amazon.com/cdk/api/latest/docs/pipelines-readme.html#current-limitations)

**你能为 VPC 局势做些什么**:

*   当您创建管道并描绘出您感兴趣的部署环境时，记下客户 id 和区域。
*   对所有环境组合执行 **cdk synth** 。如果需要，在合成时将 vpcId 指定为上下文变量。
*   这将创建一个“context.cdk.json ”,其中包含您的环境的网络/vpc 相关信息。
*   将它们提交给代码存储库，并从中获取 vpcid。

# CDK 版本

关于 AWS CDK，我必须承认的一件令人担忧的事情是次要版本的变化率。**不要误解我的意思，引入重大修复和功能的改变受到高度赞赏**。而且，我还没有被变化咬得很厉害。这类似于我在使用 istio 作为服务网格选项时注意到的变化。

这表明对您为应用程序创建的结构进行良好测试的必要性。如果您正在为您的组织创建可重用的通用构造，请进行一些测试，以验证新版本的更改没有破坏您这边的某些东西。

# CDK 松弛水道

如果你已经阅读了这篇文章，看起来你对使用或学习 CDK 很感兴趣。没有比他们的松散渠道更好的地方来与志同道合的人建立网络，这些人已经接受了 CDK 作为他们前进的道路。我有过无数次互动，其中有人向我介绍了挑战和我的问题的可能解决方案。所以[如果你还没有加入这里](https://cdk.dev/)。

**参考文献:**

1.  汉斯-彼得·高斯特在 [Unsplash](https://unsplash.com/s/photos/learning?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片