# 创建无服务器微服务“Hello World”REST API:循序渐进

> 原文：<https://medium.com/nerd-for-tech/create-serverless-microservice-hello-world-rest-api-step-by-step-502555dc48a1?source=collection_archive---------3----------------------->

![](img/03d26c6d5543f463a6b6c8c559381e32.png)

基本微服务的基本元素包括一个 API 端点和一个数据库。在本文中，我将展示如何构建一个“Hello World”微服务，我将在以后的文章中基于它来解释微服务设计模式。

本文是解释微服务设计模式系列的一部分

1.  【本文】创建无服务器微服务“Hello World”:一步一步来

[](https://www.linkedin.com/pulse/create-serverless-microservice-hello-world-step-elhousieny-phd%25E1%25B4%25AC%25E1%25B4%25AE%25E1%25B4%25B0/?trackingId=3xdxZckTF8ML9xqjVXYxLQ%3D%3D) [## 创建无服务器微服务“Hello World”REST API:循序渐进

### 基本微服务的基本元素包括一个 API 端点和一个数据库。在这篇文章中，我将…

www.linkedin.com](https://www.linkedin.com/pulse/create-serverless-microservice-hello-world-step-elhousieny-phd%25E1%25B4%25AC%25E1%25B4%25AE%25E1%25B4%25B0/?trackingId=3xdxZckTF8ML9xqjVXYxLQ%3D%3D) 

# [2。向微服务添加 Lambda](https://www.linkedin.com/pulse/create-serverless-microservice-hello-world-step-elhousieny-phd%E1%B4%AC%E1%B4%AE%E1%B4%B0-1c/)

[](https://www.linkedin.com/pulse/create-serverless-microservice-hello-world-step-elhousieny-phd%E1%B4%AC%E1%B4%AE%E1%B4%B0-1c/) [## 逐步创建无服务器微服务“Hello World ”:向微服务添加 Lambda

### 本文是解释微服务设计模式 1 系列文章的一部分。创建无服务器微服务“你好…

www.linkedin.com](https://www.linkedin.com/pulse/create-serverless-microservice-hello-world-step-elhousieny-phd%E1%B4%AC%E1%B4%AE%E1%B4%B0-1c/) 

3.[【本文】为 Mac 设置 AWS 无服务器开发环境](https://www.linkedin.com/pulse/setting-up-aws-serverless-dev-environment-mac-elhousieny-phd%E1%B4%AC%E1%B4%AE%E1%B4%B0/)

[](https://www.linkedin.com/pulse/setting-up-aws-serverless-dev-environment-mac-elhousieny-phd%E1%B4%AC%E1%B4%AE%E1%B4%B0/) [## 为 Mac 设置 AWS 无服务器开发环境

### 本文是解释微服务设计模式 1 系列文章的一部分。创建无服务器微服务“你好…

www.linkedin.com](https://www.linkedin.com/pulse/setting-up-aws-serverless-dev-environment-mac-elhousieny-phd%E1%B4%AC%E1%B4%AE%E1%B4%B0/) 

4.[向无服务器微服务添加 dynamo db](https://www.linkedin.com/pulse/4-adding-dynamodb-serverless-microservice-rany-elhousieny-phd%E1%B4%AC%E1%B4%AE%E1%B4%B0/)

[](https://www.linkedin.com/pulse/4-adding-dynamodb-serverless-microservice-rany-elhousieny-phd%E1%B4%AC%E1%B4%AE%E1%B4%B0/) [## 4.向无服务器微服务添加 DynamoDB

### 本文是解释微服务设计模式 1 系列文章的一部分。创建无服务器微服务“你好…

www.linkedin.com](https://www.linkedin.com/pulse/4-adding-dynamodb-serverless-microservice-rany-elhousieny-phd%E1%B4%AC%E1%B4%AE%E1%B4%B0/) 

完整代码可以在[https://github . com/ranyelhousieny/micro services _ AWS _ server less](https://github.com/ranyelhousieny/Microservices_AWS_Serverless)找到

==========================================

# 1.使用 API 网关构建无服务器 REST API 端点

可以为 REST 或 GraphQL 创建 API 端点。在本文中，我将使用 API Gateway 创建 REST 端点。如果您对 GraphQL 感兴趣，请遵循本文使用 AppSync 和 Amplify 构建 GraphQL 微服务[https://www . LinkedIn . com/pulse/building-app sync-graph QL-using-AWS-Amplify-rany-elhousieny/](https://www.linkedin.com/pulse/building-appsync-graphql-using-aws-amplify-rany-elhousieny/)

# 使用 AWS 控制台创建 API 网关

转到您的 AWS 控制台并搜索 API 网关

![](img/807d7505782ba7e2c40389a26449cb09.png)

选择公共 REST API

![](img/01b5eeee31de29dbddaa0e2c1243d05f.png)

按照下图中的步骤操作:

![](img/f783aa6a14a5db221fbc89512e8dbff8.png)

现在，您将看到创建动作和资源的屏幕。资源是您希望使用 API 访问的资源。选择创建资源

# 创建资源

![](img/1e4f440a859d1784f5f86cb256959782.png)![](img/609318b863eb5c43eb13a4f4cfa45ad5.png)![](img/82c6a4b95e56a520ab824d9c9477b534.png)

# 创建 REST 方法

一种方法是 http CRUD 动词(API)，例如(GET、POST、DELTE、UPDATE)。

![](img/306ec7166eba21161ae106bf9171160e.png)

从消息菜单中选择获取方法

![](img/c0c360648ab0c78e1747fed62cd01009.png)![](img/5a91911be562bed839da71ecf906c9a5.png)![](img/9d49ffd0c5793d6a4040fcdaa7cda1bb.png)

现在选择模拟响应

![](img/ea092fd555f229cdcd3bb150950b393d.png)

您将获得以下屏幕。不要担心稍后会解释它，但是现在让我们通过集成一个响应来返回一个简单的“Hello World”消息

![](img/55c3e59536068263f2c3970f429c8d0e.png)![](img/5950d14d2908afe01129792a3eb78193.png)![](img/ed59c0be7ee95e8a7c6c0748512bdcfa.png)

让我们在浏览器上部署和尝试

![](img/d3aadaf533959a2683449c315ddc2477.png)![](img/af14d5cd7e307ca2427f68155907c891.png)

点击 url 并在末尾添加/消息

![](img/83d0a50ad2492c076d305b0068d7e208.png)![](img/0f44d086e63968aa32504ff3432635ca.png)

您可以使用 fetch 从 React 前端调用这个 API，如下文所述，但是将 url 更改为您刚才使用的 URL[这里](https://www.linkedin.com/pulse/fetch-data-redux-thunk-react-native-app-rany-elhousieny-phd%E1%B4%AC%E1%B4%AE%E1%B4%B0/)

发布者

![](img/9c0a9a45b3a97bcbbfd20d3c953ba2e4.png)

[状态在线](https://www.linkedin.com/in/ranyelhousieny/)

[phdᴬᴮᴰ雷尼·埃尔豪斯尼](https://www.linkedin.com/in/ranyelhousieny/)

高级经理软件工程师，AWS 解决方案架构师认证，PSM，ACSA，MIS，UPE，MSCA

[32 篇文章](https://www.linkedin.com/in/ranyelhousieny/detail/recent-activity/posts/)