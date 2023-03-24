# Matt 的 103 号趣闻——从 URL 启动应用程序

> 原文：<https://medium.com/nerd-for-tech/matts-tidbits-103-launching-an-app-from-a-url-c27da66cd52c?source=collection_archive---------4----------------------->

![](img/38aac84142469fb3e19295382d341213.png)

本周的花絮将向你展示当用户点击一个链接时如何启动一个应用程序。[上一次，我写了关于调试 Android 中的一个视图绑定问题。](/nerd-for-tech/matts-tidbits-102-debugging-a-perplexing-view-binding-issue-69d2bb81999c)

当你点击电子邮件、网页等中的链接时，你的移动设备上有应用程序启动吗？想知道如何为你的应用程序做同样的事情？不要再想了，因为在这个简单的指南中，我将向你展示如何做到这一点！

# 机器人

在 Android 上，这可以通过在应用程序的`AndroidManifest.xml`中定义一个`<intent-filter>`来实现。

这里有一个简单的例子:

```
<intent-filter>
  <action android:name="android.intent.action.VIEW" />
  <category android:name="android.intent.category.DEFAULT" />
  <category android:name="android.intent.category.BROWSABLE" />
  <data 
    android:scheme="https"
    android:host="[matthew-b-groves.medium.com](https://matthew-b-groves.medium.com)"
  >
</intent-filter>
```

这样做会导致任何以`https://matthew-b-groves.medium.com`(我的媒体简介的 URL)开头的链接启动这个应用程序。

您甚至可以定义多个`<data>`清单，以允许多个 URL 模式调用您的应用程序。*(请注意，您定义的任何可能的方案/主机组合都将匹配——甚至跨* `*<data>*` *条目)*

根据您想要的行为，您甚至可以为`android:scheme`使用完全定制的东西，比如`mattstidbits`，并完全省略`android:host`属性。这将允许*任何以`mattstidbits://`开头的* URL 启动你的应用。但是，这只有在用户安装了您的应用程序的情况下才有效。如果用户没有安装你的应用程序，你想引导他们到 Play Store 列表，你需要使用一个`http`或`https`方案和一个你自己的主机/域。然后，你可以按照这些指令来创建一个 JSON 文件，你可以把它嵌入到你的网站上，它会指示浏览器把用户重定向到你的应用的 Play Store 列表页面。

如果您想让您的应用程序访问嵌入在 URL 中的信息(这样您就可以将它们带到应用程序中的特定屏幕)，您需要将类似下面的内容添加到您的应用程序的`onCreate`方法中:

```
val action: String? = intent?.action
val uri: Uri? = intent?.data
```

然后，您可以验证动作是`Intent.ACTION_VIEW`并获取传入的`uri`并使用`Uri`类的方法/属性从 URI 中提取信息来帮助您处理请求。

测试您的深度链接也是一个好主意，您可以使用`adb`命令在模拟器或物理设备上进行测试，如下所示:

```
adb shell am start -d "*your-deep-link-url*" 
```

