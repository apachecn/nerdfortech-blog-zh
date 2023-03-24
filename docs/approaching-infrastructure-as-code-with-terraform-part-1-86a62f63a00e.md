# 初学者使用 Terraform 将基础设施视为代码

> 原文：<https://medium.com/nerd-for-tech/approaching-infrastructure-as-code-with-terraform-part-1-86a62f63a00e?source=collection_archive---------13----------------------->

![](img/7cf8a201a58396458bd0cc3ce3b4b517.png)

Terraform 是一个很棒的配置管理工具，在云世界中用来提供云中的资源。当涉及到管理和编排云中的架构时，它是最棒的自动化工具之一。它有能力吸收数据库、CI/Cd 和许多其他很酷的东西，使应用程序架构更容易处理和管理。Terraform 的声明性很强，只是刷新状态；不重复资源。

# 核心概念

## 供应者

在将 terraform 连接到任何云提供商、数据库、管道或任何其他外部服务时，有必要拥有提供商文件。它直接负责对平台 API 的理解。通俗地说，它知道如何与任何外部技术交流，你可以查看[文档](https://registry.terraform.io/browse/providers)了解不同技术及其提供商的一些细节。更像是将 terraform 代码翻译成第三方提供商能够理解的东西。

```
Example:provider “aws” {region = “us-west-2”access_key = “my-access-key”secret_key = “my-secret-key”}
```

## 资源

要在云上使用和编排的服务和资源需要在 terraform 文件的代码块中正确定义。您可以在这里描述 vpc、子网和其他类型的资源。语法为“资源名称”“内部变量名”；第一部分由 AWS 给出，而第二部分由工程师定义。

```
Example:resource “provider-prefix_name-of-resources” “internal-preffered-name” {count = 3vpc = true}
```

## 数据

在创建资源时，需要过滤一些查询来为特殊的后续操作指定某些资源，这就是数据发挥作用的地方。

```
Example:data “provider-prefix_name-of-resources” “internal-preffered-name” {default = true}
```

然后，您可以立即创建一个资源块来执行任何所需的操作。

## 输出

这用于显示 terraform 应用命令执行完毕且基础设施/资源已经运行后的特定结果；它可以用来显示子网 ID，VPC ID 和其他。

```
Exampleoutput “reource-id” {value = “”}
```

## 可变的

用于声明可以在语法的所有部分引用的某些值。通过使用 terraform apply 并手动插入值，可以为变量赋值。此外，您可以通过使用 terraform apply 命令将该值作为参数传递来分配该值。最后，您可以通过用变量文件赋值来实现这一点，这是最佳实践

```
Example:variable “image_id” {type **=** string}
```

## Terraform 命令

```
**Terraform init**
```

这是为了初始化工作目录，并且在模块化地形配置时非常有用。

```
**Terraform plan**
```

该命令将比较所需状态和当前状态，并给出将要删除、更改、创建或替换的内容的草稿。

```
**Terraform apply**
```

这个命令只是创建必要的资源，并确保完成所需的状态。

```
**Terraform destroy**
```

如果您必须清除云提供商或其他地方的所有资源，这个命令可以帮助您安全地删除它们。

## **不适用**

使用 terraform plan 查看参考资料，用于从配置文件中检查当前状态和期望状态之间的差异。您可以为 apply 命令传递 in -auto-approve 标志，让它自动应用。对于 destroy 命令，如果你想指定某个资源，那么你可以添加一个标志

## **设置凭证**

设置凭证与配置过程一样重要，因为它提供了对相关平台的基本访问。在 AWS 上设置时，有两种主要方法可用于实现这一点，它们包括:

*   通过以下格式导出所有凭据:

```
access_secret_key=”XXXX” 
```

然后，您可以毫无问题地应用 terraform 资源。

*   在中配置凭据。/aws 目录，然后运行:

```
aws configure
```

并按照列出的步骤进行。

## **版本控制**

这用于跟踪存储库中的代码，对于维护代码基础设施的真实性非常重要。打开一个库并根据需要推送代码是很好的，如果你不知道如何推送，那么检查这个[文档](https://docs.github.com/en)。此外，有些文件不必放在存储库中，而是放在。gitignore 文件如。terrform/provider、状态文件、tf 变量文件和其他。VCS 的一些原因包括:

*   *妥善保管。*
*   *历史变迁。*
*   *团队协作。*
*   *审查有合并请求的基础设施。*

## 使用 Terraform 的十大最佳实践

1.  *对配置进行参数化，使代码可与输入变量一起重用，这样就可以在代码库的不同部分调用它*
2.  *对于变量，可以通过用变量文件赋值来实现；****terraform . tfvars .****这有助于在不同环境下复制基础设施。*
3.  *在变量块中分配变量时，使用等号。*
4.  *不要在主文件中硬编码凭证和秘密，而是使用****terraform . TF vars****来存储它们。*
5.  *申请前先学会使用 terraform 计划，了解各州的区别。*
6.  *不要把秘密文件推送给 git。*
7.  模块化你的地形配置。
8.  *将远程文件存储在像 S3 这样的存储单元和其他可用的对象存储选项中。*
9.  *为每个有输入输出变量的模块生成* ***自述*** *。*
10.  *在地形状态文件桶上启用版本控制。*

> 总的来说，Terraform 给你一个机会去了解什么可行，什么不可行，然后把它应用到你的下一个基础设施中，并重复使用。

还有，会有更多关于 Terraform 的文章，留在身边。现在，请记住，这篇文章不仅仅是为软件领域的专家准备的，即使是新手也可以加入并学到很多东西，这就是为什么我试图用外行和专业术语将一切都讲清楚，所以如果你有任何问题，可以通过 [**Twitter**](https://twitter.com/SamuelArogbonlo) **联系我，或者通过**[**GitHub**](https://github.com/samuelarogbonlo)**找到我。**

**感谢阅读❤️**

如果你对这个话题有任何想法，请留下评论——我乐于学习和探索知识。

# 我可以想象这个帖子有多有用，请留下掌声👏下面几次以示对作者的支持！此外，如果你需要一个咨询和自由职业的 DevOps 工程师，我就是你要找的人；雇用我，让我们完成这个项目。