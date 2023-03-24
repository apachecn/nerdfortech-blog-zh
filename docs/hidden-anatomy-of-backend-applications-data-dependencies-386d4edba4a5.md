# 后端应用程序的隐藏剖析:数据依赖性

> 原文：<https://medium.com/nerd-for-tech/hidden-anatomy-of-backend-applications-data-dependencies-386d4edba4a5?source=collection_archive---------12----------------------->

在之前的文章中([这里](https://sergiy-yevtushenko.medium.com/hidden-anatomy-of-backend-applications-context-lifecycle-47f8d48dd4b6)和[这里](https://sergiy-yevtushenko.medium.com/hidden-anatomy-of-backend-applications-50c9c9b67ed9)，我们已经从非传统的角度看了后端应用程序。这样的外观使我们能够看到后端应用程序固有的处理模式，并且不依赖于使用的工具、语言和框架。本文关注的是另一种普遍存在的处理模式，它几乎存在于任何后端应用程序中。

几乎每个后端入口点都需要一些数据来生成响应，这不足为奇。它可能像常量字符串一样简单，也可能像…