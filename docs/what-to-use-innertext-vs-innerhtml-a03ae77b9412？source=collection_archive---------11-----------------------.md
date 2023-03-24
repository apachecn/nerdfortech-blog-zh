# 用什么？innerText 与 innerHTML

> 原文：<https://medium.com/nerd-for-tech/what-to-use-innertext-vs-innerhtml-a03ae77b9412?source=collection_archive---------11----------------------->

![](img/e2cda146d7a3d848e21d7e9f6018372e.png)

上一个项目是制作一个问答应用程序，接下来我想分享我学到的一个要点。

# 出了什么问题？

当我完成制作测验应用程序的课程时，我遇到了一个问题。有几个问题和答案包含单引号或双引号。问题和答案的文本来自获取的 JSON API。我发现在文本中，引号显示为'或"。我想让这些 HTML 代码正确地显示为“或”。

# **我试图解决什么问题？**

我开始谷歌搜索。我试着用引号的 HTML 代码搜索，以及如何解码它们。99%的谷歌搜索结果与`escape()/unescape()`或`encodeURI()/decodeURI`相关。所有这些功能都不是我想要的，因为我想解码文本中的 HTML 代码。我认为对我来说寻找解决方案更难，因为我甚至不知道如何清楚地定义问题，以及我应该采取什么方向来解决它。

几天过去了，没有解决办法。然后，我又开始谷歌搜索，耐心地一个一个查看所有结果。我对我的问题没有答案感到失望和沮丧，然后我找到了课程讲师詹姆斯·奎克创建的 Discord 服务器，人们可以在其中分享和提问，并获得专家的帮助。在社区上，我碰巧发现有人提出了一个和我完全一样的问题。错误来自于`innerText`地产。

```
...const questionIndex = Math.floor(Math.random() * availableQuestions.length);currentQuestion = availableQuestions[questionIndex];question.**innerHTML** = currentQuestion.question;choices.forEach((choice) => {const number = choice.dataset['number'];choice.**innerHTML** = currentQuestion['choice' + number];});...
```

加粗的部分是我最后用`innerHTML`代替`innerText`修复的地方！

# innerHTML 和 innerText 的区别？

我很欣慰我可以修复这个问题，但真正的问题是…我真的不明白为什么 innerHTML 的变化修复了它。😅

我开始再次谷歌这些不同之处，并了解到主要的不同是`innerText`只把所有的东西当作文本。这意味着当`currentQuestion`被应用到 HTML 元素的`innerText`时，引号的 HTML 代码将被视为文本，因此它们不能在屏幕上正确显示。

相反，`innerHTML`属性将解析元素中包含的 HTML 实体。换句话说，引号的 HTML 代码将被解析并转换为值。

顾名思义，我知道`innerText`用于呈现文本内容，而`innerHTML`用于元素中的 HTML 标记。

# 解决问题的技巧…？

如果我知道这些问题来自这些属性，我会花更少的时间来解决它。我认为清楚地理解这个问题并选择正确的关键字进行搜索对于任何问题的解决都是非常重要的。希望，当下一个问题来临时，我已经准备好更有效的谷歌搜索了！🤓