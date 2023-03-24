# 用 React Native Firebase 设置 Google Tag Manager

> 原文：<https://medium.com/nerd-for-tech/setup-google-tag-manager-with-react-native-firebase-84bfc814d00c?source=collection_archive---------3----------------------->

![](img/eee85e5dadf08f644385c642aaa3a52b.png)

在这篇文章中，我将向您提供一些与 [Google Tag Manager](https://marketingplatform.google.com/about/tag-manager/) 和 [React Native Firebase](https://rnfirebase.io/) 库集成的退订瞬间。

当您已经在项目中设置和使用 Firebase Analytics 时，这篇文章将会非常有用。什么是谷歌标签管理器(GTM)以及为什么它必须集成到您的项目中，您可以在这里找到一些信息[。](/bam-tech/how-to-install-google-analytics-for-react-native-80134383e39d)

设置 GTM 配置的步骤在官方网站上有完整的描述，你也可以在其他地方找到更多的信息，包括这里的媒体。但是，不幸的是，在我的设置过程中，我没有找到关于 GTM 与 React Native 集成的完整解释以及一些故障排除的瞬间。我花了几乎一天的时间弄清楚 ta 如何让 Android 平台与 GTM 一起工作。提供安装步骤不适合我，所以我尝试了不同的方法，直到找到一个工作。

我假设你的项目在两个平台上工作，IOS 和 Android。所以我很快会谈到这两个问题。请再次确认你没有这篇[文章](https://proandroiddev.com/firebase-with-gtm-events-not-showing-on-google-analytics-c57a550fdfba)中描述的任何问题，并且你的所有准备步骤都已就绪。

# IOS 设置

官方页面上描述的步骤是将 GTM 集成到你的项目中的工作变体，以防你已经使用了 firebase。除了上面列出的步骤之外，没有特别的步骤。您只需在项目根目录下创建文件夹“容器”，将导出的容器配置放在那里，然后通过菜单“文件->将文件添加到{您的项目}”将该文件夹添加到 XCode 应用程序中。重要的是要为“做参考”或其他东西打上一个复选框。现在，你必须在你的 Podfile 中添加最后一个 end '**pod ' Google tag manager**'，并且不要忘记在 ios 文件夹下做' **pod install** '。

现在，要使用 firebase analytics 测试您的容器，请转到您的 firebase 项目并检查 DebugView 部分。

```
import analytics from '@react-native-firebase/analytics';...analytics().logEvent('YOUR_EVENT_NAME', params);
```

如果一切正常，除了你的事件名称在你连接的设备的日志列表中，你还可以看到来自 GTM 的事件，这取决于你在容器中的设置。在我的例子中，我只是添加了一个新标签‘IOs _ test _ GTM’，由发送到 firebase analytics 的每个事件触发。

# Android 设置

如前所述，GTM [docs](https://developers.google.com/tag-manager/android/v5) 中描述的 Android 集成步骤对我不起作用。我认为，原因是所描述的步骤是针对原生 Android 集成的，而不是针对 react-native firebase 库的，后者负责将事件发送到您的 firebase 分析帐户。为了使集成工作，他们要求开发人员在 **build.gradle** 文件中添加下一个依赖项

```
dependencies {
  // ...
  compile 'com.google.android.gms:play-services-tagmanager:11.0.4'
}
```

在多次尝试和更改配置后，我发现我必须添加以下用法来代替:

```
dependencies {
  // ...
  implementation 'com.google.android.gms:play-services-tagmanager:17.0.0'
}
```

只有在那之后，我才能在 firebase analytics 的 debugView 中看到我的 GTM 标签。

# 摘要

差不多就这些了，伙计们！我希望，这篇短文能帮助你节省时间，并把它奉献给一些更愉快的员工。在新的或已经交付的 react-native 基础项目上设置 GTM 是相当容易的，只要所有步骤都正确通过，并且信息符合您的上下文和环境。