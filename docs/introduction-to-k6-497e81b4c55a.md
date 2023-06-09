# K6 简介

> 原文：<https://medium.com/nerd-for-tech/introduction-to-k6-497e81b4c55a?source=collection_archive---------4----------------------->

# **什么是 K6**

“k6”是一个开源的、免费的、以开发人员为中心的负载测试工具，旨在使性能测试成为一个高效和愉快的过程。有了‘K6’，你将能够回归性能，及早发现问题，从而构建健壮的系统和应用。这个框架的后端是使用 GO 开发的，测试人员应该将 Javascript 编写成脚本。有两种不同的版本可供用户选择。开源供我们正常使用，云版本可以通过付费来扩展我们的测试。

# 关键特征

开发人员友好的 CLI 工具。
JavaScript 脚本 es 2015/ES6——支持本地和远程模块
检查和阈值——用于客观、自动的负载测试

# 用例

通常 k6 用户是开发、QA 和 DevOps 工程人员。他们利用 k6 来评估 API、微服务和网站的性能。与 k6 一起使用的常见实例有:

负载测试——K6 旨在最小化系统资源的使用。这是一款高性能工具，专为生产前和 QA 环境中的高负载测试而开发(峰值、压力、浸泡测试)。

性能监控——K6 为性能测试的自动化提供了极好的元素。为了持续监控生产环境的性能，您可以用最小的负载量运行测试。

# 利益

1.  易于使用的基于开发人员的 API 和 CLI。直观、通用、强大的 k6 API 和 CLI 应运而生。
2.  自动化测试——旨在自动化您的测试。将通过/失败标准用于您的绩效目标。
3.  Javascript 编写测试——用熟悉的脚本语言创建现实的负载测试。为了开发和管理您的测试套件，重用模块和 Javascript 库。
4.  多种储物可能性——单独使用很棒——如果组合起来更好！k6 很容易与标准 CI 系统链接，允许将测试结果交付给几种后端和格式，如 DataDog、Kafka、CloudWatch、NewRelic、JSON 和 CSV。
5.  高性能工具——K6 引擎是用 Go 编写的，它是最有效的负载测试工具之一。
6.  轻松扩展云—规模设计。在分布式环境或本地 PC 上的云中使用相同的测试。不同类型实施的内聚体验。

# 权衡取舍

k6 是一个 JavaScript 脚本化的高性能负载测试工具。这些架构容量的设计提供了几种补偿:

1.  不在浏览器中运行
    这意味着 k6 不能让网页和浏览器一样。这也意味着 API 支持的库将不兼容。通过绕过浏览器，系统资源的使用大大减少，使工具更加有效。对于负载测试网站，还是可以利用 k6 的。从记录的用户会话中，您甚至可以执行测试。
2.  不是在 NodeJS
    上运行的 JavaScript 一般不适合高性能。该工具本身是在 Go 中开发的，使用 JavaScript 执行时间来简化测试脚本，以达到最佳速度。您可以使用 webpack 捆绑 npm 模块，并在使用 NodeJS APIs 导入 npm 模块或库时将它们合并到您的测试中。