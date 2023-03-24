# ExpressJS 101

> 原文：<https://medium.com/nerd-for-tech/expressjs-101-b67171f1dbd6?source=collection_archive---------6----------------------->

一段时间前我就想这么做了，因为在新来的 web 开发人员主宰或完成流畅的 HTML 工作后，关于下一步该做什么有很多困惑。

前端开发人员应该熟悉后端活动，不管是否专业，不仅仅是工具和如何发送/接收数据，而是 HTTP 通信的基础等等。

由于两个世界的熟练程度，越来越需要全栈工作。在我看来，仅仅学习道路的一边是不够的。

在 NodeJS 世界中，有许多工具、框架和本机内置模块用于处理 HTTP 通信。ExpressJS 成为这种转变最流行的工具。

这篇文章不是为了对 ExpressJS 进行全面的技术回顾。但是为了解释这个工具如何能够适应使用 HTTP 的简单性而不至于一试就死。

ExpressJS 是一个杀手级工具，因为它可以非常容易地处理任务。

1.  URL 参数和查询字符串解析
2.  自动回复标题。
3.  路线和更好的代码组织
4.  插件采用或中间件
5.  请求正文解析
6.  身份验证、验证、会话等。

## 第一种配置

查看下面的脚本，它包含一个简单配置的基本脚本和两个附加脚本。设置属性为我们的 HTML 脚本设置模板目录。而`view engine`设置将用于 HTML 站点的模板引擎

```
var express = require('express')
var app = express()
app.set('port', process.env.PORT || 3000) 
app.set('views', 'templates')
app.set('view engine', 'jade')
```

## 中间件模式

中间件模式是一系列连接在一起的处理单元，其中一个单元的输出是下一个单元的输入。

在 NodeJS 中，这通常意味着以下形式的一系列函数:

```
function(args, next) {
   [Place your logic here]
   next(output)
}
```

中间件的关键方面是在请求周期中扮演的角色及其连续性

`request -> middleware1 -> middlewareN -> route -> response`

Express 利用框架来实现中间件功能，如下所示:

```
var express = require('express')
var app = express()// [Here goes the middleware]
app.use(middleware1)
app.use(middleware2)
...
```

根据您想要开发的任何逻辑，执行的顺序是相关的。除了第三方中间件之外，Express 还允许您创建自己的中间件，框架如下:

```
var middleware = function(req, res, next) {
  [Place your logic here]
  next()
}app.use(middleware)
```

也可以匿名声明。这个是自动调用的。

```
app.use(function(req, res, next) {
  [Place your logic here]
  next()
})
```

在对 ExpressJS 服务器的单个客户机请求的生命周期中,`req`值总是相同的对象。它可以很容易地被引用，避免了需要将引用传递给中间件内部逻辑时的麻烦

