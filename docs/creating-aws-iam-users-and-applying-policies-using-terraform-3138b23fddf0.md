# 使用 Terraform 创建 AWS IAM 用户和应用策略

> 原文：<https://medium.com/nerd-for-tech/creating-aws-iam-users-and-applying-policies-using-terraform-3138b23fddf0?source=collection_archive---------0----------------------->

我们可以使用 AWS 身份和访问管理(IAM)创建用户和组，并通过权限管理他们对 AWS 服务和资源的访问。

# **IAM 用户**

**创建 IAM 用户:**

要在 Terraform 中创建单个 IAM 用户，请创建一个 aws_iam_user 资源块并为其命名。如果我们只需要创建一个用户，这是一个相对简单的步骤。

```
resource "aws_iam_user" "demouser" { name = "tuckerdemo"}
```

**创建多个 IAM 用户:**

创建多个 IAM 用户有不同的方法。

我们可以从我们的第一个用户复制并粘贴资源块，并为后续的块赋予新的名称。然而，这会导致代码的重复，并且对于创建许多用户来说效率非常低，因为我们需要手动调整每个用户的名称。

count 元参数可用于复制和创建资源，次数不限。在这种情况下，3。

```
resource "aws_iam_user" "demo" { count = 3 name = "tuckerdemo"}
```

以这种方式使用 **count** 的问题是所有 3 个用户都有相同的名字“tuckerdemo”。

在名称后使用 **count.index** 将给出对应于资源块每次迭代的不同索引号。

```
resource "aws_iam_user" "demo" { count = 3 name = "tuckerdemo.${count.index}"}
```

但是，如果我们的用户需要不以数字开头和结尾的唯一名称，那么如果没有进一步的定制，这种方法就不合适。

如果我们将 **count.index** 与插值函数相结合，我们可以进一步定制计数的每次迭代。插值函数**长度**和**元素**可用于此。

**element** (list，index) —返回列表中给定索引处的单个元素。如果索引大于元素的数量，这个函数将使用标准的 mod 算法换行。该函数仅适用于平面列表。

**length** (list) —返回给定列表或映射中的成员数，或给定字符串中的字符数。

唯一用户名的列表可以添加到一个单独文件的变量中，然后在我们的资源块中引用。让我们创建一个用户名变量。

```
variable "username" { type = list(string) default = ["tucker","annie","josh"]}
```

对于下面的代码，对于用户名列表中的 3 个用户，**长度**函数将返回值 3。**元素**函数将返回用户名列表中与**计数**参数的不同索引号相关联的单个名称([0]="tucker "，[1]="annie "，[2]="josh ")。

```
resource "aws_iam_user" "demo" { count = "${length(var.username)}" name = "${element(var.username,count.index )}"}
```

如果我们想输出我们创建的所有用户的 arn，我们可以用输出文件来实现。为“user_arn”创建一个输出块。值行中的“*”表示输出所有 aws_iam_user 的 arn。如果您只想输出特定用户的 arn，请将“*”更改为列表中该用户的索引号(0、1、2 等。).

```
output "user_arn" { value = aws_iam_user.demo.*.arn}
```

使用长度和元素的替代方法是使用 **for_each** 。

如果一个资源或模块块包含一个 **for_each** 参数，其值是一个映射或一组字符串，Terraform 将为该映射或集合的每个成员创建一个实例。在本例中，我们将它用于一组由用户名组成的字符串。

```
resource "aws_iam_user" "example" { for_each = toset(["tucker", "annie", "josh"]) name     = each.value}
```

需要注意的是，当在资源上使用时， **for_each** 仅支持集合和映射。我们需要使用 **toset** 将用户名列表转换成一个集合。当在资源块上使用 **for_each** 时，它变成了一个资源映射，而不是像 count 那样的资源列表。

# **IAM 策略**

IAM 策略授予 IAM 用户对 AWS 资源的访问权限。AWS 利用跨许多服务的标准 JSON 身份和访问管理(IAM)策略文档格式来控制对资源和 API 操作的授权。

IAM 策略由一个或多个语句组成，这些语句包括一个效果(允许或拒绝)、一个或多个操作(例如 ec2:Describe*)以及一个或多个资源(“*”是指所有资源，但要指向特定资源，我们可以输入 arn)。

在这个演示中，我们将使用下面显示的简单策略。当我们运行 plan 命令时，您将看到为 var.tf 文件中指定的每个用户创建了一个“newemp_policy”。

```
resource "aws_iam_user_policy" "newemp_policy" { count = length(var.username) name = "new" user = element(var.username,count.index)policy = <<EOF{ "Version": "2012-10-17", "Statement": [ { "Effect": "Allow", "Action": [ "ec2:Describe*" ], "Resource": "*" } ]}EOF}
```

这个关于使用 Terraform 创建 IAM 用户和策略的简短演示到此结束。有多种方法可以创建多个用户，因此请使用最能满足您需求的方法。 [AWS 策略生成器](https://awspolicygen.s3.amazonaws.com/policygen.html)是一个可以帮助您创建 JSON 策略文档的工具。