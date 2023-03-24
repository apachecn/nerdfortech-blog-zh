# Braintree Direct 入门

> 原文：<https://medium.com/nerd-for-tech/getting-started-with-braintree-direct-eff1f7e65756?source=collection_archive---------7----------------------->

Braintree Direct 是处理支付交易的高级 API 之一。它提供了一组客户端和服务器 SDK 来处理请求。

![](img/f2397251aa628429faf20b625eb6af0b.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上由[Towfiqu barb huya](https://unsplash.com/@towfiqu999999?utm_source=medium&utm_medium=referral)拍摄的照片

[Braintree](https://www.braintreepayments.com/) 是一个全栈支付平台。它允许您在应用程序或网站中接受客户的在线支付。使用 Braintree 的好处是，它在一个地方为您提供所有服务，无需单独设置支付网关和商户账户。

布伦特里归贝宝所有。它提供许多产品，如布伦特里直接，布伦特里扩展，布伦特里市场等。每种产品都提供不同的工具和服务。

## 布伦特里直拨

Braintree Direct 由一套接受、验证和处理不同支付方式的工具组成。换句话说，Braintree Direct 由适用于客户端和服务器的软件开发工具包(SDK)组成。

*   **客户端 SDK** 提供了安全收集客户提供的支付信息的工具和技术。
*   **服务器软件开发套件**包含能够安全处理收集的支付信息的工具。

## 嵌入式用户界面

[Drop-in UI](https://developer.paypal.com/braintree/docs/start/drop-in) 是由 Braintree 提供的一个完整且现成的支付 UI，提供了一种安全接受支付的快捷方式。UI 包括一个卡输入表单，如果启用，还包括 PayPal、Venmo、Apple Pay 和 Google Pay 按钮。当用户成功完成 UI 时，您的客户端代码会获得一个*支付方法随机数*供您的服务器使用。

嵌入式用户界面为我们提供了以下优势:

*   **快速简单的集成:**快速将嵌入式 UI 集成到您的应用程序或网站的结账流程中。
*   **用户友好且可定制的 UI:** 定制结帐表单以满足您的需求，并符合您的品牌要求。
*   **接受 PayPal、信用卡等支付方式:**轻松将新的支付方式添加到您的表单中。

## 客户端工作流

我们的客户使用 Braintree SDK 执行的事件工作流程如下所示:

1.  令牌请求已发送到服务器
2.  从服务器收到的客户端令牌
3.  支付信息发送到布伦特里以获得*支付方式随机数*
4.  *从布伦特里收到的支付方式随机数*
5.  *支付方式随机数*发送到服务器

## 服务器工作流

我们的服务器在进行交易时执行的事件工作流如下所示:

1.  从客户端接收客户端令牌请求
2.  生成客户端令牌
3.  向客户端发送客户端令牌
4.  从客户端接收*支付方式随机数*
5.  向布伦特里发送*支付方式随机数*以创建交易

Braintree 是最容易集成的支付网关之一，如果您正在寻找将您的应用程序与支付网关集成，Braintree 将是一个理想的选择。

注意:布伦特里并没有赞助这篇文章，这只是我的技术探索！