# 模块化代码的快速路由器

> 原文：<https://medium.com/nerd-for-tech/express-router-for-modular-code-f155d4406897?source=collection_archive---------4----------------------->

![](img/d9efcd6a63a0905c169661498a793257.png)

[安德鲁·尼尔](https://unsplash.com/@andrewtneel?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

ExpressJS 附带了一个 *Router()* 功能，用于帮助组织您的 express 应用程序的路线。这个函数实际上是作为一个中间件使用的，用于在多个文件之间分配路径。让我们详细检查一下这个！

## 什么是快速路由器？🤔

它主要用于为您的 Express 应用程序编写模块化路由或端点。理论上没有更多的问题，让我们来看一个例子。

假设我们要开发一个电子商务网站，有三个主要路径，分别命名为*购物车、产品*、*登录、*和*注册。*这些路由及其子路由可以写在一个文件中，但是随着复杂度的增加，维护这些代码将成为一场噩梦。现在，我们来看看快速路由器是如何解决这个问题的。

首先，让我们为*购物车*子路线创建一个单独的文件

**cart.js**

```
const router = require("express").Router();// this route is equivalent to "/cart" get method
router.get("/", (req, res) => { // logic for getting cart items});// this route is equivalent to "/cart" post method
router.post("/", (req, res) => { // logic for adding item to cart});// this route is equivalent to "/cart/:id" delete method
router.delete("/:id", (req, res) => { // logic for deleting item from cart});module.exports = router;
```

在上面的代码中，为 *cart* 及其子路线定义了一个 *Router* 。注意"/cart "字符串没有写在任何路线之前。这是因为上述路线将从 *app.js* 文件中触发。

同样，我们为*产品、登录、*和*注册编写单独的路线。*

**products.js**

```
const router = require("express").Router();router.get("/:type", (req, res) => { // logic for getting products});module.exports = router;
```

**login.js**

```
const router = require("express").Router();router.post("/", (req, res) => { // logic for logging a user in});module.exports = router;
```

**register.js**

```
const router = require("express").Router();router.post("/", (req, res) => { // logic for registering a new user});module.exports = router;
```

至此，已经为我们的应用程序中的四个主要路由创建了一个*路由器*。但是这些是什么时候执行的呢？怎么称呼他们？答案是，我们将它们作为中间件传递给主路由。

## 通过*路由器*作为中间件

将*路由器*导入到你的 *app.js* 中，并将这些作为中间件传递给主路由。

**app.js**

```
const express = require("express");// importing our custom routers
const cartRouter = require("./routes/cart");const productRouter = require("./routes/products");const loginRouter = require("./routes/login");const registerRouter = require("./routes/register");const app = express();// using the custom routers as middlware
app.use("/cart", cartRouter);app.use("/products", productRouter);app.use("/login", loginRouter);app.use("/register", registerRouter);const port = process.env.PORT || 5000;app.listen(port, () => { console.log("listening on port %s", port);});
```

我们结束了。上面的 app 相当于使用了直达路线。

例如，如果向服务器发出“/cart/<id>”*delete*请求，它将转到 *cartRouter* ，然后执行 *delete* 方法中的代码。</id>

恭喜你，你刚刚使用路由器🥳使你的快速代码模块化