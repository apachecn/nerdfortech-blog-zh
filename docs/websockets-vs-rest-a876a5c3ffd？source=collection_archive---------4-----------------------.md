# WebSockets vs REST

> 原文：<https://medium.com/nerd-for-tech/websockets-vs-rest-a876a5c3ffd?source=collection_archive---------4----------------------->

![](img/4b44cec2e3740050cfc1f1a1535fb0d2.png)

每天都有人问我，“WebSockets 和 REST API 之间有什么区别？”在这篇文章中，我试图理清围绕这些技术的术语，并为它们的使用提供一个简单的解释。本文主要面向非技术读者，帮助他们理解 WebSockets 和 REST 哪个更适合他们的数据需求。在我们进入 WebSocket 和 REST API 使用之前，让我们先了解一下它们是什么。

## 什么是 WebSocket？

Websocket 是一种计算机通信协议，或者简单地说是两台计算机相互通信的“一套规则”。它通过 TCP 套接字在两台计算机之间建立双向通信。让我们了解 WebSocket 如何帮助无缝地提供数据。WebSocket 本质上在客户机(请求数据)和服务器(发送数据)之间建立了一个双向连接，这个连接是连续的。一个例子是电话，一旦你接通电话，它就能实现无缝的双向通信。你可以在这里阅读关于 [Websockets 的更详细的解释。](https://tradermade.com/tutorials/socketio-vs-websocket/)

## 什么是 REST API？

REST 是一组约束或设计原则，用于创建使用 HTTP 协议在客户机和服务器之间进行通信的 web 服务，然而，这种通信在任何给定的时间都是单向的。因此，REST API 只根据请求提供数据(或任何资源)。这类似于自动售货机:你需要请求合适的资源来获得一台。

有需要可以免费试用我们的[外汇 WebSockets](https://tradermade.com/market-data/streaming-forex-api) 和 [REST API](https://tradermade.com/market-data/forex-api) 。

## Websocket 有什么用途？

Websocket 主要用于连续(但不总是)和双工数据传输，例如，用于低延迟和高频外汇数据馈送或聊天应用程序。客户端使用 REST API 每秒请求 100 个速率会占用大量资源，而 Websocket 客户端每秒可以接收 100 个速率，而不会占用太多资源。参见 [C# Websocket 教程](https://khanna-rahul.medium.com/your-first-c-websocket-client-5e7acc30681d)。

## REST API 的用途是什么？

REST API 用于数据需求不连续和无状态的情况。例如，在应用程序需要时请求实时数据，或者请求历史时间序列数据来显示图表。REST 是一个基于资源的概念，类似于我们的自动售货机的例子，然而，Amazon.com 是这种类比的更恰当的扩展，在那里你可以在线请求你的资源并让它被交付。

## 什么时候用 Websocket，什么时候用 REST？

Websockets vs REST 其实并不是一个“什么是最好的？”问题，而是“什么更合适？”问题。REST 更加通用，可以交付我们提供的几乎所有可用资源。当数据需求是临时的时，它工作得很好，但是，当数据交付需要超快(比如每秒超过 100 个请求)和连续时，每一点都很重要(对于外汇数据)，它就没有用了。当交付过程中不需要遗漏任何费率时(比如需要进行分笔成交点数据分析时)，WebSocket 是理想的选择，速度是关键。Websockets 非常适合负载较高的场景。

## 结论

WebSockets 和 REST 在技术上都很出色，因此最近很受欢迎。合适的选择取决于用户数据需求。我们希望这有助于提高您对这两种交付方式的理解，但是，如果您仍想了解更多信息，请联系我们或留言。

请分享和鼓掌我们的工作，这有助于我们在决定未来的文章时知道什么类型的内容是值得赞赏的。