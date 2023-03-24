# 评估各项评估(第 4 部分)

> 原文：<https://medium.com/nerd-for-tech/assessing-the-assessment-part-4-95af7a2c0fe4?source=collection_archive---------18----------------------->

这将是我关于 LinkedIn HTML 评估的最后一篇文章。正如在我以前的文章中，我将讨论评估中出现的几个 HTML 属性。

**控制用*控制*控制**

一个问题是下面的代码会做什么:

```
<audio controls>
   <source src=”sound.mp3” type=”audio/mpeg”>
   <source src=”sound.ogg” type=”audio/ogg”>
   <source src=”sound.wav” type=”audio/wav”>
</audio>
```

我们来分析一下。首先，在标签中你会看到*控件*属性。你以前遇到过这种属性吗？如果是这样的话，它必须在< audio >中，如本例所示，或者在< video >中，因为这是 controls 属性可以使用的唯一元素。根据[W3Schools.com](https://www.w3schools.com/tags/att_controls.asp)的说法，这是一个布尔属性“指定应该显示音频/视频控件”但是什么样的控制呢？这个问题的可能答案列表提到了浏览器的默认控件或 JavaScript 提供的控件。哪个是对的？答案是…浏览器的默认控件。

接下来，注意每个 *src* 属性指向不同的声音文件。在你有多个<源>元素的情况下会发生什么？一个可能的答案是，“浏览器会自动按顺序播放每个声音文件……”是这样吗？实际上，浏览器将播放第一个支持的格式。此外，其中一个答案显然试图通过指示将播放第一个支持的声音文件来欺骗您，但随后它补充说浏览器“将循环播放声音，直到用户停止它。”我不得不对此进行一些检查，发现视频或音频文件必须包含*循环*属性才能发生这种情况。因此，问题的正确答案是，“浏览器选择第一个支持的格式来播放浏览器的默认控件。”

**张贴*海报***

这是评估中的一个问题，如果没有快速的谷歌搜索，我可能不知道如何回答:“在<视频>标签中，**海报**属性有什么作用？”Mozilla Developer Network 告诉我们，这个属性指定了一个“下载视频时要显示的图像的 URL”两个可能的答案是这么说的，但其中一个还加上了“直到视频播放完毕。”选哪个？[W3Schools.com](https://www.w3schools.com/tags/tag_video.asp)也提到了海报属性将导致指定的图像显示“在视频下载时，*或者直到用户点击播放按钮*”(重点是后加的)。

**信任*媒体***

看看这个<link>元素:

```
<link href=”phone.css” rel=”stylesheet” ____=”print”>
```

好的，我们需要填补空白。这些答案中的哪一个需要填入空白处:标题、设备、类型还是媒体？关键在于指定的值，在本例中是“print”根据 [W3Schools](https://www.w3schools.com/tags/att_link_media.asp) ，这是*媒体*属性的一个可能值，该属性“用于打印预览模式/打印页面” [Mozilla](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/link) 解释道，“[媒体]属性主要在链接到外部样式表时有用——它允许用户代理选择最适合它运行的设备的样式表。”那么*媒体*就是了。

注意你的语言！

这是另一个填空题。你认为这个< a >标签中的 *hreflang* 属性属于什么？

```
<a href=”[https://es.yahoo.com](https://es.yahoo.com)” hreflang=”____” target=”_blank”>Visita Yahoo</a>
```

这是我在准备评估之前没有看到的另一个属性。W3Schools.com 说[*hreflang*](https://www.w3schools.com/tags/att_a_hreflang.asp)*“指定链接文档的语言”，并且它的值将是两个字母的语言代码。因此，虽然从可能的答案列表中选择“西班牙语”可能很诱人，但正确的答案将是“es”。*

*这就结束了我对 LinkedIn 的 HTML 评估的讨论。正如我之前所说的，我强烈建议观看一些关于这个的 YouTube 视频(你可以在搜索栏中输入“linkedin html 评估”)，并且你也想用你自己的谷歌搜索来验证评估者选择的一些答案。你可能已经注意到，我已经开始严重依赖诸如 Mozilla 开发者网络和 W3Schools.com T21 这样的资源。我接下来的几个帖子会涉及 LinkedIn 的 CSS 评估。让我们一起继续学习吧！*