# 面向开发人员的 FHIR:第 4 部分

> 原文：<https://medium.com/nerd-for-tech/fhir-for-developers-part-4-f1f45d232579?source=collection_archive---------1----------------------->

FHIR 中的数据类型

与任何其他语言或规范一样，FHIR 有一组数据类型用于定义资源元素。大多数数据类型与任何其他编程语言都非常相似，但与 FHIR 中定义的这些基本数据类型一起，它们将服务于医疗保健领域中一些特殊而令人兴奋的用例。

本文将深入探讨 FHIR 中的不同数据类型及其背后的用例。

在 FHIR 中，基本上有四类数据类型:

**原始类型**:简单/原始类型，是具有原始值的单个元素。其中包括许多其他编程语言中使用的常见数据类型。下面是一些可用的原始数据类型的例子。

**串串**:“病人做得好”

**布尔**:真/假

**日期**:2019–02–09

**十进制** : 1234.00001

**整数** : 1234

**uri**:https:/medi . info/set**(是！由于在 FHIR 数据集中广泛使用 URI/URL，FHIR 为 URI/URL 提供了单独的数据类型)**

**base64 binary**:bhg 6868 KC 989..**(以二进制存储图像和文档)**

**日期时间**:2013–06–08t 10:57:34+01:00

**瞬间**:2013–06–08t 10:57:34.099+01:00

> 日期时间和瞬间之间的差异是精确的。dateTime 通常用于人类时间，与用于机器时间的 instant 相比，它的精度较低。此外，日期时间的精度是可变的，例如，医生会给你日期时间格式的处方。

![](img/57b517a87b0aa5b48894886fd2c4656e.png)

原始数据类型

**通用类型:**通用复杂类型，是可重用的元素簇。这些是在规范的不同部分和地方重用的微型结构。通用数据类型的一个很好的例子是人名。

![](img/ab107604b9d8c690e34e134afed92d74.png)

通用数据类型

**在 FHIR 规范中的多个地方使用了 HumanNames** 来定义不同角色的名字，如病人、医生等。

![](img/5b95aaca7d6b4dbe989245b832bcbf4b.png)

人类名称数据类型

每当我们需要存储遵循某些特定术语的值时，就会使用 CodeableConcept。存储病人的疾病或药物。我们可以使用术语服务器来为我们提供这些值。

![](img/1afc0b84837b33a292f55a6be14100db.png)![](img/d45c6cc6debc0d7baa07066c1fbc43d0.png)

可编码概念

> 您可能已经注意到 0..1, 1..1, 0..*上图中。这代表了**基数。简单地说，基数告诉你一个特定的字段可以容纳多少个值。0..1 代表 0 或 1 类似地代表 0..*表示 0 或更多。**

**元数据类型:**一组用于元数据资源的类型。

![](img/aacacf7f9c26c11290ddbfaae91b12a7.png)

元数据数据类型

**ContactDetail** :用于存储任何人的联系方式。

![](img/52bc5aaf5260241148ccc4e1a70c9873.png)

'联系人详细信息'

**数据需求:**数据需求结构定义了知识资产的一般数据需求，例如决策支持规则或质量度量。

![](img/a7cd0d85558c052c7fb17125e7c9a640.png)

数据需求

**特殊用途类型**:这些类型是在规范中为某些特定用途而定义的。

![](img/d55ae7fb18598b9dbbf1aecd09f66a8e.png)

特殊用途数据类型

**引用**:一个资源中的许多已定义元素都是对其他资源的引用。引用总是在一个方向上定义和表示——从一个资源(源)到另一个资源(目标)。从目标到源的对应反向关系存在于逻辑意义上，但通常不会在目标资源中明确表示。

![](img/7b65bf1fa0759d0912d0eeb622cfde60.png)

参考

**Meta:** 用于表示任何资源的信息。它包括关于资源的信息，如配置文件、版本、标识等。不要将这与元数据数据类型混淆，二者在 FHIR 中有不同的用途。

![](img/505cbfa5e3baf25002e6555d304d5420.png)

元

本文包括一些重要的数据类型。FHIR [文档](https://www.hl7.org/fhir/datatypes.html#Coding)中有许多其他数据类型。你可以查看[文档](https://www.hl7.org/fhir/datatypes.html#Coding)了解更多细节。

轻松点说:

![](img/15ce1283cdfac822a9d842c1a13c4786.png)

如果你喜欢我的作品，请**喜欢并分享**这篇文章(**免费:)**)。还有，做 [**关注**](/@jaideeppahwa1) me 更多这样的文章。

另外，看看我的其他文章:

![Jaideep Pahwa](img/8ecddd40c56ab22f8d22e46eb84085dc.png)

杰迪普·帕瓦

## 5 分钟技术

[View list](/@jaideeppahwa1/list/5-minutes-tech-c6f26ea4a89c?source=post_page-----f1f45d232579--------------------------------)3 stories![](img/44b836fb056b352b71d24d80ea5dae58.png)![](img/1640460e6964a54b2ef94838a37070c2.png)![](img/ad1109593cd4318caaf0ebf73bd2b541.png)![Jaideep Pahwa](img/8ecddd40c56ab22f8d22e46eb84085dc.png)

[贾迪普·帕瓦](/@jaideeppahwa1?source=post_page-----f1f45d232579--------------------------------)

## 面向开发人员的 FHIR

[View list](/@jaideeppahwa1/list/fhir-for-developers-ea551cc4840c?source=post_page-----f1f45d232579--------------------------------)9 stories![](img/69baf9af856005b059eaa2afda58633e.png)![](img/860b5ffbe20ce4d7ca2a6e8868e3c31c.png)![](img/f911695f9a806c105eff8fac3966ba3e.png)![Jaideep Pahwa](img/8ecddd40c56ab22f8d22e46eb84085dc.png)

[贾迪普·帕瓦](/@jaideeppahwa1?source=post_page-----f1f45d232579--------------------------------)

## 自助救助

[View list](/@jaideeppahwa1/list/self-help-942c66816c1d?source=post_page-----f1f45d232579--------------------------------)2 stories![](img/1caa702bb1ef13ea85dc5b3eab487300.png)![](img/f605fd6dc16d796d47f5ca4ed1a54278.png)![Jaideep Pahwa](img/8ecddd40c56ab22f8d22e46eb84085dc.png)

[贾迪普·帕瓦](/@jaideeppahwa1?source=post_page-----f1f45d232579--------------------------------)

## 通用技术公司

[View list](/@jaideeppahwa1/list/general-tech-e702a6db69b5?source=post_page-----f1f45d232579--------------------------------)2 stories![](img/818dc6544d8e4d29e50db65eca07f935.png)![](img/c5e98c5b77b4b3cbf5e77f309ce76a95.png)