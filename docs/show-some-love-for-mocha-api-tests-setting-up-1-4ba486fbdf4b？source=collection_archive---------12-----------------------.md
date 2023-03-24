# 对 Mocha api 测试表现出一些热爱吧！设置#1

> 原文：<https://medium.com/nerd-for-tech/show-some-love-for-mocha-api-tests-setting-up-1-4ba486fbdf4b?source=collection_archive---------12----------------------->

# 介绍

在这个故事中，我将使用`Express`和`MongoDB`构建简单的 restful api。我将使用`Docker-Compose` 建立测试环境，并为我的所有端点编写 api 测试，没有任何模仿。

# 关于我

我成为`Nodejs`爱好者已经快三年了，从初级开发人员，到初级全栈工程师，现在是中级开发人员。我的职业生涯主要是用`Javascript` *&* `Typescript`框架和`AngularJS``ExpressJS``React`之类的库进行全栈开发。对于软件工程的`Devops`方面，我也有一些经验，比如使用`Terraform`、`Docker`、`Teamcity`、`Nginx`等工具。我目前的职位是 AWS 云工程师，这不仅是我的工作，也是我的爱好——掌握 AWS 云。

# 先决条件

1.  NodeJS v12.6.0 或更新版本；
2.  NPM 版本 6.14.0 或更新版本；
3.  Docker v19.03.13 或更新版本；
4.  Docker-compose v1.27.4 或更新版本；
5.  Visual Studio 代码(或您喜欢的任何其他代码编辑器)；
6.  JavaScript 和软件开发入门/初级知识。

# 让我们开始设置！

用`npm init`命令初始化项目。

![](img/db7be34982c7284aa79a8e74b40abfab.png)

npm 初始化命令

然后，安装`express`、`mongoose`、`mocha`和`jsonschema`进行车身验证。

![](img/56529029bd01459ec34fc4b381bdd5b3.png)

安装依赖项

脚手架工程结构。

![](img/6d5c260e6286a8f87bba5f8749d4f32a.png)

项目结构

# 初始化连接到 Mongodb 的 Express 应用程序

这个很简单。最佳实践是将实际的 express 应用程序与 web 服务器分开，因此创建了两个单独的文件:`app.js`和`server.js`(应用程序的入口点)。`Mongo-connect.js`模块说明了一切——它通过使用 mongoose 库连接到`Mongo`数据库。文件将保存所有的配置变量，如端口、mongo 连接字符串等，通常这些东西应该来自 env 变量，但为了简单起见，我将硬编码它们。和 routes 目录—这是我们实际的 api 将要存在的地方。

![](img/5ee0253c8e92263df2a8edfad1e02a1f.png)

快速应用程序定义— app.js

![](img/76416123df591aef440e8dae26cbd834.png)

配置文件— config.js

![](img/d83ec67bdfa6549bef7a9e719cc9c748.png)

实际 http 服务器— server.js

![](img/acb6e2708f1a3bd5caffda4bc4f703d6.png)

使用 mongose-mongo-connect . js 连接到 mongodb

# 是不是少了什么？

确实是的。`Mongodb`docker 上运行的服务器。下面是一个 docker 为`Mongodb`编写文件的好例子:

![](img/93dff5a9ec0681a2f5da59e934532ef6.png)

docker-为开发环境编写

但是让我复制这个文件并创建另一个名为`docker-compose-tests.yaml`的文件:

![](img/a1e6a77e13a8456d9ce97ff242df8408.png)

docker-为测试环境编写

你注意到这一点点不同了吗？没错——港口。开发环境将使用默认的 27017 端口，但是测试环境— 17017。这就是我们如何简单地确保我们将不得不为两种环境分离服务器。另外，这里有一个`config.js`文件的小更新:

![](img/fc0e5543e9bff613fb2743ee7c097233.png)

config.js 文件中测试 Mongodb 连接字符串的新条目

让我们启动这两个容器，命令几乎相同(不要忘记让 docker 服务在您的机器上运行):

`docker-compose up -d` —这将启动开发合成文件。

`docker-compose -f docker-compose-tests.yaml up-d` —这将启动测试合成文件。

要关闭服务，使用相同的命令，但是使用关键字`down.`代替关键字`up`

您可以使用`docker container ls`验证容器正在运行:

![](img/9d7df24fba6c7793c277edf314692b8a.png)

码头集装箱 ls 结果

# 创建模型并注册路线

我将为`Items`资源创建一个简单的`CRUD` api。项目模型将非常简单:

![](img/28bfd2b6084555dda9475803806ed3d6.png)

项目模型

注意，我已经创建了`models/Item.js`目录和模块。

接下来—注册路由器:

![](img/7ec89225713797a40d8b295a3afebb45.png)

空项目路由器-routes/Items . js

![](img/6cf5209bcb68a56b11ad5b4853399cf1.png)

Api 路由定义和导出器文件— routes/index.js

![](img/1a63da1445be9e472354c98c0311ee66.png)

在 app.js 文件中的 express.json 中间件之后注册它

# 是时候使用 restful api 了！

这里有五个基本的 crud api 端点:GET /，GET /:id，POST /，PUT /:id，DELETE /:id。

![](img/3a3f1e2548d4b0712d78ed27f7c8e2fc.png)

模拟路线

现在我要做两件事——设置`jsonschema`验证器并安装`express-async-wrap`来处理所有异步错误。

![](img/aaff1a3652f7399291e3e6c48414799d.png)

jsonschema 验证器实现

现在是最简单的部分——实现所有的 http api 端点。

GET / —非常简单的 api 端点，它支持两个查询参数——size 和 page，并且一次最多只允许检索 100 个项目，默认为 10 个。另外，请注意`X-Total-Count`头，它返回数据库中有多少项。

![](img/4ba77f99e9aca28c9caab0dcb6a1157b.png)

获取/API/项目

GET/:id——甚至更小的 api 端点，但是不要忘记验证传入的 id！

![](img/459c6bed8f39f54336fb44306e6ff1ac.png)

GET /api/items/:id

POST / —在数据库中创建新条目，但这有一个关键时刻— *总是* *验证传入的请求体。*此外，注意`updateItem`函数——它将显式地只使用来自 DTO(数据传输对象)的所需字段。schema 和 updateItem 函数都可以而且应该被 PUT /:id 端点重用。

![](img/cfb302291e03ec812c96c92f7f141d13.png)

帖子/API/项目

PUT /:id 必须验证 req.params.id 和传入正文。

![](img/66dbf393a3c1c31ebf1fa02ecd0884db.png)

PUT /api/items/:id

DELETE/:id——没有扭曲，并且您答对了——这必须验证 req.params.id:

![](img/b32e8743522a264288121ebc08785e94.png)

删除/api/items/:id

另外，在引入`NotFound`错误后，不要忘记更新`app.js`中的错误处理程序:

![](img/2e5fbbeb4bec8c92141247aaa3479624.png)

app.js 中的错误处理程序

# 就是这样。

我们有自己的 api，你可以通过 postman，curl 或者任何你喜欢的工具来测试它。下一个故事将只包括 api 集成测试，没有模拟和我们已经建立的专用 Mongodb。点击[这里](https://liudas-demikis.medium.com/show-some-love-for-mocha-api-tests-testing-2-342fcd250aeb)可以找到第二部分。