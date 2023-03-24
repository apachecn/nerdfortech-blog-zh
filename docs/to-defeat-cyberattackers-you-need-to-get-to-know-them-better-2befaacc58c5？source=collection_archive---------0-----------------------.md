# 要打败网络攻击者，你需要更好地了解他们

> 原文：<https://medium.com/nerd-for-tech/to-defeat-cyberattackers-you-need-to-get-to-know-them-better-2befaacc58c5?source=collection_archive---------0----------------------->

![](img/389435ba62133ec6772be77137daad38.png)

现在你的敌人。

中国将军孙子的古老智慧是关于物理战争的，但它同样适用于今天的网络世界。如果你想保护你的数字资产，你最好知道你的敌人——恶意黑客——可能会如何攻击它们。

这种告诫是 2022 年“[软件漏洞快照](https://www.synopsys.com/blogs/software-security/software-vulnerability-snapshot-report-findings/?cmp=pr-sig&utm_medium=referral)的焦点，这是新思网络安全研究中心(CyRC)最近的一份报告，基于对 2700 个目标网络或移动应用程序、源代码文件或系统进行的近 4400 次测试的数据。(披露:我为 Synopsys 撰稿。)

由 Synopsys 应用安全测试(AST)服务完成的大多数测试都是关于了解敌人的，旨在以真实世界攻击者的方式探测正在运行的应用，然后确定要修复的最关键的漏洞。

那些侵入性的“黑盒”和“灰盒”测试包括[动态应用安全测试](https://www.synopsys.com/software-integrity/security-testing/dast.html?cmp=pr-sig&utm_medium=referral) (DAST)、[移动应用安全测试](https://www.synopsys.com/software-integrity/application-security-testing-services/mobile-application-security-testing.html?cmp=pr-sig&utm_medium=referral) (MAST)分析，以及[渗透测试](https://www.synopsys.com/software-integrity/application-security-testing-services/penetration-testing.html?cmp=pr-sig&utm_medium=referral)——旨在评估应用或系统安全性的模拟攻击。DAST 和 MAST 是在运行代码中发现缺陷的自动化工具，而渗透测试是手动完成的，使组织能够在软件的最终开发阶段或部署后找到并修复运行时漏洞。

测试发现了大量缺陷——95%的应用程序至少有一个漏洞或配置错误，25%的漏洞是高风险或严重风险。这在某种程度上是好的——软件安全测试的目的是发现漏洞，并在坏人发现和利用它们之前修复它们。

正如该报告所言，“组织需要以与攻击者相同的方式测试他们正在运行的 web 应用程序，然后在漏洞被外部代理利用之前识别并消除它们。”

但这也是一个警告——您正在构建和/或使用的软件有缺陷的可能性接近 100%,并且其中四分之一的缺陷会造成重大损失。如果你不解决这些问题，你就是在自找麻烦。

**最坏的最坏的**

CyRC 的研究人员发现，77%的目标都有漏洞，这些漏洞出现在开放网络应用安全项目 [(OWASP)的十大名单](https://owasp.org/www-project-top-ten/)——最坏中的最坏。

该列表中最糟糕的漏洞是允许跨站点脚本(XSS)的漏洞，这些漏洞可让攻击者访问应用程序资源和数据。根据该报告，“Synopsys AST services 发现，22%的测试目标暴露于反射、存储或基于 DOM(文档对象模型)的 XSS 漏洞，”这些漏洞可以使黑客将恶意负载注入网页。

其他严重风险漏洞，如远程代码执行和 SQL 注入，使得攻击者能够在 web 应用程序或应用程序服务器上执行代码并访问敏感数据。

那么解决这些弱点的最好方法是什么呢？该报告有几个关键要点，可以帮助组织更好地了解他们的敌人，并采取有效措施击败他们。

*   **使用所有可用的测试工具。不同的工具以不同的方式发现软件中的弱点。组织应该全部使用它们。除了前面提到的 DAST、MAST 和渗透测试之外，[静态应用安全测试](https://www.synopsys.com/software-integrity/security-testing/static-analysis-sast.html?cmp=pr-sig&utm_medium=referral) (SAST)可以帮助在编写代码时标记代码中的缺陷，而[软件组合分析](https://www.synopsys.com/software-integrity/security-testing/software-composition-analysis.html?cmp=pr-sig&utm_medium=referral) (SCA)将帮助找到开源组件以及它们来自哪里、使用的是什么版本，以及它们是否包含任何已知的漏洞或许可冲突。**
*   **注意第三方危险。**今天的软件产品更多的是汇编的，而不是编写的——它们包括专有的、第三方的和开放源代码的组合。平均大约 77%的代码库是开源软件。

和其他软件一样，这个软件也是不完美的。根据该报告，在 OWASP 十大漏洞中被列为“严重”的易受攻击的第三方库使用漏洞，在超过 20%的 pen 测试中被发现，在 25%的 SAST 被发现。

正如报告指出的，如果您不知道您使用的所有组件的版本，和/或正在使用的代码不受支持或过期，您的组织就容易受到攻击。

*   **创建一个** [**软件的物料清单**](https://www.synopsys.com/blogs/software-security/building-sbom-with-black-duck/?cmp=pr-sig&utm_medium=referral)**【SBOM】。如果一家汽车制造商不跟踪其零部件的来源，它就不会长久。然而，太多的组织对他们正在使用的软件产品中的组件或他们依赖的其他组件知之甚少，甚至一无所知——这创造了一个供应链，它可以运行几个层次，并涉及数百到数千个所谓的依赖关系。**

这使得软件供应链成为一个巨大而有吸引力的攻击面，需要比现在更好的保护。如果有人需要提醒，去年 12 月在名为 Log4Shell 的 Apache 开源日志库 Log4j 中发现的灾难性漏洞就是一个例子。造成灾难性后果的原因之一是，太多的 Log4j 用户不知道他们使用的版本是否存在漏洞。他们没有跟踪他们的供应链。

这使得 SBOM——一个组织正在使用的每个软件组件的清单——如此重要。这也是 SCA 工具如此有价值的原因，因为它有助于找到这些组件，以及它们的版本和它们是否有已知的漏洞。

正如该报告所言，“许多公司有数百个应用程序或软件系统在使用，每个公司本身可能有成百上千个不同的第三方和开源组件，迫切需要一个准确、最新的 SBOM 来有效地跟踪这些组件。”

好消息是，现在比以往任何时候都更好地意识到了这种需要。Synopsys 最近发布的一份名为“[一往无前:GitOps 和 Shift Left Security](https://www.synopsys.com/software-integrity/resources/analyst-reports/gitops-and-shift-left-security.html) ”的报告发现，73%的受访者已经加大了保护其组织软件供应链的力度。拜登总统 2021 年 5 月的[“关于改善国家网络安全的行政命令”](https://www.whitehouse.gov/briefing-room/presidential-actions/2021/05/12/executive-order-on-improving-the-nations-cybersecurity/)明确呼吁公共和私营组织建立和维护 SBOMs。

*   不要让低风险缺陷变成高风险。标记为低风险的漏洞通常会获得该等级，因为它们不太可能造成太大的损害，或者因为攻击者不太可能利用它们。但是报告指出，根据组织的概况，这种漏洞可能会成为高影响/高可能性。一个例子是详细的服务器横幅漏洞，虽然被认为是低风险的，但“提供了诸如服务器名称、类型和版本号等信息，这些信息可能允许攻击者对特定的技术堆栈进行有针对性的攻击。”

软件安全建议总是带有免责声明:没有什么能让你刀枪不入。但是了解你的敌人会让你更接近那个目标。大多数攻击者的一个特点是他们寻找容易攻击的目标。当他们面对一个困难的目标时，大多数人倾向于继续前进。

遵循本报告中的建议可以帮助你加入那些更困难的目标。