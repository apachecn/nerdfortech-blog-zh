# 解决与将 Node.js + PostgreSQL API 部署到 Heroku 相关的 CORS 错误

> 原文：<https://medium.com/nerd-for-tech/solving-cors-errors-associated-with-deploying-a-node-js-postgresql-api-to-heroku-229591a32ccd?source=collection_archive---------13----------------------->

## **第 2 部分:部署 GraphQL API**

在本指南的第一部分中，我介绍了在将 Node.js + PostgreSQL REST API 部署到 Heroku 时如何解决 CORS 错误。

[](/nerd-for-tech/solving-cors-errors-associated-with-deploying-a-node-js-postgresql-api-to-heroku-1afd94964676) [## 解决与将 Node.js + PostgreSQL API 部署到 Heroku 相关的 CORS 错误

### 第 1 部分:部署 REST API

medium.com](/nerd-for-tech/solving-cors-errors-associated-with-deploying-a-node-js-postgresql-api-to-heroku-1afd94964676) 

你可以通过上面的链接查看这篇文章，因为我会参考它，以避免重复我自己。

在第二部分中，我将详细介绍将 GraphQL API 部署到 Heroku stress-free 需要做的事情。但我们将采取不同的方法，即通过 Docker 将 GraphQL API 部署到 Heroku。

据 opensource.com[，](https://opensource.com/resources/what-docker) [Docker](https://www.docker.com/) 介绍，Docker 是一款 DevOps 工具，用于通过使用容器来创建、部署和运行应用程序。容器允许开发人员将应用程序与它需要的所有部分打包在一起，比如库和其他依赖项，并作为一个包进行部署。这个容器现在使得应用程序可以在任何其他 Linux 机器上运行，而不管该机器可能具有的任何定制设置是否与用于编写和测试代码的机器不同。

我假设你了解 GraphQL、Docker、Typescript 以及本指南第 1 部分中提到的其他技术。我们将使用 Apollo Server 作为 GraphQL 服务器，Typeorm 处理 Postgres 函数，Express-Session 用于会话和 cookie，Redis 用于存储 cookie。

在本指南中，我们将介绍以下步骤:

*   设置我们的服务器
*   在我们的服务器上配置 CORS
*   设置 Dockerfile
*   配置环境变量
*   部署到 Heroku

我们将使用的依赖项:

*   表达
*   阿波罗服务器快递
*   类型-graphql
*   约尔迪什
*   连接-redis
*   快速会话
*   字体
*   heroku-cli 全球安装
*   dotenv-安全
*   克-奥二氏分级量表

## **设置我们的服务器**

如果我们想避免在生产中出现 CORS 错误，以正确的方式设置我们的服务器是非常重要的。CORS 错误有时是由于错误的配置导致服务器无法设置 cookies。为了解决这个问题，我们的服务器设置需要如下所示:

## 在我们的服务器上配置 CORS

建议显式配置 CORS 以允许特定的客户端 URL。这可以通过以下方式实现:

在这种情况下，我们的客户端也需要进行配置，然后才能访问我们的 API。对于 Apollo 客户机，下面的配置就可以了:

## 设置 Dockerfile

首先，我们需要下载安装 Docker，并确保它在后台运行。如果你之前没有用过 Docker，建议你看一下 Academind 的这个完整的 Docker 教程，帮助你入门。

下一步是创建 Dockerfile 和. dockerignore 文件，将 node_modules 和我们的错误文件(yarn-error.log 或 package-error.log)添加到。dockerignore 文件。我们的 docker 文件需要看起来像这样:

这包含了 Docker 为我们的 API 创建 Docker 映像所需要运行的一步一步的过程。你可以通过下面的链接阅读更多关于如何 dockerize 你的 Node.js web 应用的内容。

[](https://nodejs.org/en/docs/guides/nodejs-docker-webapp/) [## 将 Node.js web 应用程序| Node.js 归档

### 这个例子的目的是向你展示如何将 Node.js 应用程序放入 Docker 容器。的…

nodejs.org](https://nodejs.org/en/docs/guides/nodejs-docker-webapp/) 

## 配置环境变量

我们的环境变量设置，无论是本地的还是 Heroku 上的，都将与 [part 1](/nerd-for-tech/solving-cors-errors-associated-with-deploying-a-node-js-postgresql-api-to-heroku-1afd94964676) 的情况相同。

## 部署到 Heroku

现在我们已经完成了服务器的设置，最后一步是部署到 Heroku。我们需要做的第一件事是通过 heroku-cli 登录 Heroku 和 Heroku 容器注册表:

接下来，我们需要创建一个 Heroku 应用程序，构建图像，将其推送到容器注册表，并将图像发布到我们的应用程序:

我们完了。如果您已经在生产中启用了 GraphQL playground，那么您可以在浏览器中打开您的 API heroku URL:

> https://yourherokuurl.com/graphql

或者您可以通过客户端使用您的 API。