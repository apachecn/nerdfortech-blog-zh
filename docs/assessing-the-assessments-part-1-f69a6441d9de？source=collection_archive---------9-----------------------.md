# 评估各项评估(第 1 部分)

> 原文：<https://medium.com/nerd-for-tech/assessing-the-assessments-part-1-f69a6441d9de?source=collection_archive---------9----------------------->

正如我在第一篇媒体文章中提到的，我最近完成了一个在线软件工程项目，并一直在找工作。作为我求职努力的一部分，我整理了我的 LinkedIn 个人资料，甚至决定参加一些他们的[技能评估](https://www.linkedin.com/help/linkedin/answer/94427/linkedin-skill-assessments?lang=en)，这将(有希望)让我的个人资料在未来雇主的眼中有所提升。到目前为止，我已经参加了 HTML 和 CSS 的技能评估，我很高兴地告诉大家，我在这两个方面都取得了前 5%的分数。

这些 15 个问题的评估是多项选择。然而，它们被证明是相当棘手的，如果我没有花一些时间准备它们，我怀疑我会做得几乎一样好。事实上，我强烈建议任何考虑参加评估的人去 YouTube 上看一些参加评估的人的视频。这样，你就会对可能出现的问题以及如何回答这些问题有所了解。您会注意到，每次评估时，问题都会有所不同。

在我接下来的几篇文章中，我将讨论这些评估，从 HTML 开始。在此过程中，我还将包括一些关于 HTML 的概述。

**《素材库》复习**

一些最简单的问题简单地询问了某些 HTML 元素和标签的用途。在这里回顾一下并确保理解元素和标记之间的区别可能是个好主意。我必须承认我使用了术语*元素*和*标签*好像它们是同义词，但实际上，它们指的是不同的东西。引用 W3Schools.com 的话:“一个 HTML 元素由一个开始标签、一些内容和一个结束标签定义。”换句话说，标签指示元素的开始和结束，而元素包含标签和标签中包含的内容。

```
<p>How now, brown cow?</p>
```

如果你学过 HTML，你肯定会认出上面是一个段落元素，有一个

开始标签，一个

结束标签，元素的内容夹在它们之间。我在测验中没有遇到要求我区分标签和元素的问题，但我仍然建议能够解释这一点。

当然，有些 HTML 元素不遵循这种模式。顾名思义，void 元素(也称为空元素)没有结束标记，也没有内容。空元素的例子有
、

* * *

和。顺便说一下，LinkedIn 的 HTML 评估可能会包括一个问题，要求您识别一组 void 元素(提示，提示)。

说到元素，您能识别 HTML 文档的根元素吗？这是评估报告中提到的。答案包括、、和<root>。作为一种试图摆脱你的方法，这些可能的答案之一甚至不是一个实际的 HTML 元素。你能发现它吗？</root>

这是一个非常基本的问题，所以你可能会知道选择… 。我怀疑人们很容易误认为它会是，尤其是因为它出现在 HTML 文档的顶部。然而，这是 DOCTYPE 声明，用于告诉浏览器这是一个 HTML 5 文档——是的，我在 LinkedIn 评估上看到了一个关于 DOCTYPE 声明的问题。对了，可能答案列表中的伪元素是<root>。</root>

**貌似语义**

有些问题涉及到了语义标记。与严重依赖通用 [< div >元素](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/div)的旧版本 HTML 相反，HTML 5 包括许多[语义元素](https://www.w3schools.com/html/html5_semantic_elements.asp)，这意味着它们向浏览器和任何阅读代码的人指示它们包含什么样的内容。显然，< table >元素包含一个表。<表单>元素包含一个表单。其他例子还包括<地址>、<文章>、<旁注>、<块引用>、<图>、<页脚>、<主>、<节>。

例如，LinkedIn 评估中的一个问题是，表明文本指键盘输入的最佳语义方式是什么。答案是使用`标签。另一个要求您选择语义正确的方式来标记这个布局:`

“赚钱是你维持一家企业所必须做的事情——被驱使去做一些有价值、有目的的事情要强大得多。”

*琳达·温曼*

您应该只在

标签中包含引用和名称吗？把它们放在一个

元素中，同时把引用放在<q>标签中，名字放在标签中，怎么样？或者用

> 元素怎么样？如果你使用
> 
> > ，那么你应该用什么标签来表示引用和人名？</q> 

在我为准备评估而观看的一个 YouTube 视频中，这个人选择了这个选项:

```
<blockquote> <q>“Making money is what you have to do to sustain a business — being driven to make something of value and purpose is much more powerful.”</q> <cite><em>Lynda Weinman</em></cite></blockquote>
```

然而，这是不正确的。既然这个问题是关于语义准确性的，那么选择使用 [<块引用>](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/blockquote) 而不是<部分>或者仅仅是< p >的选项当然是正确的。但是没有必要在<引用>标签内使用< em >标签，因为<引用>会自动将内容斜体化。同样，< q >会自动添加引号，因此当单词已经被引号包围时使用< q >会导致如下结果:

”“赚钱是你维持一家企业所必须做的——被驱使去做一些有价值和有目的的事情要强大得多。""

也就是说，文本周围会有一对引号！我会选择这个选项:

```
<blockquote> <p>“Making money is what you have to do to sustain a business — being driven to make something of value and purpose is much more powerful.”</p> <cite>Lynda Weinman</cite></blockquote>
```

在我的下一篇文章中，我将回顾评估中询问的更多 HTML 标签，以及它提出的一些关于属性的问题。