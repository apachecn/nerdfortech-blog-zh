# 我处理动态 JSON 负载的方式。

> 原文：<https://medium.com/nerd-for-tech/hands-on-my-way-of-handling-dynamic-json-payload-2b74677902b6?source=collection_archive---------0----------------------->

当您从事 API 测试自动化工作时，您可能会遇到创建动态 JSON 有效负载来支持您的测试的挑战。

![](img/102ce72d84c8ee8fdec8fd4c97b5e774.png)

费伦茨·阿尔马西在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

对此有不同的方法。

1.  您可以使用 GSON 或 Jackson 库，并从 Java 对象生成 JSON
2.  你可以使用速度模板，注入动态值…