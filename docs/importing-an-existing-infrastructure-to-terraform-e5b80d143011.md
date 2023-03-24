# 将现有基础设施导入 Terraform。

> 原文：<https://medium.com/nerd-for-tech/importing-an-existing-infrastructure-to-terraform-e5b80d143011?source=collection_archive---------2----------------------->

![](img/fee8629c48c8591e36f893e27ef401e0.png)

希望搜索引擎在你意识到你提到的实际地形在这里之前，可能已经带你去过 NASA 的网站了。别担心，你不是一个人。

每个公司都带着某些期望和假设迁移到公共云。管理层*期望*更好的成本优化，产品团队*期望*超快速的产品交付，开发团队*期望*更少的维护。但是这些期望通常不会实现。

人们经常如此字面地看待牛和宠物，以至于他们不关心有多少资源正在被创造，谁创造了这些资源，有多少正在被使用，它们是否被充分地使用，等等。

IaC(Infrastructure as Code)并不是解决所有这些问题的灵丹妙药，但是使用 IaC 管理您的基础设施有助于保持健康，最终有助于解决上述棘手问题。

一旦我们将基础设施转化为代码，接下来就是软件了。一旦它是一个软件，那么我们就可以利用我们从过去学到的软件工程原则和最佳实践。

# **比赛开始了……**

将应用程序/服务器迁移到公共云总是发生在最佳阶段。DevOps 团队将冲向终点线。

![](img/ebb2b9f25d4aaf8304bb657195ec5e24.png)

在这一点上适应 IaC 就像在最好的一圈换轮胎一样。整个迁移团队都应该了解这项技术(比如说，Terraform)。你需要几个真正精通这方面的人，这样你才能走得更快。最重要的是，你必须承担撞上任何路障的风险。因此，人们走传统的和臭名昭著的“点击并建立”路线。

# 下一个最佳适应时间

迁移到 IaC 的最佳时间是在迁移的开始阶段，但是如果您错过了这个时间，您还有机会。在本文中，我们将讨论几个 Terraform 导入工具。

# 编码的基础设施

Terraform 中的 IaC 代码由两个主要部分组成。

1.  ***”。tf"*** 文件，这是我们用 HCL 语言写的。
2.  terraform 根据我们在**中提供的 HCL 定义创建的 ***【状态文件】*** 。tf"** 文件。

将现有基础设施导入 IaC 包括生成 HCL 脚本和状态文件。

# 无效的“进口土地”

Terraform 附带了一个“导入”工具，如果你有手写的 HCL 定义(至少你需要一个实体模型定义)，使用它你可以只导入**基础设施的状态**。

对于一个或两个资源，手动编码 terraform HCL 定义并运行 terraform import 来生成状态文件是很好的。但是以这种方式进口大型基础设施是一场噩梦。

# 开源导入工具

这些开源导入工具帮助我们生成 **HCL 定义。**一旦你有了定义，这些工具使用相同的“地形导入”来生成状态文件。

很少有非常流行的工具可以将现有资源导入 Terraform。基于他们是如何创造的”。tf 文件”，我们可以把它们分为基于**模板的**和 **API 驱动的**。

## 基于模板:地形化[不活跃]

Terraforming 是一个非常灵活的工具，可以将基础设施导入 terraform。这个基于 Ruby 的工具是 terraform 之外的第一批导入工具之一。这使得它最受欢迎。它还可以将新生成的状态文件与任何现有的状态文件合并。这在其他选项中是不可用的。

不幸的是，该项目目前没有得到积极的发展。其次，这基于预定义的模板。即。该工具通过使用预定义的模板来生成 HCL 脚本/资源块。这使得它很难维持。terraform 资源定义中的任何更改都可能会破坏代码，除非您手动将其添加到模板中。

[](https://github.com/dtan4/terraforming) [## dtan 4/地形改造

### 将现有 AWS 资源导出为 Terraform 样式(tf，tfstate)需要 Ruby 2.3 或更高版本 Terraform v0.9.3 或…

github.com](https://github.com/dtan4/terraforming) 

## API 驱动:terraformer，terracognito

与“terraforming”和其他基于模板的工具不同，这些工具使用 terraform SDK 来识别资源模式并生成适当的 HCL 资源定义。这使得它更容易适应任何变化。

列出一些限制，这些工具似乎没有合并状态文件的特性。每种类型的资源(比如所有 AWS EC2instances)都有自己的状态文件。但好的一面是，对于大型基础设施，为每个资源保存单独的状态文件总是更好，以避免爆炸半径和时间。否则，运行简单的 terraform 应用/计划必须刷新整个基础架构。是的，您可能会失去资源依赖性，因为资源是分开管理的。

[](https://github.com/cycloidio/terracognita) [## cycloidio/terracognita

### 将您当前的云基础架构作为代码 Terraform 配置(HCL)导入到基础架构，或/和导入到…

github.com](https://github.com/cycloidio/terracognita) [](https://github.com/GoogleCloudPlatform/terraformer) [## Google cloud platform/terra former

### 基于现有基础设施生成 tf/ json 和 tfstate 文件的 CLI 工具(反向地形)。免责声明…

github.com](https://github.com/GoogleCloudPlatform/terraformer) 

# 奖励:导入工具作为漂移识别

无论流程有多长，都可能存在手动/意外更改。这些需要被适当地跟踪。

导入工具有助于识别基础架构中的手动/意外更改。运行计划的无服务器运行“terraform 导入工具”将生成当前基础架构的 HCL 定义。当对照生产状态文件运行时，将给出基础结构中的差异。有助于保持卫生。

# 结论

这些工具支持所有主要的云提供商。它们有自己的局限性，并且是为通用用例设计的。因此，可能需要进行一些更改才能在定制基础架构中使用。还有，需要严格的测试，“terraform 计划”是你的朋友。