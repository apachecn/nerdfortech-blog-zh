# 在 SpringBoot 中集成 JWT 实现令牌验证(补充说明)

> 原文：<https://medium.com/nerd-for-tech/integrate-jwt-in-springboot-to-realize-token-verification-additional-notes-5438790568d8?source=collection_archive---------17----------------------->

在上一篇文章中，我们在 SpringBoot 中集成了 JWT，并实现了令牌验证。在实际应用中，我们会发现如果每个视图层(控制器)都手工验证令牌，代码会特别臃肿。本文的文章主要是解决这个问题。

![](img/77be292d1193866ee463b803ce4816f0.png)

在 [Unsplash](https://unsplash.com/s/photos/developer?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上由[米米·蒂安](https://unsplash.com/@mimithian?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍摄的照片

如果你对 JWT 的集成不熟悉，可以看看我之前的文章:[在 SpringBoot 中集成 JWT 实现 Token 验证(集成)](https://zivukushingai.medium.com/integrate-jwt-in-springboot-to-achieve-token-verification-integration-60b57b24db19)

## 自定义注释

**创建自定义注释**

我们已经创建了一个名为 JwtToken 的注释，它没有参数。对上述一些注释的解释:

*   @Target(ElementType。METHOD)，Target 表示被注释修改的对象的范围，METHOD 用于描述方法
*   @保留(RetentionPolicy。运行时)，运行时注释，注释不仅保存在类文件中，而且在 JVM 加载类文件后仍然存在
*   @ Documented，元注释，表示这个注释应该由 Javadoc 工具记录

**拦截器**

**注释拦截器**

我们拦截所有以 api 开头的请求路径，如果用@JwtToken 注释，将验证令牌信息，从而实现一个自定义的注释验证令牌。