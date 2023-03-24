# 加速咖啡筛选

> 原文：<https://medium.com/nerd-for-tech/speeding-up-coffee-sifting-f26e9bb14ba7?source=collection_archive---------0----------------------->

## 咖啡数据科学

## 改变协议以加速筛选

我为[断续浓缩咖啡](https://youtu.be/kZrFy_IimXY)筛选了很多咖啡，并且努力提高筛选的效率。我目前使用[研究员摆振](https://towardsdatascience.com/the-fellow-shimmy-sifter-a-data-driven-review-8eedc23580f8)筛细(<300 微米)和[克鲁夫筛](https://towardsdatascience.com/comparing-kruve-coffee-sifters-new-and-old-677e8b16ec62)粗磨 500 微米。这需要几分钟，但我一直在寻找让它更快的方法。

我已经能够使用搅拌器来改善 Kruve 筛的筛分时间，并且我已经能够通过在筛分期间几次将粉末倒入另一个碗中来改善 Fellow Shimmy。

![](img/62f4e504ac7b41071268cd6ed533ea3f.png)

所有图片由作者提供

# 克鲁夫筛的改进

我开始使用 Kruve 筛子，最初，它通过 2 个筛子(400 微米/500 微米)以 3g/分钟的速度筛分，因此需要 7 分钟或更长时间来筛分 21g。有了搅拌器，我可以提高到 5 分钟。搅拌器的缺点是它们会随着时间的推移而退化，而且声音很大。

![](img/7114f130ed56d3774a774ab79b106edb.png)

我通常会做一个纸三角形，里面放一些洗过的硬币，以增加重量，帮助将咖啡渣压在屏幕上。我还会用一点埃尔默胶水来封住纸，这样咖啡渣就不会进去了。

![](img/4f96642638c1492f5c5d502ac19e55f1.png)![](img/3b696eabc84a079fbc6868c45215f42f.png)![](img/465326cf638d4c935528d42dbff1be85.png)![](img/d7678f6e5ddd9a73c3f655230713965f.png)![](img/e4fa0d4f0ab540a896be13c6e8cd4633.png)![](img/01cbffd1c0223cfc16c9d6409bd817e4.png)

然而，随着时间的推移，纸张会慢慢磨损。

![](img/062b33902766fbab5ca82c87eb17763e.png)

我用一根细线和一个金属盘(一个杠杆机器的旧淋浴屏)做了实验。这对于搅拌器是有效的，但是金属丝会相对容易折断。他们可以制作一个内置旋转器的屏幕。这对于 400um 及以下的屏幕更为重要。

![](img/dde5ed0d45c66cf2cda124490d3c34e3.png)![](img/9101925221f12e9896f46650b69c3d37.png)

我的黑客没有持续很长时间，但我怀疑有人会做得更好。

我最近一直在使用的搅拌器是一个弯曲的金属网，这样它就不会太用力挤压筛子的侧面。它不会像纸一样降解，与较重的金属盘相比，它在筛选过程中不会产生太多噪音。

![](img/0b8efcd4035b31cce49f233a01a7b534.png)

# 改善研究员摆振

我开始用摆振筛选精细层，因为它更符合人体工程学。速度稍快，但更重要的是，在手臂运动方面更省力。此外，它没有那么大声，也不需要搅拌器维护。

![](img/7a408b71aa4435595e36734c212339d1.png)

我通常要花 5 分钟用 Shimmy 筛，再花一分钟用 Kruve 用 500 微米的筛网分离粗颗粒。然后，我发现了一个巧妙的技巧，我经常倾倒研磨。如果我每分钟将粉末倒入另一个碗里，我可以将筛选时间从 5 分钟减少到 3 分钟。

部分增益是由于上下摇动筛子时，颗粒可以来回通过筛子，这降低了细颗粒与粗颗粒分离的速度。

## 协议:

重复直到过筛:

1.  筛选 1 分钟
2.  倾卸底部托盘
3.  重复

![](img/a6e9ba3301b636665e641202f1a19db5.png)![](img/aa769f837acb1620853d016ddfad75af.png)

然后我在最后称重，准备冰球。

![](img/eedec8487b781dcb65eb30cd0d0edaf5.png)![](img/d12893530fe95bcff12289c465d84c61.png)

终于，我可以喝一杯好的断续浓缩咖啡了。

![](img/0e8c89151f4375278ac031ec20cefeb0.png)

断奏击球让我着迷的一点是，击球后的断奏冰球有多美。

![](img/86ab62974f1d624f5dc393420a48334b.png)

一个更好的摆振设计将使细粒落入一个空腔中，当摇动筛子时，该空腔更难使粉末返回到筛子上。

如果你愿意，可以在推特、 [YouTube](https://m.youtube.com/channel/UClgcmAtBMTmVVGANjtntXTw?source=post_page---------------------------) 和 [Instagram](https://www.instagram.com/espressofun/) 上关注我，我会在那里发布不同机器上的浓缩咖啡照片和浓缩咖啡相关的视频。你也可以在 [LinkedIn](https://www.linkedin.com/in/dr-robert-mckeon-aloe-01581595) 上找到我。也可以关注我在[中](https://towardsdatascience.com/@rmckeon/follow)和[订阅](https://rmckeon.medium.com/subscribe)。

# [我的进一步阅读](https://rmckeon.medium.com/story-collection-splash-page-e15025710347):

[我未来的书](https://www.kickstarter.com/projects/espressofun/engineering-better-espresso-data-driven-coffee)

[我的链接](https://rmckeon.medium.com/my-links-5de9eb69c26b?source=your_stories_page----------------------------------------)

[浓缩咖啡系列文章](https://rmckeon.medium.com/a-collection-of-espresso-articles-de8a3abf9917?postPublishedType=repub)

工作和学校故事集