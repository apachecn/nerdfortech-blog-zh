# 在 SpringBoot 中集成 JWT 实现令牌验证(拦截器)

> 原文：<https://medium.com/nerd-for-tech/integrate-jwt-in-springboot-to-realize-token-verification-interceptor-f6b0e8a01d2c?source=collection_archive---------12----------------------->

在上一篇文章中，我们已经实现了使用自定义注释来验证令牌信息，所以我们会发现，当我们需要验证更多的接口时，我们需要给每个方法添加@ JwtToken 注释，这也是非常麻烦的。在本文中，我们继续使用拦截器来验证令牌信息。

![](img/2a3bed2ac1a457b01e9095132def7ff5.png)

[Unsplash](https://unsplash.com/s/photos/developer?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上 [AltumCode](https://unsplash.com/@altumcode?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的照片

如果你不熟悉 JWT 的集成，可以看看我的文章:[在 SpringBoot 中集成 JWT 实现 Token 验证(集成)](https://zivukushingai.medium.com/integrate-jwt-in-springboot-to-achieve-token-verification-integration-60b57b24db19)

如果对自定义标注验证令牌信息感兴趣，可以看看我的文章:[在 SpringBoot 中集成 JWT 实现令牌验证(补充说明)](https://zivukushingai.medium.com/integrate-jwt-in-springboot-to-realize-token-verification-additional-notes-5438790568d8?source=your_stories_page-------------------------------------)

**自定义拦截器**

我们通过在上一篇文章中集成的 JWT 来验证令牌信息。如果验证成功，则返回 true。如果验证失败，将 HttpServletResponse 对象的代码更改为 401，这意味着用户没有登录并返回 false。此时，该方法将直接结束。我们进入下一步

**注释拦截器**

通过注册我们的自定义拦截器，我们设置拦截路径，以 api 开头的路径将被验证令牌信息。
我们还设置了非拦截路径，如注册、登录、忘记密码等。无需用户登录就可以直接请求，没有必要验证令牌信息。

至此，我们已经用一个定制的拦截器完成了令牌信息的验证。与自定义标注相比，这种方法更简单、更方便。我们只需要关注哪些路径被屏蔽了，哪些路径没有被屏蔽。