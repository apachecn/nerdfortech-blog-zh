# 从一个男孩和女孩的故事看重试的原则

> 原文：<https://medium.com/nerd-for-tech/the-principles-of-retries-as-seen-through-a-boy-and-girl-story-5d16a0d60938?source=collection_archive---------4----------------------->

![](img/c2d9bc577f3b19c36856876697ad521b.png)

在生活中，你依赖于许多外部团体，在软件中也是如此。就像真实世界一样，外部系统可能不总是满足我们系统的要求，因此“**重试**”。让我们试着通过一个男孩和女孩的故事来捕捉一些重试的原则:)

场景——一个男孩终于鼓起足够的勇气向他的爱人求婚。

**男生求婚，女生说→“我结婚了！”**

**重试策略** →不要！

分析→出现不可挽回的错误时使用此策略。这就像`[**422 Unprocessable Entity**](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/422)**,** [**451 Unavailable For Legal Reasons**](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/451)**,** [**410 Gone**](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/410)**.**` 没有必要在这里重试。接受失败，继续前进。

**男生求婚，女生说→ …没什么**

**重试策略** →再次尝试，牢记刺激因素。

分析→就像暂停一样。您请求了某些东西，但您不知道该请求是否被满足。你可以再试一次，但要小心。如果你不想太麻烦，尝试指数后退。如果没有刺激因素，没有重试的成本…我们就继续尝试，直到你得到一个明确的答案。 [**等幂**](/@suhas_chatekar/why-you-should-use-the-recommended-http-methods-in-your-rest-apis-981359828bf7) 在这里很重要。

**男生求婚，女生说→“我会回来找你的”**

**重试策略** →请求被确认。请使用请求 id 重试。

分析→这里你知道要求达到了。总是发送带有 id 的请求，以便您稍后可以再次咨询服务器并检查请求是否被满足。

**男生求婚，女生说→ …没什么** *(但是另一个女生在等男生回她！)*

**重试策略** →重试到有时间再假设失败。

分析→现在你有一个业务 SLA:)这就像一个客户在等待你的系统响应。如果你在需要的时间内没有得到明确的回应，假设失败，解放顾客(在这种情况下，对等待的女孩说是)。启动对外部系统的故障响应，以便在需要时采取纠正措施。

**男生求婚，女生昏迷。**

**重试策略** →重试直到可行。

分析→就像`[**500 Internal Server Error**](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/500)**,** [**503 Service Unavailable**](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/503)**.**` 重试，直到可行。

男孩求婚，女孩说→“我不是你的另一半。”。去找她。”

**重试策略** →重试，但使用其他人。

分析→就像`[**303 See Other**](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/303)**.**` 在新的服务器上尝试请求。

**男生求婚，女生说→“你不够有钱。”**

**重试策略** →在获得足够的凭证后重试。

分析→就像`[**426 Upgrade Required**](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/426)`。符合要求后再试。如果能在运行时自动完成，那就太好了！比方说，您已经用完了点击图像分析 API 的积分。如果您有在运行时购买更多信用的机制，那就太好了，否则在一个特殊的地方标记请求，在那里人们可以采取纠正措施并再次放置请求进行处理。

**男生求婚，女生说→任何否定回答**

**重试策略** →使用**备份选项重试。**

分析→如果有备份选项，就用这个策略。假设您正在尝试发送短信，而当前提供商没有响应。您可以立即尝试不同的版本。或者说你正在尝试对一个地址进行地理编码。提前准备好备份选项，这样你就可以依靠它们了。

**男孩求婚，女孩说→“是”**

**重试策略** →不需要。幸福生活:)

分析→请求成功。你的 HTTP 请求得了 200 分。万岁！