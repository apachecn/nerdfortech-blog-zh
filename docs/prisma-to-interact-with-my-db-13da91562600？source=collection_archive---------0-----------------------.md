# Prisma 与我的数据库交互

> 原文：<https://medium.com/nerd-for-tech/prisma-to-interact-with-my-db-13da91562600?source=collection_archive---------0----------------------->

![](img/8a20ec60100ffb252725830b01911c23.png)

由 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的 [Mael BALLAND](https://unsplash.com/@mael_balland?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的照片

我已经使用 [Prisma](https://www.prisma.io/) 几个月了，最近我给[做了一个关于它的演讲](https://www.meetup.com/fr-FR/LyonJS/events/279341192/)，让我们在这里试着回顾一些要点。

*你可能更喜欢浏览幻灯片，* [*给你*](https://prisma-talk.netlify.app/1) *。*

# 与我的数据库交互

Prisma 属于 ORM 家族(对象关系映射)。它可以被看作是服务器和数据库之间的一个额外的层