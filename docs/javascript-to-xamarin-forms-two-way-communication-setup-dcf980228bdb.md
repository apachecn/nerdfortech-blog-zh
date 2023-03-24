# JavaScript 到 Xamarin。形成双向通信设置

> 原文：<https://medium.com/nerd-for-tech/javascript-to-xamarin-forms-two-way-communication-setup-dcf980228bdb?source=collection_archive---------0----------------------->

## 如何设置 WebView JavaScript 和 Xamarin 之间的通信？表单应用程序

大家好👋！！！在今年切换到毛伊岛之前，让我们总结一个不典型的案例——通过 WebView 与 JavaScript 的通信。这篇博文将向您展示如何为 Android 和 iOS 设置 JS 到 XF 和 XF 到 JS 的命令。

对于我们的例子，我们将发送一些信息并显示一个延迟的警报。并对 Xamarin 应用程序中的一些 HTML 按钮做出反应。

# 准备 WebView

首先，我们需要创建一个从 WebView 继承的新的自定义控件。此外，它将包含一个函数来调用预定义的 JS 函数和动作，WebView 将从 JS 调用这些函数和动作。

CustomWebView 可以通过以下方式在 Popup 或 Page 后面的代码中使用它。

# iOS 上的 JavaScript 绑定

对于 iOS 足以创建一个渲染器；姑且称之为`CustomWebViewRenderer`。它应该继承自`WkWebViewRenderer`并实现`IWKScriptMessageHandler`的一个接口。此外，它应该包含我们的 JS 函数`InvokeDisplayJSText`的定义和我们的脚本消息处理器的注册。是的，iOS 不需要这么多。以下是完整的渲染器代码:

# Android 上的 JavaScript 绑定

对于 Android 来说稍微复杂一些，但是实现也是从创建一个渲染器开始的，姑且给它起个相同的名字`CustomWebViewRenderer`。应该是继承了`WebViewRenderer`。此外，它应该包含我们的 JS 函数`InvokeDisplayJSText`(类似于 iOS)的定义和我们的自定义`JSBridge`和`JavascriptWebViewClient`的注册。

`JavascriptWebViewClient`的定义非常简单——它只需要在 WebView 加载页面时加载定义好的 JS 脚本。

`JSBridge`的定义不会更复杂。我们需要导出 JS 代码定义的同名 JS 接口。

现在我们正在加载 JS 并通过 JS 桥执行 Xamarin 代码🙃