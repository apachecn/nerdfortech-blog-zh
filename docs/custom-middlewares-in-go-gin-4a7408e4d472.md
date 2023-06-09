# Go gin 项目中的自定义中间件

> 原文：<https://medium.com/nerd-for-tech/custom-middlewares-in-go-gin-4a7408e4d472?source=collection_archive---------0----------------------->

![](img/c96990948c81ee4940f47c06d4af10cc.png)

在我的一个副业项目中，我探索了一种机制来移动需要为每个传入的 API 调用执行的公共功能，如身份验证、令牌、会话等。Go gin 框架中的中间件是解决这一问题的技术。如果我把中间件比作 JAVA 中类似的东西，那就是拦截器。拦截器的工作是拦截每一个来电并执行步骤。中间件完全一样。

我发现这个回购协议收集了一些令人惊讶的中间产品。如果你需要任何可用的，只需连接这个回购，并开始使用它。

但是这篇文章致力于为你的特定用例创建一个定制的中间件。让我们从这个开始。

# 目的

我的中间件有一个简单的双重目的:
1 .为每个传入的请求添加一个 uuid。这个 uuid 将用于打印处理程序流中的日志，从而使请求变得唯一，并简化可跟踪性和可调试性。
2。捕获服务请求所花费的时间。

**给我看看代码**

让我们跳到代码。我把我在以前的帖子中提到的应用程序的代码片段放在这里。

**原始 application.go 文件**

现在在文件中添加中间件。只需添加如下代码。

**applicationUpdated.go 文件**

观察添加 **guidMiddleware()** 函数并使用**路由器嵌入它的简单性。使用()**方法。中间件函数有一个**返回类型 gin。手柄功能**。

就像您可以添加尽可能多的中间件并将其嵌入到 gin 路由器中一样。就是这样！这就是在你的 gin 应用程序中添加一个定制中间件是多么容易。让我知道你是否面临任何问题，发现任何差异。