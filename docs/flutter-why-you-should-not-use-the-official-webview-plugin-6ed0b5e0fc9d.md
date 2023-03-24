# Flutter:为什么你不应该使用官方的 WebView 插件

> 原文：<https://medium.com/nerd-for-tech/flutter-why-you-should-not-use-the-official-webview-plugin-6ed0b5e0fc9d?source=collection_archive---------1----------------------->

TL；dr:用`[flutter_inappwebview](https://pub.dev/packages/flutter_inappwebview)`代替。它功能丰富，切换过程只需几分钟。

![](img/ecab12ca1c3f1079f68755455de950ff.png)

我已经开发了几年的 Flutter 应用程序，你们中的一些人可能和我有同样的想法:官方插件更好。好吧，我们错了。我已经放弃了官方的 Flutter WebView 插件，我想你也应该放弃。

3 个月前，我发表了一篇文章:“[Flutter:How make WebView transparent](https://crizantlai.medium.com/flutter-how-to-make-webview-transparent-9e3ce25d8d0)”，效果很好，但这只是一个临时的修正。关于他们是否会合并它，或者是否会增加这个功能，仍然没有官方的回应。

幸好还有另外一个插件:`[flutter_inappwebview](https://pub.dev/packages/flutter_inappwebview)`。切换很简单，只需将`WebView`改为`InAppWebView`，并对参数做一些调整，就大功告成了。以下是一些值得关注的特性:

**设置透明背景**

设置透明背景很简单:

```
InAppWebView(
  initialOptions: InAppWebViewGroupOptions(
    crossPlatform: InAppWebViewOptions(
      transparentBackground: true,
    ),
  ),
);
```

**添加/删除 JavaScript 处理程序**

在官方的 WebView 插件中，你只能通过把 JavaScript 处理程序放到窗口小部件树中来添加它们(T4 参数)，这有点不方便。在`[flutter_inappwebview](https://pub.dev/packages/flutter_inappwebview)`中，一旦有了`InAppWebViewController`实例，就可以在任何地方添加或删除 JavaScript 处理程序，真好！

**访问相机和相册**

据我所知，如果你的网站需要访问相机和相册(例如上传一张图片)，用官方的 WebView 插件是做不到的。但是有了`[flutter_inappwebview](https://pub.dev/packages/flutter_inappwebview)`你就可以了！负责人向[文档](https://pub.dev/packages/flutter_inappwebview)请示。

最后，我真的很感谢`[pichillilorenzo](https://github.com/pichillilorenzo)`创造了这个神奇的插件。大家应该用它而不是颤振队官方出的。

你知道你可以为一篇文章鼓掌 50 次吗？去砸那个按钮！