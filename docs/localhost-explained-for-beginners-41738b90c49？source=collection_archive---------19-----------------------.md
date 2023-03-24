# Localhost 为初学者讲解

> 原文：<https://medium.com/nerd-for-tech/localhost-explained-for-beginners-41738b90c49?source=collection_archive---------19----------------------->

什么是本地主机？为什么叫家？在这篇博文中，我将为编程初学者解释 localhost，这样你很快就会理解它。

![](img/4aee9c02f2c1df18f3e816cd881a0e1f.png)

马文·迈耶在 [Unsplash](https://unsplash.com/s/photos/network?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

# Localhost 让初学者的生活更轻松

让我们从一个例子开始:当你编写一个服务器应用程序，并且你还没有运行远程服务器，你可能会问自己:“*我如何测试我的服务器应用程序？*”。这就是 localhost 出手相救的时刻，因为——令人惊讶的是——您还不需要远程服务器来测试您的应用程序。

简单来说，服务器就是一台计算机，就像你拥有的那台一样。有区别(专门的 CPU，可能运行的是 Linux 而不是 Windows，优化的程序等。)，但总体来说只是一台电脑。因此，您的计算机也可以充当服务器，并与自身建立连接。

# Localhost 是如何工作的？

当您尝试呼叫网站时，您的电脑会尝试连接到互联网上某个位置的远程电脑(也称为服务器)。它发送一个请求，结果收到一个包含一些内容的响应。

当您调用 localhost(或 127.0.0.1)时，您向自己的设备发送一个请求。您的计算机识别该地址(localhost 或 127.0.0.1 ),它不是通过互联网发送出去，而是通过环回机制返回请求。记住 server 应用程序示例，现在您的服务器(在同一台机器上)可以接收请求并发送包含一些内容的响应。

请求/响应永远不会离开您的计算机。换句话说:通过调用 localhost，你的计算机总是与自己对话。

这样，您就可以测试尚未准备好投入生产使用的应用程序。这不是很好吗？

现在，您应该对使用 localhost 有了很好的理解，甚至能够向其他编程初学者解释它。

# Localhost 和 127.0.0.1 的区别

(几乎)没有——只是键入 localhost 比 127.0.0.1 更舒服，但从技术上来说(几乎)是一样的。

为什么差不多？因为 127.0.0.1 是用 IPv4(互联网协议第 4 版)写 IP 地址的方式。有了 IPv6(互联网协议版本 6)，也可以用::1 代替 localhost。所以从技术上来说，说 localhost，127.0.0.1 和::1 指的是同一个概念会更正确。

现在你有希望理解这句谚语了，没有什么感觉像 127.0.0.1。

提示:家

感谢阅读！

*原载于 2021 年 5 月 22 日 https://codingflashlight.com*[](https://codingflashlight.com/localhost-explained-for-beginners/)**。**