以下是可用的最流行和最有用的中间件。您应该在 [npm](https://npmjs.com) 存储库中检查它们，并评估存在性、维护率、可用版本以及开发人员和用户文档

1.  `body-parser`作为请求有效载荷的机制
2.  `compression`具有 gzip 功能
3.  `connect-timeout`设置请求超时功能
4.  `cookie-parser`与 cookie 处理相关
5.  `cookie-session`通过饼干店进行会话
6.  `csurf`进行 CSRF 验证
7.  `express-session`通过内存或其他类型的存储进行会话
8.  `morgan`用于服务器日志
9.  `serve-static`为静态内容
10.  `cookies`和`keygrip`类似于`cookieParser`
11.  `express-validator`与验证相关
12.  `passport`作为认证库
13.  `helmet`用于安全标题
14.  `connect-cors` CORS

还有更多

## 模板引擎

将`view engine`变量设置为`jade`实例会在内部触发以下函数调用

这可以通过许多不同的方式来实现。

`app.set('view engine', 'jade')`

`app.engine('jade', require('jade').__express`

或者使用自定义回调

```
app.engine([format], function(path, options, callback) {
  [Template parsing logic and options]
})
```

## 快速启动逻辑

这一节很好理解，实例化 Express 框架的主文件，可以使用`http`内置模块委托服务器创建方法，vs 使用同一个`express` app 实例做同样的事情。

第一种方法是这样完成的:

```
var express = require('express')
var http = require('http')
var port = 3000var server = http.createServer(app)
server.listen(port, () => {
  console.log('Server up')
})
```

第二种方法几乎相同。

```
var express = require('express')
var app = express()
var port = 3000app.listen(port, () => {
  console.log('Server up')
})
```

## 利弊

在传统的 web 应用场景中，我们可能会遇到与关注点分离、性能、代码重复等相关的瓶颈。对于本研究案例，可以关注:

1.  性能可能很慢，而且是单任务的，这对于多任务或多线程场景来说不是一个有效的方法
2.  可怜而迟钝的 UX
3.  数据复制占用了 HTML 文件的带宽

使用胖客户端时，视角完全变了。如果不熟悉厚的薄的客户端阅读[这个](https://en.wikipedia.org/wiki/Rich_client)

胖客户端的优势在于:

1.  响应界面和 UX
2.  只有数据通过 JSON 对象传输
3.  您可以重用核心功能
4.  异步任务
5.  实时应用程序

这一新的功能浪潮彻底改变了全景。以及增强 web 开发客户端的新方法，剩下的就是历史了。

后端也以 HTTP 为主角改进了很多。容器化、微服务、API 分解、数据库多样化等等。

# 水疗、休息和 API

可重用性是这里的主要陈述:一次构建一个 API，在任何地方使用它。

无论使用哪种可用的客户机，您都可以使用相同的数据源。整个功能也是按照做什么和不做什么来划分的。由于 API 分解，您可以将服务用于特定的组件。

此外，微服务场景出现，虚拟化和开发运维问题势头强劲。

## 休息

REST 是一种开发网络应用程序的架构模式。REST 系统的目标是在机器之间连接和交换数据时保持简单。

鉴于 HTTP 的无状态特性和客户端-服务器架构，HTTP 是 REST 的理想协议。

与像 T21、SOAP、UDDI 这样的网络服务相比，这要简单得多，因为它们都依赖复杂的词汇来进行交流。每个新操作都是一个新的词汇表条目，增加了代码的复杂性。

REST 架构基于处理 CRUD 操作的 HTTP 动词。这就是为什么你听说过:`GET`、`PUT`、`POST`和`DELETE`，有时还有`PATCH`、`HEAD`或`OPTIONS`

另一个关键组件是**REST**REST**Resources**，这些是可以存储在计算机上的实体，比如文件、数据库条目或任何函数的处理输出。

REST 使用 HTTP 请求和响应来提供资源的表示。举个例子:从服务器下载一个文件，这个“文件”被认为是一种资源。

当修改资源时，通过访问、改变或删除它，也将表示在 **REST** 系统中改变的资源状态。

这是处理签名

```
// Request Handler signaturefunction(request, response, next) {
  [Something here]
}// Error Handler signature
function(error, request, response, next) {
  [Something here]
}
```

客户端的 HTTP 请求可以从路由处理程序中访问，其中第一个参数是处理程序的回调。

## 获取请求

下面是如何用 Express 处理 GET 请求。用于检索数据。

```
// Root GET route
app.get('/users', function(request, response) {
  response.send('hello')
})// GET Parameter handling
app.get('/users/:id', function(request, response) {
  ...
}) 
```

这些动态参数可以通过请求的 param 对象来访问，就像这样:`request.params.id`

```
// GET Parameter handling
app.get('/users/:id', function(request, response) {
  var id = request.params.id
})
```

## 发布请求

POST 用于创建资源。

```
app.post('/users', function (request, response) {  
  var username = request.body.username  
  var email = request.body.email  
  // ...  // Code to create a new user  
  response.send(user) 
});
```

## 上传请求

这个用来更新一个资源(或者创建一个可能不存在的资源)。

```
app.put('/users/:id', function(request, response) {
  var id = request.params.id 
  if(exists) {
   ... 
  } else {
   ...
  }
  response.send(user)
})
```

## 删除请求

这个用于删除资源。

```
app.delete('/users/:id', function (request, response) {
 var id = request.params.id;  
 // code to delete the user  response.send(user); 
 // or maybe the URL to create a new user? 
});
```

处理查询字符串也可以通过请求的查询对象来访问，比如:`request.query.name`

在`[http://localhost:3000/?name=David+Lares&age=22&occupation=Batman](http://localhost:3000/?name=David+Lares&age=22&occupation=Batman)`

你可以根据`key`变量名得到每个值，其中:`request.query.name`是`David Lares`，然后，`request.query.age`是`22`等等。

## 处理请求正文

如果您正在处理 **POST** 或 **PUT** 请求，您可能期望从表单或异步请求中接收数据。

ExpressJS 曾经直接从其核心功能中处理主体请求，但是这被分离到另一个名为`body-parser`的库中

安装:`npm install body-parser --save`

导入中间件:

```
var bodyParser = require('body-parser')app.use(bodyParser.json()) // for single-page apps and JSON clients
app.use(bodyParser.urlencoded({extended: false})) // for web-forms
```

使用`request.body`对象访问数据。

从 web 表单上传文件需要一个`multipart/form-data`属性。这可以用`Multer`、`express-busboy`、`connect-busboy`或`node-multiparty`第三方库来解析。

发送原始 JSON？`Bodyparser`也可以处理它，使用:

`app.use(bodyParser.json({type: 'application/*+json'}))`

自定义缓冲区也是如此

`app.use(bodyParser.raw({ type: 'application/vnd.custom-type' }))`

和 HTML

`app.use(bodyParser.text({ type: 'text/html' })`

诸如此类。查看 [*文档*](https://www.npmjs.com/package/body-parser) 了解更多信息

## 请求对象

请求对象有很多属性，可以补充框架的使用。请求对象具有这些属性(只是其中的一部分)

1.  `request.params`:参数中间件
2.  `request.param`:提取一个参数
3.  `request.query`:提取查询字符串参数
4.  `request.route`:返回路线字符串
5.  `request.cookies`:cookie，需要`cookieParser`
6.  `request.signedCookies`:已签名的 cookies，需要`cookieparser`
7.  `request.body`:有效负载，需要主体解析器
8.  `request.get(headerKey)`:表头键值
9.  `request.accepts(type)`:检查类型是否被接受
10.  `request.acceptsLanguage(language)`:检查语言
11.  `request.acceptsCharset(charset)`:检查字符集
12.  `request.is(type)`:检查类型
13.  `request.ip` : IP 地址
14.  `request.ips` : IP 地址(打开`trust-proxy`)
15.  `request.path` : URL 路径
16.  `request.host`:没有端口号的主机
17.  `request.fresh`:检查新鲜度
18.  `request.stale`:检查陈旧性
19.  `request.xhr`:对于 AJAX-y 请求为真
20.  `request.secure`:检查协议是否为`https`
21.  `request.subdomains`:子域数组
22.  `request.originalUrl`:原始网址

## 响应(HTTP 响应)

响应对象也可以通过 Express 中的路由处理程序来访问。在将 HTTP 响应发送给客户端之前，也可以使用 response 对象来修改 HTTP 响应。

以下是一些可以使用的响应属性:

1.  `response.redirect(status, url)`:重定向请求
2.  `response.send(status, data)`:发送响应
3.  `response.json(status, data)`:发送 JSON 并强制正确的报头
4.  `response.sendfile(path, options, callback)`:发送文件
5.  `response.render(templateName, locals, callback)`:渲染一个模板
6.  `response.locals`:将数据传递给模板

下面是一个用 ExpressJS 返回简单字符串消息的例子

```
app.get('/', function(request, response) {
  response.send('Hello world!') // resolved as text/plain
  response.send([6, 8, 9]) // resolved as application/json
  response.send({name: "David"}) // resolved as application/json
})
```

但是`response`类型可以像这样用`response.set`硬编码

```
app.get('/', function(request, response) {
  response.set('Content-Type', 'text/plain')
  response.send('Hello world!') // resolved as text/plain
})
```

## 会议

用 Express 处理用户会话也很容易，它需要`cookiesParser`中间件

这里有一个例子

```
app.use(express.cookiesParser())
app.use(express.session({secret: 'weak_secret'}))app.get('/', function(request, response) {
  var session = request.session
})
```

## Redis 会话

像这样处理与 [Redis](https://redis.io/) 的会话

您将需要安装依赖项:`npm install connect-redis express-session`

以下是的使用方法

```
var session = require('express-session')
var RedisStore = require('connect-redis')(session)app.use(session({
  store: new RedisStore(options)
  secret: 'keyboard cat'
}))
```

## 负载平衡

根据我的经验，出于负载平衡的需要，我使用了带有`Clusters`、`Nginx`、`HAProxy`和`Varnish`的 ExpressJS，但是我很确定它可以适应其他类似的工具。

总结一下，ExpressJS 提供了很多东西，但也不是唯一可以使用的东西。令人惊叹的框架，包括 Sails、Loopback、Meteor、哈比神或 Restify，可以成为 ExpressJS 的替代方案

你可以随时参考这个[站点](http://nodeframework.com/)来找出 NodeJs 生态系统中还有哪些工具可以使用。