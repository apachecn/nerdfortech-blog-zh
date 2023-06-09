# 编写清晰的 API 文档

> 原文：<https://medium.com/nerd-for-tech/writing-a-clear-api-documentation-43c2152eed55?source=collection_archive---------23----------------------->

API 文档是 API 的技术文档。它通常包括从 API 支持什么类型到存在哪些端点以及它们做什么的所有内容。文档在几个方面是必不可少的:

*   任何想使用 API 的开发人员都需要这份文档来了解它到底是如何工作的，他们能信任它吗？
*   新的团队成员应该能够理解 API 背后的操作，而不需要任何额外的研究或与团队专家交谈
*   API 的用户需要这些文档来理解 API 是否对他们的需求有帮助。
*   文档本身应该足够简单，易于阅读和理解

![](img/b28f8a2d23efb4e6bc5400df4975f6d0.png)

照片由[西格蒙德](https://unsplash.com/@sigmund?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

有两种类型的文档:

- API 描述指南:这是每个端点的文档，使用的格式是一个简单的表格。该表包括端点的名称、它采用的参数、它关闭的类型以及任何其他可用选项。这通常包含了开发人员需要了解的关于 API 的所有内容。

- API 参考指南:这更像是对每个方法或端点的概述。每一个都有一个简短的描述以及任何附加信息。一些开发人员喜欢使用这种类型的文档，因为它比更简化的版本更容易使用，但是两者都有用，这取决于您计划如何使用它们。

**注:** API 文件不是参考文件。

![](img/fc8b3d7037a17254f43f27212e889620.png)

克里斯托弗·高尔在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

让我们来看一下编写 API 文档的一些技巧。

第一个建议是保持简单。使用聊天用语或过长的句子可能看起来不错，但它们不能解释任何事情。把自己放在一个对 API 一无所知的人的位置上可能更有意义——如果你是这样做的，我敢打赌你会希望事情尽可能地简化和透明。

API 越复杂，这个文档就越重要。如果有许多端点、复杂的类型和众多的路径，开发人员不需要询问其他人就能容易地理解事情是如何工作的，这一点很重要。它应该用清晰、简单的语言来写，这样即使是最少技术经验的人也能理解它，而不必花几个小时去理解它。

![](img/e28cb0803f3db6eae10bc580a8720b25.png)

约书亚·索蒂诺在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

API 文档也是技术写作的一种形式，包括记录能够被其他应用程序使用的代码或编程语言。

为 API 编写文档时，请确保包括:

*   支持的类型。例如，如果您的 API 有两个参数，这两个参数都必须是数字，那么请在文档中说明这一点。像这样简单的事情很容易搞砸，最终得到人们无法理解的文档。
*   什么样的请求是可能的？例如，如果所有端点都是 GET 请求，并且不可能发布任何东西，那么在文档中说明这一点。这有助于试图使用 API 的开发人员知道他们可以用它做什么，或者在开始使用它之前他们需要学习什么。在这种情况下，提及为什么不允许其他类型也是可行的。
*   支持哪些类型。如果一个 API 只接受电子邮件和电话号码作为输入，那就这么说。明确什么不能用来满足 API 的要求。
*   利用这些投入可以做些什么？获取用户列表，加载特定用户，或者在出错时处理错误。有没有可以返回的具体错误码？这些信息应该很容易在文档中找到，这样开发人员就知道如何正确使用您的 API，而不必感到沮丧。

一个好的 API 文档应该易于参考，但仍然保持简单易懂的叙述。您的 API 文档对客户来说是一个 UI。这是营销最终用户的最佳方式，所以要小心开发。