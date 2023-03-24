# Flutter:将 HTTP 响应解析成对象

> 原文：<https://medium.com/nerd-for-tech/flutter-parsing-http-responses-into-objects-c51aaf410b73?source=collection_archive---------15----------------------->

![](img/eff6f99155a46fde94ab71b5c9a2e497.png)

ost 应用不在本地工作，它们通过 HTTP 请求向/从 API 发送/接收数据。通过用 JSON/XML 格式化数据来与服务器通信是一种常见的做法。以下是您应该将响应解析为模型对象的原因。

**关注点分离**

UI 层不需要知道 JSON 的结构，也不需要知道如何遍历…