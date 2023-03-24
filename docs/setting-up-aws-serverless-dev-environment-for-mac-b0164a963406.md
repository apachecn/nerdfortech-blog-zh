# 为 Mac 设置 AWS 无服务器开发环境

> 原文：<https://medium.com/nerd-for-tech/setting-up-aws-serverless-dev-environment-for-mac-b0164a963406?source=collection_archive---------3----------------------->

![](img/d934d38eb740e78ad022e26e5637f9f7.png)

本文是解释微服务设计模式系列的一部分

1.  [打造无服务器微服务“Hello World”:一步一步来](https://www.linkedin.com/pulse/create-serverless-microservice-hello-world-step-elhousieny-phd%25E1%25B4%25AC%25E1%25B4%25AE%25E1%25B4%25B0/?trackingId=3xdxZckTF8ML9xqjVXYxLQ%3D%3D)

[](https://www.linkedin.com/pulse/create-serverless-microservice-hello-world-step-elhousieny-phd%25E1%25B4%25AC%25E1%25B4%25AE%25E1%25B4%25B0/?trackingId=3xdxZckTF8ML9xqjVXYxLQ%3D%3D) [## 创建无服务器微服务“Hello World”REST API:循序渐进

### 基本微服务的基本元素包括一个 API 端点和一个数据库。在这篇文章中，我将…

www.linkedin.com](https://www.linkedin.com/pulse/create-serverless-microservice-hello-world-step-elhousieny-phd%25E1%25B4%25AC%25E1%25B4%25AE%25E1%25B4%25B0/?trackingId=3xdxZckTF8ML9xqjVXYxLQ%3D%3D) 

2.向微服务添加 Lambda

[](https://www.linkedin.com/pulse/create-serverless-microservice-hello-world-step-elhousieny-phd%E1%B4%AC%E1%B4%AE%E1%B4%B0-1c/?published=t) [## 逐步创建无服务器微服务“Hello World ”:向微服务添加 Lambda

### 本文是解释微服务设计模式 1 系列文章的一部分。创建无服务器微服务“你好…

www.linkedin.com](https://www.linkedin.com/pulse/create-serverless-microservice-hello-world-step-elhousieny-phd%E1%B4%AC%E1%B4%AE%E1%B4%B0-1c/?published=t) 

# **3。[本文]为 Mac 设置 AWS 无服务器开发环境**

[](https://www.linkedin.com/pulse/setting-up-aws-serverless-dev-environment-mac-elhousieny-phd%25E1%25B4%25AC%25E1%25B4%25AE%25E1%25B4%25B0/?trackingId=2YpjFH5BDk02EyEpp4APwQ%3D%3D) [## 为 Mac 设置 AWS 无服务器开发环境

### 本文是解释微服务设计模式 1 系列文章的一部分。创建无服务器微服务“你好…

www.linkedin.com](https://www.linkedin.com/pulse/setting-up-aws-serverless-dev-environment-mac-elhousieny-phd%25E1%25B4%25AC%25E1%25B4%25AE%25E1%25B4%25B0/?trackingId=2YpjFH5BDk02EyEpp4APwQ%3D%3D) 

完整代码可以在[https://github . com/ranyelhousieny/micro services _ AWS _ server less](https://github.com/ranyelhousieny/Microservices_AWS_Serverless)找到

=====================================

在 AWS 控制台上搜索 Lambda

![](img/144d61f24fe581fda5420026f0ad6d9d.png)

单击创建函数

![](img/67647d7905337323c4066ae2e849f0ad.png)

请遵循以下步骤

![](img/d61b2d1fbd0af7a1f1e17f3f3c46398e.png)![](img/391b25be610d06a929e9f20831c5dbcf.png)

# 将 Lambda 添加到 API 网关

转到 API 网关，从 Mock 切换到 Lambda

![](img/2c074bee79b84d703b94e1c87290e0e8.png)![](img/ec433f65f475cdd4815cf88c1bb9707e.png)

现在你可以看到 API 网关与 Lambda 交互，而不是模拟响应

![](img/d2b15922d61e6bcbbe0547b2e20b8d76.png)

你可以通过点击 test 来测试它，你会收到来自 Lambda 的消息

![](img/5971cba5316176104579217fdadd01a9.png)

像以前一样进行部署，并在浏览器上进行测试，以查看以下内容

![](img/1eaf16f1e6613d7ee02fff93988fa938.png)