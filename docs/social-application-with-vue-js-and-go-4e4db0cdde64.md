# 带有 Vue.js 和 GO 的社交应用

> 原文：<https://medium.com/nerd-for-tech/social-application-with-vue-js-and-go-4e4db0cdde64?source=collection_archive---------1----------------------->

![](img/4cdfb643ef90df60dcd8aaa1429ba169.png)

标志归功于 vuejs.org 和 golang.org

## 用 vue.js 和 golang 创建和服务一个类似 twitter 的应用程序第 1 部分:设置

在这一系列文章中，我想写下一个关于使用 vue.js 作为前端和后端作为网站/web 应用程序选择的小教程。这对组合似乎不是很受欢迎，但让我们想想这个:Go 实际上是一种非常快速的后端语言，由 google 构建，既快速又易于使用，而 Vue 是一个 javascript 框架，它将页面的快速呈现作为一个主要的重点。

说到这里，我认为将它们结合起来使用会非常好，让我们创建性能非常好的应用程序。

查看其他部分:

*   第 1 部分:设置(本部分)
*   [第二部分:VUE 入门](https://ivano-dalmasso.medium.com/social-application-with-vue-js-and-go-64b3adee8dac)
*   [第三部分:组件&插槽](https://ivano-dalmasso.medium.com/social-application-with-vue-js-and-go-24a1d1e7137d)
*   [第四部分:Vuex 首次设置](https://ivano-dalmasso.medium.com/social-application-with-vue-js-and-go-3a11d506fc38)
*   [第 5 部分:VUEX 终结](https://ivano-dalmasso.medium.com/social-application-with-vue-js-and-go-ef364b572422)
*   [第 6 部分:Vuex 中的表格和数据](https://ivano-dalmasso.medium.com/social-application-with-vue-js-and-go-a22a1afb76eb)
*   [第七部分:与 golang 服务器的连接](https://ivano-dalmasso.medium.com/social-application-with-vue-js-and-go-d9e563466b66)
*   [第 8 部分:基于令牌的认证](https://ivano-dalmasso.medium.com/social-application-with-vue-js-and-go-64978f7c381f)
*   [第 9 部分:存储索引为 DB 的认证令牌](https://ivano-dalmasso.medium.com/social-application-with-vue-js-and-go-4d0caa37ddac)

## 我们要建造什么

在本教程中，我们将创建一个小型社交网络应用程序，让用户进行身份验证，创建一些文本帖子，并对其他人发表的帖子进行评论。

在课程的大部分时间里，我会尽量不去深究，而是只讲那些能让一个人对一个项目“做好准备”的最基本的东西。

第一部分的代码可以在以下位置找到:

[后端](https://github.com/idalmasso/go-vue-tutorial-backend/releases/tag/v0.1)

[前端](https://github.com/idalmasso/go-vue-tutorial-frontend/releases/tag/v0.1)

在这第一章中，我们将建立我们的项目，并为接下来的部分做好准备。

## 安装

让我们在代码库中创建一个文件夹，它将包含我们所有的代码。我将把这个文件夹称为“主文件夹”。

在这里为项目添加两个存储库(即在 github 上创建并克隆它们)，然后我们将准备好设置这两个项目。从现在开始，我将把“前端文件夹”和“后端文件夹”称为包含两个存储库的两个文件夹。

第一点，我们去前端部分；我希望已经安装了 npm(如果没有，从[https://www.npmjs.com/get-npm](https://www.npmjs.com/get-npm)安装)。

我们需要安装 vue-cli，只需打电话

```
npm install -g @vue/cli
```

这将全局安装 vue 的命令行界面。命令完成后，我们最终可以使用

```
vue create "frontend folder"
```

其中很明显“前端文件夹”是你给你的前端回购文件夹的名字(所以它将创建该文件夹内的项目)。

CLI 将询问一系列选项，在第一个选项中，只需选择“手动选择功能”，在第二个选项中，确保选择“选择 Vue 版本”、“Babel”、“渐进式 Web 应用程序(PWA)支持”、“路由器”、“Vuex”、“Linter/Formatter”。

我们选择的选项将在其他章节中更详细地解释，只需知道 PWA 是使网站成为渐进式 Web 应用程序的支持(【https://en.wikipedia.org/wiki/Progressive_web_application】)，路由器将在我们的前端应用程序中为我们提供路由功能，vuex 将为我们提供一个集中存储。

在下一个选择中，它将要求 vue 版本。我用 vue 2 制作了这个教程，但是你可以随意选择不同的选项，变化不是很大，几乎所有的东西都可以在这个项目的两个版本中使用。

将以下所有选项保留为默认值(如果您喜欢一些更好的 linter 配置，请随意使用您喜欢的配置)。

然后，安装程序会自动在该文件夹中为您创建所有的项目结构。完成后，进入文件夹并启动

```
npm run serve 
```

这将创建一个小的开发服务器，现在你可以进入你的浏览器@[http://localhost:](http://localhost:8080/)8080/并查看页面和由 vue 命令行界面创建的小的基础应用程序。现在只要停止服务器，让我们也准备后端项目。

我假设您已经安装了 go，否则请转到[这里](https://golang.org/dl/)并安装它。

在 bakend 文件夹中，我们将使用以下命令创建一个 go 模块

```
go mod init github.com/idalmasso/go-vue-tutorial-backend
```

明确使用与您的设置相关的正确模块名称；

这将初始化您的模块，并使其可以使用。

注意:在以前的 GO 版本中，你必须编写所有的代码 GOPATH，现在有了这些模块，你就可以这样做了，并且可以把你的代码放在系统的任何地方

## 时势

让我们花一点时间来看看我们现在在这两个项目中得到了什么。

在后端文件夹中有一个单独的文件 go.mod，它包含了模块的定义，也包含了所有需要的包。

在前端文件夹中有更多的东西:

*   node_modules 文件夹，包含 vue cli 已安装的所有节点模块
*   一个公共文件夹，它包含一组静态资产，这些资产将在构建前端时被复制到分发文件夹中
*   一个 src 文件夹，其中包含将用于 vue 前端的所有 javascript 代码
*   其他一些配置文件，特别是包含应用程序及其依赖项的 package.json。在这个文件夹中，我们还可以设置一些实用程序脚本，在“脚本”部分，您可以看到我们之前调用的“服务”来创建开发服务器。特别是“build”命令将构建完整的 Vue 应用程序并创建一个分发文件夹，该文件夹必须由您喜欢的真实服务器提供服务。

## 对项目设置的一点改进

现在，现实世界的项目不能设置在开发服务器上，所以我们将在 golang 中构建的后端服务器也将负责为 vue 应用程序提供服务。

想要尽可能地分离后端和前端，我发现的最佳解决方案是在后端项目中创建一个 git 子模块，它指向前端项目的一个分支，该分支只包含分发项目。

说起来容易做起来难:我们所要做的就是进入前端文件夹，首先创建一个实际应用程序调用的构建

```
npm run build
```

这将创建“dist”分发文件夹，其中有前端应用程序工作所需的所有文件。下一步是将它作为一个不同的分支推送到 github，对于 semplicity，我使用了一个插件“push-dir ”,它实际上可以让你推送单个目录的内容。为此，请安装插件

```
npm i -D push-dir
```

并在 package.json 中添加一个脚本，如下所示:

```
"deploy": "push-dir --dir=dist --branch=production --cleanup"
```

然后调用命令

```
npm run deploy
```

会为我们做所有的工作。

现在，在后端文件夹中，我们可以像这样创建子模块

```
git submodule add -b production [https://your_frontend_repository](https://your_frontend_repository) dist
```

我们现在可以用下面的代码初始化子模块

```
git submodule update --init
```

然后用以下内容更新它

```
git submodule update --remote
```

为了易于使用，我们还可以在后端文件夹中添加一个 package.json 文件，将这两个命令作为脚本。

## 测试实际设置

最后一步是测试我们刚刚建立的设置:唯一需要的是创建一个小的 go 后端，将能够服务于 dist 文件夹。

如果一切正常，我们将准备开始编写真正的应用程序。

在后端文件夹中创建一个文件，并将其命名为“server.go”。然后添加以下代码:

这实际上是一个非常基本的代码，从包定义开始(main package，是 go 应用程序中起点的缺省值)，接着是导入，然后是主函数，起点。

在那里，程序创建了一个新的 gorilla 路由器/http 多路复用器。

有了这个对象，我们实际上可以告诉服务器哪些端点应该监听，以及它应该做出什么响应。在这种情况下，我们创建一个 http 文件服务器(称为“fs”)，它将服务于。/dist”文件夹(这实际上是前端应用程序，来自 git 子模块)。

下面一行表示在路由器上，来自“/”的请求将由文件服务器处理。http 上的最后一个调用。ListenAndServe 是一个阻塞程序，它在 localhost:3000 上创建服务器。

现在我们可以运行应用程序，例如调用

```
go run server.go
```

使用我们的浏览器在 http://localhost:3000 上导航。

我们现在可以看到，我们的前端应用程序就在那里，由 golang 服务器提供服务。

## 这有必要吗？

这种设置看起来有点复杂:我们可以只创建和更新应用程序前端，需要时只构建和复制文件夹。

在现实世界中，后端和前端开发人员可能是不同的实体，所以通过这种方式，他们都是独立的，这也是一种更干净的工作方式。

## 后续步骤

在接下来的章节中，我们将开始创建和更新我们的 vue 应用程序，使其真正发挥作用。

创建一些前端后，我们将尝试添加一些后端功能，这样前端数据将被获取和/或发送到 go 服务器的一些 api，我们将获得一些小级别的通信和持久性。

然后，我们将使用 mongodb 数据库来保存数据，添加身份验证的原始版本，最后，我们将努力使我们的应用程序成为 PWA，这样它也可以脱机工作，并且可以安装在用户的设备上。