实现深度链接的 Android 开发者文档确实很优秀，可以在这里找到:[https://developer . Android . com/training/app-links/deep-linking](https://developer.android.com/training/app-links/deep-linking)

# ios

*免责声明:我不是 iOS 开发人员，所以我无法提供我能为 Android 提供的细节。*

苹果似乎提供了两种不同的机制来从 URL 启动应用程序——URL 方案和通用链接。

*   [URL 方案](https://developer.apple.com/documentation/xcode/defining-a-custom-url-scheme-for-your-app)实现起来更容易/更快，但是提示用户许可，如果应用没有安装就不起作用。
*   [通用链接](https://developer.apple.com/ios/universal-links/)要求你控制域名(很像我上面描述的谷歌应用链接)，所以它们需要更多的工作来设置，但如果用户没有安装应用，它们不会提示用户并允许你提供一个备用 URL。

这篇文章很好地解释了其中的一些区别:[https://medium . com/wolox/IOs-deep-linking-URL-scheme-vs-universal-links-50 Abd 3802 f97](/wolox/ios-deep-linking-url-scheme-vs-universal-links-50abd3802f97)

现在，让我们把重点放在 URL 方案上，因为这是我以前用过的方法。要设置一个，您只需通过项目设置中的“信息”标签在 Xcode 中指定方案。请注意，与 Android 不同，iOS 只允许您定义方案(URL 中位于`://`之前的部分)。因此，您可以在“URL Schemes”字段中输入`mattstidbits`，这样任何以`mattstidbits://`开头的链接都会启动您的应用程序(如果安装了的话)。

在您的`Info.plist`文件中，这显示在`CFBundleURLSchemes`键下。

如果你想让你的应用程序访问存储在 URL 中的信息，那么你需要在你的应用程序委托的`application()`方法中处理它。有一个包含数据的`url`参数。

与 Android 一样，您可以通过运行以下终端命令来测试这些深层链接:

```
xcrun simctl openurl booted "*your-deep-link-url*"
```

(还是那句话，我不是假装 iOS 开发者，具体怎么做请看[官方文档](https://developer.apple.com/documentation/xcode/defining-a-custom-url-scheme-for-your-app)。)

# 反应自然

如果你已经关注我的花絮有一段时间了，你可能想知道我是否会触及这一点——以下是如何在 React Native 中做到这一点！

React Native 有两种不同的设置——如果你使用 Expo，按照这些指令指定你想在 Expo 的 JSON 配置中使用的方案。

然而，如果你没有使用 Expo，你通常需要按照上面 iOS/Android 的相同步骤在你的`AndroidManifest.xml`和`Info.plist`中设置深度链接。要在 React Native CLI 项目中实现这一点，您应该按照这些说明[来配置您的项目。请注意，对于 iOS，您需要向 AppDelegate 添加一些特殊代码来实现这一点。](https://reactnavigation.org/docs/deep-linking/#set-up-with-bare-react-native-projects)

请注意，这两个选项都假设您使用 React 导航来管理应用程序中的导航(我强烈建议您这样做！)

如果你想用这个 URL 做一些事情(而不仅仅是调用这个应用程序)，你需要配置一个`linking`对象并把它分配给你的`NavigationContainer`。

`linking`对象有点古怪，因为它就像你的导航图的副本。它使您能够将 URL 参数映射到内部路线名称/路径，但要求您精确匹配应用程序的导航图。

例如，您可以按如下方式进行设置:

```
const config = {
  screens: {
    Tidbit: 'post/:id',
    Profile: 'user',
  },
};

const linking = {
  prefixes: ['https://matthew-b-groves.medium.com'],
  config,
};

function App() {
  return (
    <NavigationContainer linking={linking}>
      <Stack.Navigator>
        <Stack.Screen name="Tidbit" component={PostDetailScreen} />
        <Stack.Screen name="Profile" component={ProfileScreen} />
      </Stack.Navigator>
    </NavigationContainer>
  );
}
```

如果你有嵌套的导航器(你很可能会这样做)，你的`linking`对象也需要匹配这个嵌套，这会变得有点乏味。

更多详情，见[本文](https://reactnavigation.org/docs/configuring-links)。

要测试 Expo 项目的链接，运行下面的命令(如果您想改变目标平台，请用下面的`ios`代替`android`):

```
npx uri-scheme open "*your-deep-link-url*://127.0.0.1:19000" --android
```

对于一个非 Expo 项目，您可以通过上面部分描述的本地 iOS/Android 命令来测试深度链接。

# 总体战略

我们能从中学到什么？

*   iOS/Android/React Native 上的深度链接提供了复杂的功能。
*   几乎在所有情况下，出于一致性的考虑，您可能希望您的链接在 iOS/Android 上具有相同的格式(让链接在两个平台上无缝工作)
*   我建议从最简单的方法开始，然后从那里开始——为了使用更复杂的应用程序链接(Android)或通用链接(iOS ),可能会有很多繁文缛节需要处理，以便在你公司/项目的网站上更改文件，所以如果你能够使用更基本的深度链接/URL 方案(不需要修改网站)，那么就从那里开始。

有兴趣和我一起在埃森哲出色的数字产品团队工作吗？我们有一个机会:

*   [移动开发者(东北地区)](https://www.accenture.com/us-en/careers/jobdetails?id=00960583_en&title=Mobile+Developer)

你还有其他深层链接的技巧想要分享吗？请在下面的评论中告诉我！