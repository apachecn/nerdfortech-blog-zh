# 使用原生 SDK 的 Android 本地化

> 原文：<https://medium.com/nerd-for-tech/android-localization-with-native-sdk-610ed6564977?source=collection_archive---------14----------------------->

![](img/5f5a44e06696581267e620dcdbf004c0.png)

# 更轻松、更高效地发布您的翻译

完美的 L10n 会是什么样子？我们如何才能创造出令人敬畏的开发者体验？我们如何管理代码之外的内容，如源代码和翻译？

这些是一些[本地化挑战](https://www.transifex.com/blog/2020/localization-software-development-challenges/)和我们在开始 Transifex 本地化之旅后一直在问自己的问题。今天，我们非常兴奋地宣布发布 Transifex 原生 Android SDK，以加速移动应用程序的移动应用程序本地化！

[Transifex Native](http://transifex.com/native/) 是一个端到端的基于云的本地化堆栈，为持续开发和本地化工作流带来了一个新的范例。其主要组件之一是 [SDK](https://docs.transifex.com/transifex-native-sdk-overview/introduction#framework-specific-sdks) 。2020 年末，我们开始创建一个 [Python SDK](https://docs.transifex.com/python-sdk/quickstart) (也支持 Django)[JavaScript](https://docs.transifex.com/javascript-sdk/quickstart-transifex-native-and-javascript)，以及最近的一个 [iOS SDK](https://docs.transifex.com/ios-sdk/quickstart-transifex-native-and-ios) 。 [Android SDK](https://docs.transifex.com/android-sdk/quickstart-transifex-native-and-android) 基于现有的字符串资源相关方法，如 getString()、getText()提供 Transifex Native 提供的附加功能。

通过使用 Transifex Native，您可以:

*   尽早开始本地化，快速且经常地部署
*   本地化与发展并行
*   易于传递大量(和任意的)上下文
*   拥有单一的全球内容存储库

# Android SDK 的优势

在接下来的段落中，我将讨论 Transifex 原生 Android SDK 在[移动应用本地化](https://www.transifex.com/blog/2021/mobile-app-localization-guide/)方面提供的一些好处。

**更快发布本地化内容，避免麻烦**

首先，Android 库可以通过空中下载(OTA)获取应用程序的翻译，命令行工具可以将应用程序的源字符串上传到 Transifex。SDK 允许你继续使用 Android 提供的相同的字符串方法，比如 getString(int id)，getText(int id)等。

**利用缓存的翻译**

其次，默认情况下，Android 原生 SDK 使用已经缓存在应用程序中的翻译，如果没有找到缓存的翻译，则使用现有的 strings.xml 文件。您可以使用 CLI 工具从开发环境中预先填充缓存，以执行拉取并获取翻译。

除此之外，值得一提的是，您甚至可以使用 SDK 来创建自己的缓存策略。

**提高所有团队的效率**

第三，也是最重要的，有了 Native，所有处理本地化的团队，比如工程师、产品和内容经理，或者设计师，都能够并行工作，消除了任何瓶颈和中间步骤之间的等待时间。

简化本地化工作流程的一个例子是[本地人与 Figma](https://www.transifex.com/blog/2020/everything-connected-transifex-native-figma-%E2%9D%A4-l-f-e/#A_deeper_dive_on_some_workflow_steps) 携手工作。遵循此工作流程意味着可以立即获得所有已经本地化的内容。这是确保内容翻译正确映射的好方法，可以将内容和元数据直接推送到 Transifex 中的项目，等等。

# 您只需要一个单一的本地化平台

通过 Transifex Android SDK 进行本地化为贵公司的本地化提供了额外的好处。

通过使用 Transifex 原生技术，您可以获得本地化的单一来源，并在所有应用程序中利用现有的翻译。这可以是你的安卓应用、iOS 应用，也可以是你的网络应用。

使用 Transifex Native，将您所有应用程序的内容放在您的项目 Transifex 中的一个本地化位置。为您的所有应用获得更一致的翻译、更快的本地化和即时更新！

# 准备好国际化您的 Android 应用程序了吗？

要使用原生 SDK 设置和管理您的 Android 应用程序，请查看 GitHub 上的[详细文档](https://github.com/transifex/transifex-java)。现在就开始享受 [15 天免费试用](https://www.transifex.com/signup/)。

*这个故事由 Panagiotis Papachristofilou 撰写，最初发布在* [*Transifex 博客*](https://www.transifex.com/blog/2021/android-localization-with-native-sdk/) *上。*