# Matt 的 101 号花絮——如何写一份好的 bug 报告

> 原文：<https://medium.com/nerd-for-tech/matts-tidbits-101-how-to-write-a-good-bug-report-818ab8994ada?source=collection_archive---------6----------------------->

![](img/38aac84142469fb3e19295382d341213.png)

本周我有一个关于提交 bug 报告的故事要分享——为什么它们很重要，以及什么是“好”的报告。[上次，(很抱歉帖子之间的延迟！)我写了一些团队相关的小技巧。](/nerd-for-tech/matts-tidbits-100-all-about-teams-856771e938de)

在我提交错误报告之前，我已经写了一些花絮，但是我有一个令人兴奋的更新要分享——我提交的一个错误最近已经被修复了，这是我的报告的直接结果！！

[2020 年 2 月，我写了一篇关于一个 bug](https://matthew-b-groves.medium.com/matts-tidbits-56-be-careful-with-jvmoverloads-165d87581820) 的文章，我发现 Android Studio 对衍生自 Android view 组件的基于 Kotlin 的类的“快速修复”建议危险地自动生成了一个带有`@JvmOverloads`注释的构造函数，导致默认的样式/主题属性被忽略。

描述这个漏洞的报告在这里:[https://issuetracker.google.com/issues/149986188](https://issuetracker.google.com/issues/149986188?pli=1)

这里要吸取什么教训？除了我最初对使用`@JvmOverloads`的谨慎之外，这次我想告诉大家的是如何写一份好的 bug 报告。这确实是被修复的错误和被忽略的错误之间的关键区别之一。

那么，一份好的 bug 报告应该包含哪些内容呢？

1.  对问题的清晰描述——如果人们不明白你在说什么，在这个过程中很早就被忽略了。诀窍是要非常精确地指出问题是什么，要有足够的细节——作为一名开发人员，我见过说“<x>不起作用”的错误报告——这还不够好。另外，尽量准确地说明你正在使用的所有组件的版本(Android Studio、Kotlin、库、手机品牌/型号、操作系统等)。)，如果可以的话，做一些额外的测试，以确定并能够解释哪些配置可能会受到此问题的影响。</x>
2.  一个容易重现的测试用例——理解问题是一回事，但是如果您做一些额外的工作来构建一个清晰展示问题的示例项目，您可以极大地提高问题被解决的可能性。不要只是上传整个项目的副本——你能提供的例子越小/越简单越好。
3.  对预期行为的描述——如果调查你的 bug 的人已经跟踪你完成了上面的前两项，但是你还没有详细说明你认为应该发生什么，你的 bug 可能没有得到处理——他们可能认为它“按照设计的那样运行”。在这里，你可以使用一些有说服力的/合乎逻辑的论点来清楚地解释*为什么*当前的行为是错误的，以及它应该做什么。
4.  仔细检查你的错误报告(逻辑错误、信息缺失、打字错误、语法错误等等)。)如果你的错误报告中有错误，它不会被认真对待——所以一定要仔细检查错误，尽量用专业的语气写。使用大写适当的完整句子等。消除了阅读它的人可能得出你不知道你在说什么的结论的障碍。
5.  宣传这个问题，并让其他人参与进来——越多的人明显受到这个问题的影响，这个问题的优先级就越高。因此，一旦你提交了问题，尽可能广泛地分享它——告诉你的同事过去和现在的情况，并要求他们也这样做，并考虑写一篇关于它的文章以增加它的可见性。

遵循以上步骤，如果幸运的话，您的问题将会得到解决！特别是开源项目的质量越来越依赖于社区成员报告他们所经历的问题。而且，如果你的 bug 看起来需要很长时间才能修复，考虑尝试自己解决它！开源项目通常拥有有限的开发人员资源，因此如果您自己为问题提供了可靠的解决方案(或者至少已经对原因进行了一些研究，并提供了如何解决问题的建议)，这可能是解决问题的另一个有益的推动。

最后，请注意，上述建议不仅仅适用于外部错误报告——这些策略也可以用于增强内部错误报告！

作为参考，这里是我之前提交的另外两个 Android Studio 错误，它们也已经被解决了(这两个都是间接的，但是它仍然可以帮助项目的开发人员提供额外的数据点):

[https://issuetracker.google.com/issues/131403382](https://issuetracker.google.com/issues/131403382)

https://issuetracker.google.com/issues/143971679

有兴趣和我一起在埃森哲出色的数字产品团队工作吗？我们有几个空缺职位，包括:

*   [移动开发商#1(东北地区)](https://www.accenture.com/us-en/careers/jobdetails?id=00960587_en&title=Native+Mobile+Developer)
*   [移动开发者#2(东北地区)](https://www.accenture.com/us-en/careers/jobdetails?id=00960583_en&title=Mobile+Developer)
*   [工程总监(波士顿或费城)](https://www.accenture.com/us-en/careers/jobdetails?id=00983613_en&title=Engineering+Director)
*   [高级 iOS 开发人员(南部地区)](https://www.accenture.com/us-en/careers/jobdetails?id=R00036232_en&title=Senior+iOS+Developer)
*   [安卓移动开发者(南部地区)](https://www.accenture.com/us-en/careers/jobdetails?id=R00027147_en&title=Android+Mobile+Developer)
*   [iOS 移动开发主管(南部地区)](https://www.accenture.com/us-en/careers/jobdetails?id=R00027113_en&title=iOS+Mobile+Development+Lead)

关于如何写好 bug 报告，你还有其他建议吗？请在下面的评论中告诉我！