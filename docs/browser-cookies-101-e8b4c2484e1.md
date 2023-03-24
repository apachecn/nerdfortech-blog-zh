# 网络 Cookies 101

> 原文：<https://medium.com/nerd-for-tech/browser-cookies-101-e8b4c2484e1?source=collection_archive---------16----------------------->

## 介绍和实施

创建 web 应用程序时，不可避免地会遇到… ***cookies*** 。

![](img/e86781467b1805840c164b0b8370359b.png)

# 介绍

web cookie(也称为 *http cookie* 或*浏览器 cookie* )只是一个文本文件。当用户导航到一个网站时，浏览器向服务器请求一段数据。作为响应，服务器发送数据——一个很小的文件，是一串文本。这一小块数据然后被存储在浏览器的内存中，允许它记住有状态的信息。例如，当用户导航到其他网站时，它能够从第一次登录和后续操作中识别用户。

将网络 cookiess 与网上购物联系起来，对理解网络 cookie 非常有帮助。Web cookies 有助于记住用户提供的任意信息，如输入表单字段的用户名、密码、地址和借记卡信息。Cookies 可以解决一些问题，例如将用户与他们在访问购物网站的不同页面时所采取的行动联系起来。如果没有 cookies，用户每次访问网站或新页面时都必须“介绍”自己。很明显，cookies 可以通过消除多余的动作来实现更无缝的交互，从而改善用户体验。

值得注意的是，有不同类型的网络 cookies。三种常见的 cookie 是“会话”、“持久”和“第三方”cookie。只有当用户当前登录到一个网站并且他们的浏览器主动打开时，会话 cookies 才存在。通常，一旦用户关闭浏览器，会话 cookies 就会被删除。相反，持久 cookies 会存在一段预定的时间，或者直到指定的某个日期。第三方 cookies 允许跨多个域或网站跟踪用户。当用户在当前页面时，这些 cookies 引用并来自其他网站。第三方 cookies 最适用的用例之一是定向广告。

# 实现 Cookies

## 创建 Cookies 服务器端

创建 cookie 时，服务器可以将一个`Set-Cookie`头和 HTTP 响应一起发送给客户机。这个头将通知浏览器创建和存储 cookie。当创建额外的 HTTP 请求时，浏览器将自动包含`Cookie`头。

通用 cookie 语法示例:

`Set-Cookie: key=value;`

Node.js，cookie 语法示例:

`response.setHeader('Set-Cookie', 'key=value');`

这里的值是一个字符串或一个字符串数组，其中包含与同一个 cookie 相关联的值:`response.setHeader('Set-Cookie', ['id=1', 'name=Diogenis']);`要删除一个 cookie，您可以使用以下方法:

*   将`Expires`标志设置为过去的日期
*   将`Max-Age`标志设置为`0`
*   利用带有上述 cookie 标志的 ExpressJS `clearCookie()`方法

## 设置 Cookies 客户端

也可以通过访问 Javascript 对象`Document.cookie`在客户端创建 Cookies。

为了创建一个客户端 cookie，类似于上面的 NodeJS 示例，您可以将一个值和任何适用的标志作为一个字符串传入:`document.cookie = “name=Diogenis”;`

要访问 cookie，您可以直接从对象中读取:

*   输入:`console.log(document.cookie);`
*   输出:`“name=Diogenis”`

为了删除 cookie，将值改写为空，并设置一个过去的到期数据:`document.cookie = “name=; expires=Mon, 20 Dec 1920 00:00:00 UTC;”;`

# Cookie 标志

可以将附加标志传递到 cookies 中，以增加数据的特异性。

`Expires`和`Max-Age`

为了构建持久 cookie，需要设置一个截止日期。这可以通过特定的日期和`Expires`标志来实现，或者通过`Max-Age`标签来实现特定的时间长度。

`Set-Cookie: id=2; Expires=Monday, 20 Jan 2021 00:00:00 GMT;`

`Set-Cookie: id=2; Max-Age=58162000;`

`Domain`和`Path`

`Domain`和`Path`标志决定了 cookie 的作用域和可访问域。`Domain`指定允许的主机，包括子域。如果没有此标志，默认行为只允许当前站点的主机，不包括子域。`Path`标志指定了一个 URL 路径，该路径必须包含在所请求的 URL 中，以便允许 cookie 与 HTTP 请求一起发送。设置标志时，使用正斜杠，将包括子目录。

`Path=/page`也会允许`Path=/page/1`和`Path=/page/1/custom`

`Set-Cookie: id=1; Path=/custom;`

`Set-Cookie: id=1; Domain: github.com;`

`Secure`和`HttpOnly`

在通过 HTTPS 协议发送到服务器的加密请求中，cookies 可以包含`Secure`标志。cookie 本身是不安全的，敏感信息不应该存储在 cookie 中。一些浏览器(Chrome、Firefox 等。)已经开始阻止不安全的站点设置带有`Secure`标志的 cookies。

Cookies 也可以设置`HttpOnly`标志。这可以防止在 Javascript 的文档对象中访问 cookiescookies 将只发送到服务器。对于只需要保存服务器端会话信息的 cookies，应该使用下面的标志。

`Set-Cookie: id=1; Secure; HttpOnly;`

`SameSite`

出于安全原因，为了防止 cookies 接收跨站点请求，可以设置`SameSite`标志。它可以遵循以下属性:`None`、`Lax`或`Strict`(不区分大小写)。允许在同一个站点或跨站点发送 cookies。`Lax`当客户端从外部站点导航到该域时，将允许发送 cookies。`Strict`只允许浏览器发送相同站点请求的 cookies。在所有其他情况下，跨站点子请求会保留 cookies。通常，`Lax`被用作浏览器默认值。

`Set-Cookie: id=1; SameSite=Strict;`