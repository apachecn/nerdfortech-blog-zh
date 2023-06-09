# REST 和 SOAP APIs 之间的区别

> 原文：<https://medium.com/nerd-for-tech/differences-between-rest-and-soap-apis-d8ee2326ad3b?source=collection_archive---------29----------------------->

![](img/7dd890408c0d761d99e03db05b40dca7.png)

在编程世界的早期，一切都是以整体的方式开发的——前端、中间层和数据库层使用的代码都放在一个解决方案中。

随着时间的推移，场景发生了变化，业务逻辑和数据库连接从前端代码中分离出来。这提供了相当多的优势

*   UI 线程不再因后台活动的完成而被阻塞
*   用不同语言编写的程序可以交流
*   前端应用程序变得更轻，这使得加载应用程序更容易，并为用户提供了更好的体验
*   如果有多个前端应用程序需要处理相同的数据，那么可以很容易地引用它们。

很快，开发人员意识到将这些部分连接起来以进行适当通信的重要性。进入—基于 SOAP 和 REST 的 web API 服务。

SOAP 和基于 REST 的 web API 服务都可以在。然而，当你可以在两者之间做出选择时，我们大多数人都不确定该选择哪一个。

如果你是那些不确定选择哪一个的人之一，那你就来对地方了。这篇博客区分了 Rest 和 SOAP APIs，并介绍了它们各自的优缺点。

**目录**

1.  SOAP 和 REST Web API 服务介绍，
2.  REST 和 SOAP APIs 之间的区别
3.  什么时候使用 REST？
4.  什么时候用肥皂？
5.  肥皂和 REST 有什么弊端？
6.  结论

# SOAP 和 REST Web API 服务简介

SOAP 代表简单对象访问协议，是 1998 年开发的网络通信协议。SOAP 处理通信和生成响应的内置功能使得独立于平台/语言成为可能。

它支持 XML 格式的数据传输，并严格遵循消息传递结构、编码规则以及请求和响应过程。使用 XML 的优势在于它既是人类可读的，也是机器可读的。

SOAP 消息结构由信封、报头、主体和错误组成。

*   信封是包含消息的所有标签的块。
*   报头是用于发送认证数据的可选字段。
*   主体部分包括请求\响应。
*   Fault 也是一个可选部分，它保存请求/响应中发生的错误。

REST 代表表述性状态转移，用于 web 通信，不需要安装任何库。它可以用来传输媒体文件以及数据。任何遵循 rest 原则的 web 服务都被称为 Restful web 服务。典型的 Restful web 服务会使用 HTTP 动词，比如 GET、PUT、POST 和 Delete。

REST 可以在任何协议之上实现。使用基于 REST 的服务的一个关键特征是通信的格式；数据以 JSON 消息的格式发送，因为它们相对较小。

# REST 和 SOAP APIs 之间的区别

让我们来看看它们之间的一些关键区别-

## 实现方式

SOAP 是一种协议，它有一个定义好的标准。它由一个 WSDL 文件组成，其中包含处理服务请求所需的信息。

另一方面，REST 都是关于架构风格的。如前所述，它可以在任何协议之上被提及。当应用程序遵循客户端服务器、可缓存、无状态、分层系统等约束时，服务就变成 RESTful 了。

## 安全性

SOAP 具有 SSL(安全套接字层)和 WS-Security，因此安全性很高，用于银行交易。

REST 使用 SSL 和 HTTPS 来保证安全性。

## 资源和带宽要求

SOAP 需要大量的资源和带宽，因为它必须将代码转换成 XML，这增加了有效载荷的大小。

REST 使用多种标准，因此需要较少的资源和带宽。

# 什么时候使用 REST？

在以下情况下可以使用 REST

*   根据 REST 的特性，您可以清楚地看到它可以在网络带宽受限的领域实现。它只使用较少的资源和带宽。
*   每当不需要维护请求之间的关系(即状态)时，可以使用 REST 方法，因为它们是无状态的。
*   如果用户/客户端系统多次请求相同的信息，并且该信息是静态的，那么可以使用 REST，因为它具有缓存特性。
*   要快速提出并部署解决方案，可以使用 REST，因为它易于实现和编码。

# 什么时候用肥皂？

肥皂可以在下列情况下使用-

*   如果需要在请求之间传输状态，那么最好使用 SOAP 方法，因为它可以在请求之间传输状态。
*   如果请求/响应需要安全地传输，并且不能接受任何数据泄漏，那么应该实现 SOAP。
*   如果客户机和服务器在定义要传输的通信类型时遵循严格的握手，那么应该使用 SOAP。

# 肥皂和 REST 有什么弊端？

由于 SOAP 使用 WSDL 进行通信，所以通过 SOAP web 服务进行通信的客户端在构建代码时必须参考 WSDL 文档。每当 SOAP web 服务发生变化时，WSDL 文档就会自动更新以适应这些更新。因此，作为一名开发人员，有必要参考更新的 WSDL 文件的所有时间，这是一个繁琐的过程。

另一方面，缺少额外的安全特性和无法维持状态被认为是主要的缺点。

# 结论

既然您已经理解了 REST 和 SOAP APIs 的主要区别和缺点，那么您可能会更加清楚为您的应用程序选择哪种 API。所以明智地选择，挑选一个最符合你需求的。

*原载于*[*https://www . partech . nl*](https://www.partech.nl/nl/publicaties/2021/05/differences-between-rest-and-soap-apis)*。*