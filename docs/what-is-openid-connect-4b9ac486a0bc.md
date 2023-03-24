# 什么是 OpenID Connect？

> 原文：<https://medium.com/nerd-for-tech/what-is-openid-connect-4b9ac486a0bc?source=collection_archive---------9----------------------->

# 描述

OpenID Connect 是由 OpenID 基金会设计和开发的标准。它可以在 OAuth 2.0 规范之上找到([https://tools.ietf.org/html/rfc6749](https://tools.ietf.org/html/rfc6749))。OAuth 2.0 是一个用于访问授权的授权框架。我们还可以调用 OpenID Connect 构建在 OAuth 2.0 之上的身份层。

在典型的 OpenID Connect 登录流中，除了最终用户之外，还涉及到两个主要方，OpenID 提供者和客户端应用程序。

**OpenID 提供者管理；**

*   用户属性和凭证，
*   并允许用户按照 OpenID 连接协议登录到客户端应用程序。
*   这些客户端应用程序和 OpenID 提供者可以属于同一个组织，也可以属于不同的组织。
*   当客户端应用程序来自 OpenID 提供者所属的外部组织时，我们将该客户端应用程序称为第三方客户端应用程序。
*   如果易贝是客户端应用程序，苹果是 OpenID 提供者，当我们用苹果 ID 登录易贝时。他们属于两个不同的组织。我们可以称易贝为第三方客户端应用。

**客户端应用依赖；**

*   对用户进行身份验证的 OpenID 提供者。
*   我们将在示例中使用流行的开源 OpenID 提供程序。
*   随着新版本的发布，我们设置这些工具的方式可能会不时发生变化。
*   我们将从我们的 GitHub 库([https://github.com/openidconnect-in-action/samples](https://github.com/openidconnect-in-action/samples))保留那些 OpenID 提供者的设置步骤。

OpenID Connect 是一种标准，它定义了客户端应用程序如何与 OpenID 提供者通信来识别用户。由 OpenID 基金会开发的 OpenID Connect 核心规范([https://openid.net/specs/openid-connect-core-1_0.html](https://openid.net/specs/openid-connect-core-1_0.html))中定义了客户端应用程序和 OpenID 提供者之间的通信是如何进行的。OpenID 基金会还开发了一些规范来解决 OpenID Connect 的一些其他用例。我们将在深入研究 OpenID Connect 时介绍这些规范。

# OpenID 连接与 OAuth 2.0

OAuth 2.0 框架提供了授权总体模式的细节。但是它没有定义如何实际执行认证。使用 OAuth 的应用程序向第三方系统构建特定的权限请求。它处理身份验证过程，并返回表示成功的访问令牌。IdP 可能需要额外的因素，例如 SMS 或电子邮件，但这完全超出了 OAuth 的范围。最后，默认情况下，访问令牌的内容和结构是未定义的。这种模糊性保证了身份提供者将构建不兼容的系统。

幸运的是， [OpenID Connect](https://openid.net/connect/) 或者 OIDC 给这种疯狂带来了一些理智。这是 OAuth 扩展。它添加并严格定义了一个 ID 标记来返回用户信息。当我们使用身份提供者登录时，它可能会返回我们的应用程序可以预期和处理的特定字段。OIDC 只是 OAuth 的一个特殊和简化的例子，而不是它的替代品。它使用相同的术语和概念。

# 索赔管理

我们总是需要仔细考虑索赔。声明只是嵌入在我们的访问和 ID 令牌中的名称或值对。例如，我们可能有一个 user_id 或 email 声明，以便下游应用程序可以使用它们来创建配置文件或做出决策。[OAuth 主规范没有引入权利要求的概念，甚至没有包括权利要求这个词。这很有帮助，因为我们可以在需要的时候定义它。](https://www.technologiesinindustry4.com/)

OpenID Connect 的使用在很大程度上保护了我们。它解释了对用户详细信息(如姓名、地址等)的一组简单声明。如果我们只使用这些信息，并根据适当的范围限制访问，用户知道他们正在共享什么信息，应用程序知道如何使用这些信息。

我们添加额外的声明，因为例行程序希望有一个用户唯一的标识符。我们使用一个模糊的主键，如果我们考虑它，它在我们的系统之外没有意义。如果我们不小心的话，这可能是一个客户标识符，一个员工号，甚至是一个社会保险号。

更多详情请访问:[https://www . technologiesinindustry 4 . com/2021/01/what-is-OpenID-connect . html](https://www.technologiesinindustry4.com/2021/01/what-is-openid-connect.html)