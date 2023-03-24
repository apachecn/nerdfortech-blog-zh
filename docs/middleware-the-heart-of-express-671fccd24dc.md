# 中间件 express 的心脏

> 原文：<https://medium.com/nerd-for-tech/middleware-the-heart-of-express-671fccd24dc?source=collection_archive---------42----------------------->

![](img/d6ff227548153f0b58ad970e379853dc.png)

尼古拉·菲奥拉万蒂在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

Express 是一个非常容易执行和学习的最小框架。但是是什么让 express 如此简单却如此强大呢？答案是中间件。

## 但是中间件到底是什么？

中间件是一种功能。这是一个在收到请求之后、响应发送之前执行的函数。在处理实际请求之前，它通常包含额外的业务逻辑或任务，如验证等。让我们看一个关于中间件的简单例子。

```
const express = require("express");const app = express();// using this function as middleware
const toUpper = (req, res, next) => { req.query.name = req.query.name.toUpperCase(); // pass control to next middleware
  next();};app.get("/", toUpper, (req, res) => { res.send(req.query.name);});app.listen(5000, () => { console.log("listening on port %s", 5000);});
```

## 下一个关键字

在使用多个中间件功能时，有一个特殊的关键字 *next* 会派上用场。每当一个中间件完成了它的工作，您可以使用 *next()* 语句调用下一个中间件。就像上面的例子一样，如果您从代码中删除了 *next()* ，控制权将不会传递给下一个中间件，客户端也不会收到任何响应。因此，请记住，要么使用 *res* 函数进行响应，要么使用 *next()。*

## 但是我为什么要用中间件呢？

问得好，您可能会想，所有这些都可以在单个函数中不使用中间件函数的情况下完成。嗯，这是正确的，但是使用中间件促进了代码模块化，并在 *err* 关键字的帮助下简化了错误处理。程序的流程也变得非常容易理解，而不是理解一大块代码。

```
// this function will throw error if parameter name is not definedconst toUpper = (req, res, next) => { req.query.name = req.query.name.toUpperCase(); next();};const addGreeting = (err, req, res, next) => { // err catches the error from previous middleware
  if (err) { res.send(err.message); } else { // send a greeting if no errors
    req.query.name = "Hello " + req.query.name; next(); }};// we are passing two middleware functions here
app.get("/", toUpper, addGreeting, (req, res) => { res.send(req.query.name);});
```

## 库函数作为中间件

有许多库提供了用作中间件的功能。通常，这些用于应用程序级路由，并在实际业务逻辑之前对请求执行操作。在下面的例子中，我们使用了来自 *cors* 和 *body-parser* 库的函数作为中间件

```
const app = express();// all the requests coming in the server will pass through
// the below middlewares
app.use( cors({origin: "http://localhost:3000"}));
app.use(bodyParser.json()); /*
   ****all the other middleares and routes here****
*/ const port = 5000;app.listen(port, () => { console.log("listening on port %s", port);});
```

这是所有的乡亲。留下掌声👏如果你喜欢这篇文章。下一集见。