# 面向开发人员的 FHIR:第 0 部分

> 原文：<https://medium.com/nerd-for-tech/fhir-for-java-developers-series-20ac91ed60b8?source=collection_archive---------2----------------------->

## 系列介绍

在这个数字化转型和创新的时代，数据收集和共享成为组织最重要的任务之一。正如所言**数据是新的黄金**。虽然数据收集至关重要，但同样重要的是数据的互操作性和共享。如果没有数据互操作性，这些数据将留在孤岛中，无法得到充分利用。

**什么是数据互操作性？**

数据互操作性是便于在不同系统之间无限制地共享和使用数据或资源的属性。要使两个或更多的系统具有互操作性，它们必须能够以对方能够理解的方式交换、解释和呈现共享数据。为了建立数据互操作性，需要设置一组协议，数据的发送者和接收者都需要遵循这些协议。不同的行业可能遵循共同的数据互操作性标准，这些标准遵循并涵盖了行业特定的标准和要求。

**FHIR(快速医疗保健互操作性资源)**是一种这样的数据互操作性标准，其在医疗保健行业中被广泛使用和采用，用于共享医疗保健相关信息。FHIR 继续为开发互操作性和健康信息交换的基于应用的方法提供了很大的希望。

![](img/f561ba272613f3e2520c7e2a03204ec4.png)

医疗保健数据有许多旧标准，但其中一个 FHIR 是在牢记开发人员第一的方法的基础上开发的。这不仅创建了流行的医疗行业标准，还帮助开发人员快速了解板载，并结合 FHIR 使用和利用其他新兴技术。

在本系列的后续文章中，我们将深入探讨 FHIR 标准及其不同的特性和优势。我们还将看看使用 JAVA 和其他库的实现和使用。

如果你喜欢我的作品，请**喜欢并分享**这篇文章(**免费:)**)。还有，做 [**关注**](/@jaideeppahwa1) me 更多这样的文章。

另外，看看我的其他文章:

![Jaideep Pahwa](img/8ecddd40c56ab22f8d22e46eb84085dc.png)

[贾迪普·帕瓦](/@jaideeppahwa1?source=post_page-----20ac91ed60b8--------------------------------)

## 5 分钟技术

[View list](/@jaideeppahwa1/list/5-minutes-tech-c6f26ea4a89c?source=post_page-----20ac91ed60b8--------------------------------)3 stories![](img/44b836fb056b352b71d24d80ea5dae58.png)![](img/1640460e6964a54b2ef94838a37070c2.png)![](img/ad1109593cd4318caaf0ebf73bd2b541.png)![Jaideep Pahwa](img/8ecddd40c56ab22f8d22e46eb84085dc.png)

杰迪普·帕瓦

## 面向开发人员的 FHIR

[View list](/@jaideeppahwa1/list/fhir-for-developers-ea551cc4840c?source=post_page-----20ac91ed60b8--------------------------------)9 stories![](img/69baf9af856005b059eaa2afda58633e.png)![](img/860b5ffbe20ce4d7ca2a6e8868e3c31c.png)![](img/f911695f9a806c105eff8fac3966ba3e.png)![Jaideep Pahwa](img/8ecddd40c56ab22f8d22e46eb84085dc.png)

[贾迪普·帕瓦](/@jaideeppahwa1?source=post_page-----20ac91ed60b8--------------------------------)

## 自助救助

[View list](/@jaideeppahwa1/list/self-help-942c66816c1d?source=post_page-----20ac91ed60b8--------------------------------)2 stories![](img/1caa702bb1ef13ea85dc5b3eab487300.png)![](img/f605fd6dc16d796d47f5ca4ed1a54278.png)![Jaideep Pahwa](img/8ecddd40c56ab22f8d22e46eb84085dc.png)

[贾迪普·帕瓦](/@jaideeppahwa1?source=post_page-----20ac91ed60b8--------------------------------)

## 通用技术公司

[View list](/@jaideeppahwa1/list/general-tech-e702a6db69b5?source=post_page-----20ac91ed60b8--------------------------------)2 stories![](img/818dc6544d8e4d29e50db65eca07f935.png)![](img/c5e98c5b77b4b3cbf5e77f309ce76a95.png)