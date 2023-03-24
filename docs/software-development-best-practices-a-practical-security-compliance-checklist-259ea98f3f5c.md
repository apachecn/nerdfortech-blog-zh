# 软件开发最佳实践:一个实用的安全性和遵从性清单

> 原文：<https://medium.com/nerd-for-tech/software-development-best-practices-a-practical-security-compliance-checklist-259ea98f3f5c?source=collection_archive---------14----------------------->

仅在过去的两年里，[我成功地拥有/领导了 4 个以上的软件产品交付，主要是在 AWS 上运行的美国卫生部门](https://www.linkedin.com/in/ruipedrosa/)。我还帮助编写安全合规报告。美国卫生部门的软件产品，尤其是当[受保护健康信息(PHI)](https://www.hhs.gov/answers/hipaa/what-is-phi/index.html) 出现时，经常根据安全&合规标准(如 [HIPAA](https://www.hhs.gov/hipaa/for-professionals/security/laws-regulations/index.html) / [HITRUST](https://hitrustalliance.net/) )进行审查，但是，实际上，我发现很难让**所有**开发人员完全了解 100 多个 pdf 文档和其他安全&合规最佳实践，所以我决定创建一个简单的“安全&合规清单”,这被证明更有效。

![](img/d50998a073f026cc7d26e2403098e391.png)

安全性和合规性

# 安全与合规性清单— AWS

✅ **独立 AWS 负责生产&非生产环境(最高级别的资源隔离)**。用 AWS 的话说，“您可以使用多个 AWS 帐户来隔离具有特定安全要求的工作负载或应用程序，或者需要满足 HIPAA 或 PCI 等严格的合规性准则。” [AWS 控制塔提供了一种最简单的方法，可以根据已确立的最佳实践来设置和管理安全的多帐户 AWS 环境](https://aws.amazon.com/controltower/)用我的一位客户的话来说，它就像盒子里的**合规性**，包括(但不限于):

*   预防性和检测性非安全/合规性检查:基于 AWS 最佳实践和常见客户治理政策，例如“禁止对亚马逊简单存储服务(亚马逊 S3)存储桶的公共写入访问”或“为连接到亚马逊弹性计算云(亚马逊 EC2)实例的亚马逊弹性块存储(亚马逊 EBS)卷启用加密”，现成的“预防性&检测护栏”和“强制性&可选护栏”
*   监控:具有生产帐户级别合规状态的仪表板
*   审计和日志记录:“[AWS 控制塔中的动作和事件的日志记录是通过与 CloudWatch 的集成自动完成的。所有操作都会被记录，包括来自 AWS 控制塔管理帐户和贵组织成员帐户的操作。在控制台的**活动**页面可以看到管理账户的动作和事件。可以在日志归档文件](https://docs.aws.amazon.com/controltower/latest/userguide/logging-and-monitoring.html)中查看会员账户的操作和事件。

# 安全与合规性清单—特定于医疗行业(HIPAA/HITRUST 要求)

✅ [仅使用 AWS HIPAA 合格服务](https://aws.amazon.com/compliance/hipaa-eligible-services-reference/)；

✅应用程序审计跟踪和日志记录。更具体地说:

*   每个临床医生对患者数据的操作都存储在审计表中。总是可以知道谁创建、更新、查看或删除了一段患者数据；
*   对于最敏感的表，平台维护以前值的完整时间戳历史，这允许将个别数据回滚到以前的状态；

通常，你可以通过使用一个库/框架很容易地实现这一点。例如，在一个[Asp.net 核心 app](https://docs.microsoft.com/en-us/aspnet/core/?view=aspnetcore-5.0) 中，我使用了[实体框架加审计](https://entityframework-plus.net/audit)

# 安全与合规性清单—常见内容

✅静态数据加密(数据库加密等。)

传输中加密的✅数据(HTTPS)

✅总是有一个正在使用的开源库的最新列表。例如，您可以有一个到您的`[package.json](https://docs.npmjs.com/cli/v7/configuring-npm/package-json#dependencies)`或`[.csproj](https://docs.microsoft.com/en-us/dotnet/core/project-sdk/msbuild-props#packagereference)`文件的链接

✅代码会定期检查 [OWASP 十大](https://owasp.org/www-project-top-ten/)安全漏洞。因为。我也喜欢看[网络安全备忘单](https://cheatsheetseries.owasp.org/cheatsheets/DotNet_Security_Cheat_Sheet.html)

✅[基础设施是使用代码(也称为 IaC)提供的，并被作为构建](/nerd-for-tech/a-hipaa-hitrust-compliant-aws-cdk-app-template-based-on-aws-best-practices-for-high-availability-24658d6be3dd)的一部分运行的单元测试所覆盖。这些单元测试的一些例子可能包括检查，比如确保[生产数据库被放置在一个隔离的网络中(没有公共可用)](https://github.com/rfpedrosa/aws-cdk/blob/master/test/database.test.ts#L25)或者 [AWS S3 桶被设置了加密](https://github.com/rfpedrosa/aws-cdk/blob/master/test/storage.test.ts#L27)(静态加密)

✅使用[静态代码分析工具](https://en.wikipedia.org/wiki/List_of_tools_for_static_code_analysis)。使用不仅检查代码而且检查依赖关系的工具。最好将它们设置为您的管道的一部分。我对[sonar cube](https://www.sonarqube.org/)和 [Black Duck](https://www.blackducksoftware.com/) 有很好的体验，但肯定还有更多好的工具。至少，如果你用的是 GitHub，从现在开始你就没有借口不启用自动安全检查了；

# 安全与合规性清单—公司级

✅ 2FA 使能。例如，在一个客户 GitHub 帐户上，我将 [2FA 设置为组织级别的强制](https://docs.github.com/en/github/setting-up-and-managing-organizations-and-teams/requiring-two-factor-authentication-in-your-organization)；

✅:客户认识所有能接触到产品的人。未经事先批准，不得授予新的访问权限。如果人员离开，他们将自动离开(上车/下车检查清单的一部分)。我知道，记住基本的东西总是好的；

✅生产中的所有手动更改都是通过 4 眼策略完成的，但这种情况很少发生，因为一切都是通过代码部署的😎

✅所有的代码至少由一个人审查。在 GitHub 中，通常由代码所有者概念强制执行

测试涵盖了特别涉及敏感数据的✅授权规则，如[受保护的健康信息(PHI)](https://www.hhs.gov/answers/hipaa/what-is-phi/index.html)

# 总而言之:

**安全性和合规性**风险是所有组织的一个**持续关注点**，因此您应该将此列表视为一个起点，而不是一个完整甚至广泛的起点。例如， [AWS 安全产品涉及很多我没有提到的服务](https://aws.amazon.com/products/security/?nc=sn&loc=2)。我没有提到它，因为它们中的一些已经被用作 AWS 控制塔设置的一部分，或者仅仅是因为我写这篇文章时没有使用它们。也就是说，一切都是时间和优先级的问题，所以我更喜欢将注意力集中在自动化上。[IBM 的一项研究表明，大部分数据泄露是由人为错误](https://www.techrepublic.com/article/ibm-says-most-security-breaches-are-eue-to-human-error/)造成的，所以我更愿意花时间设置自动代码依赖扫描作为 CI/CD 管道的一部分，或者让[基础设施被单元测试](https://github.com/rfpedrosa/aws-cdk/tree/master/test)覆盖，这样[也能自动运行](/nerd-for-tech/a-hipaa-hitrust-compliant-aws-cdk-app-template-based-on-aws-best-practices-for-high-availability-24658d6be3dd)。

最后，我经常使用这个列表来:

👉提高组织层面的安全性和合规性意识。例如，我通常将这些列表作为项目/产品/公司入职清单的一部分。当我在 [Basecone](http://basecone.com/) 作为首席解决方案架构师/安全冠军帮助 6+团队(20+人)时，清单被证明是有效的；

👉帮助我为安全和合规性审核编写安全和合规性报告；

👉帮助我与客户讨论。例如，我用这个作为路线图讨论的起点；

我相信你有很多东西要补充，所以为什么不在评论框里给我留言，让我改进这个列表呢？它甚至可能成为一个 GitHub repo，每个人都可以贡献自己的力量。😎