# 评估各项评估(第 3 部分)

> 原文：<https://medium.com/nerd-for-tech/assessing-the-assessments-part-3-51835da43b6b?source=collection_archive---------19----------------------->

这是我第三篇关于 LinkedIn 的 HTML 评估的帖子。在我以前的帖子中，我主要讨论了出现在问题和/或可能答案列表中的 HTML 元素。在这篇文章和我的下一篇文章中，我将讨论我在评估中看到的属性。

**关于属性**

即使您是 HTML 的新手，您也可能遇到过元素的开始标记中的项目是由一个单词或几个字母后跟“=”组成的，然后是一些用引号括起来的文本。常见的一个是 *href* :

```
<a href=”[https://medium.com/](/)”>Medium</a>
```

*href* 是一个 HTML 属性的例子。这个属性和其他属性在 HTML 中扮演着重要的角色。正如 W3Schools.com 所说，“HTML 属性提供了关于 HTML 元素的额外信息。”例如， *href* 属性表示一个链接实际链接到什么。另一个是 *src* ，比如你会在< img >标签中看到的。

```
<img src=”pretty-picture.jpg”>
```

*src* 属性表示标签应该显示什么图像。

让我们来看看 LinkedIn 评估中的一些特征。

有身份证吗？还是阶级？

评估询问了 class 属性和 id 属性。您可能最熟悉这两个属性，因为它们与 CSS 有关。向元素添加 id 或 class 属性允许您以特定的方式设置元素的样式。两者的主要区别在于，HTML 文档中的多个元素可以分配给同一个类，而一个 id 只能应用于 HTML 文档中的一个元素。

评估中出现了一个问题:

*在这段代码中，id 属性的用途是什么？*

*< p id="warning" >安装本产品时请小心。</p>*

您应该能够立即消除答案列表中的选项，该选项说 id 可以用于每页多次样式化。其他答案正确地陈述了“id 是唯一标识符”。然而，一个答案表明它是网站中的唯一标识符，而其他答案表明它是文档中的唯一标识符。哪个论断是正确的？如上所述，id 属性只能应用于 HTML 文档中的一个元素，所以我们可以排除声称它是网站中唯一标识符的答案。剩下的选择是相似的，有一个关键的区别。一个人说，“它确定 id 是文档中的唯一标识符，用于样式化 CSS 和 JavaScript 代码。”听起来是个不错的选择，但是另一个选项提到了这个属性的另一种用法:“在网页中链接”是的，我们可以为 JavaScript 使用 Id 属性(比如 getElementById()方法)，但是不要忘记，我们可以在锚标记中使用 id 来链接到网页上的特定位置。

关于类属性用途的问题的答案也有细微的差别，您可能需要在选择之前停下来仔细阅读它们。它们都正确地说明了类允许 CSS 选择页面上的特定元素。他们中的两个还提到这适用于 JavaScript 和 CSS，事实上也是如此(例如 getElementsByClassName()方法)。选择这两者中哪一个是正确的关键在于知道 class 属性是只能保存一个还是多个类名。你知道吗？要选择的正确答案是这样的:“您可以在 class 属性中列出任意多的类名，用空格分隔。”

**不稳定的文本区域**

另一个问题问四个可能的答案中哪一个是*不是 [< textarea >](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/textarea) 元素的有效属性。选项有*只读*、*最大值*、*拼写检查*和*表单*。在复习评估之前，我可能无法正确回答这个问题——除非是侥幸猜中。事实上，我甚至不知道有一个[拼写检查](https://www.w3schools.com/tags/att_global_spellcheck.asp#:~:text=The%20spellcheck%20attribute%20specifies%20whether,Text%20in%20elements)属性。*

提醒一下… <textarea>是“多行纯文本编辑控件，当您希望允许用户输入大量自由格式的文本时非常有用。”Mozilla Developer Network 列出了该元素的 16 个属性:</textarea>

*   自动资本化
*   自动完成
*   自动更正
*   自（动）调焦装置
*   关口
*   有缺陷的
*   形式
*   maxlength
*   最小长度
*   名字
*   占位符
*   只读的
*   需要
*   行
*   拼写检查
*   包

因此，我们可以很容易地得出结论，问题的正确答案是 *max* 。可以用 *maxlength* ，不能用 *max* ，用< textarea >。

**Enctype(？)**

这个属性对我来说是新的。评估中的问题如下:

*应该用什么来填充下面 HTML 中的空白？*

*<form method = " post " action = " mailto:info @ LinkedIn . com " _ _ _ _ _ _ = " text/plain ">*

可能的答案是*类型*、*媒体*、 *enctype* 和 *rel* 。我首先想到的是*类型*，但是该属性不能与<表单>元素一起使用。稍微搜索一下就会发现正确的选择应该是 [*enctype*](https://www.w3schools.com/tags/att_form_enctype.asp) ，它与< form >一起使用，表示“表单数据在提交给服务器时应该如何编码。”顺便说一下，除了“text/plain”之外，这个属性还可以有一个值“application/x-www-form-urlencoded”或“multipart/form-data”。

我敢肯定这是不太为人所知的 HTML 属性之一，在我为准备评估而观看的 YouTube 视频之一中，参加测试的人——一位有 3 年多经验的 web 开发人员——不知道问题的答案，并且猜错了。因此，如果任何评估问题难倒了你，也不要难过。

我将在下一篇文章中结束我在评估中遇到的关于 HTML 属性的讨论。