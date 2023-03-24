# 解码 HTTP 头(基本)

> 原文：<https://medium.com/nerd-for-tech/decoding-http-headers-basic-e552e5613125?source=collection_archive---------2----------------------->

> HTTP header = metadata =关于正在发送或接收的 HTTP 数据的数据。

我们知道 HTTP 是基于客户机-服务器模型的。客户端可以来自世界上的任何地方，服务器可以来自世界上的任何地方。客户端可以使用安装在任何操作系统上的任何浏览器，如 chrome/Mozilla，如 Windows、Linux 等。如果客户端只接受语言说 ***【泰米尔】*** 并且服务器只有英语语言内容。怎么办？

该元数据(即关于数据的数据)在 HTTP 报头中发送。

## HTTP 的基本结构:

![](img/49005f45caca68bdb2baafef53c67608.png)

主体包含实际数据——可以是 HTML 文本、JSON 等。头包含关于实际数据的元数据。

注意:空行分隔标题和正文。

HTTP 是基于消息的。

从客户端到服务器-请求和相应的报头是请求报头。

从服务器到客户端—响应和相应的标头是响应标头。

# 请求标题

**接受**

我接受什么类型的？它们被称为 **MIME** 类型(解释 MIME 超出了本文的范围)。

语法:类型/子类型

例如

```
Accept: text/html - here type is text and sub-type is html. 
Accept: image/* - here, type is an image, and sub-type can be anything like jpg, png, etc. 
// General default 
Accept: */ * - Accept any type and sub-type.
```

**接受编码**

我在浏览器中使用什么类型的编码？

编码=从一种形式变成另一种形式。这通常是为了压缩数据。

什么压缩算法客户端能看懂？

```
Accept-Encoding: gzip 
Accept-Encoding: compress 
Accept-Encoding: deflate 
Accept-Encoding: br 
Accept-Encoding: identity 
Accept-Encoding: *
```

**接受语言**

我能接受什么语言？它可以基于区域(locale)。

*语法:接受-语言:<语言>*

```
accept-language: en-US - accepts English of US 
Accept-Language: en-US,fr-CA - accepts English of US and French of Canada 
Accept-Language: * - accepts all languages.
```

这里理解质量值语法是很重要的。这就像赋予权重以区分优先次序。

```
accept-language: en-US,en**;q=0.9**
```

这里 *en-US* 优先级为 1.0， *en* 优先级为 0.9。

*注意:这种质量语法被用在许多标题中。*

**授权**

当客户端访问服务器上的私有资源时，这很有用。

Private =只有被允许的用户才能访问资源。

这涉及到对访问或操作资源的用户进行身份验证。它们主要用于 API。所以如果你构建一个 API，你需要有一个认证方法来访问你的 API。

有许多授权技术。

1)基本认证—基本<base64 encoded="" string=""></base64>

2)不记名令牌—不记名 <token>→服务器生成的令牌</token>

3)API 密钥

等等。

注意:

API 键可以作为查询参数发送

```
example.com/***?api_key=<key>***
```

或者单独的标题

```
X-API-Key: <key>
```

**连接**

我们知道 HTTP 是建立在 TCP 基础之上的。在 HTTP 1.0 中，一旦收到来自服务器的响应，TCP 就会关闭，但在 HTTP 1.1 中，您的连接将在随后的请求和响应周期中处于活动状态。

```
Connection: keep-alive --> default for http 1.1 
Connection: close
```

**内容长度**

它是正文中数据的大小。它以字节为单位。

```
content-length: 6553
```

**内容类型**

这在你发帖或提出请求时很有用。我向服务器发送什么类型的内容？

这里的内容类型可以是文本/纯文本、文本/Html、多部分/格式数据等。

多部分/格式数据在语法上必须有边界。

```
Content-Type: multipart/form-data; boundary=something
```

边界很有用，因为使用 post 方法提交的任何表单都包含 name: value 对。因此，每个名称-值对由边界分隔。边界是一组字符，它们之间没有空白。

请访问本页查看[示例](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Type)。

**主持人**

这是 HTTP 1.0 和 HTTP 1.1 的主要区别之一。

这在某种程度上启用了虚拟主机，因为服务器或机器只有一个 IP 地址。一台服务器可以托管许多资源，如 abc.com 和 xyz.com.Even。如果我们到达正确的 IP 地址，服务器不知道它将哪个资源分配给 xyz 或 abc。这就是**主机头**出现的原因。

```
Host: <host>:<port> 
e.g. abc.com:443
```

**用户代理**

这是最重要的标题。服务器识别客户端的网络浏览器和操作系统。web 浏览器用户代理的格式不同且复杂。但目的是一样的(识别浏览器类型和其上安装的 OS)。

# 响应头(从服务器角度)

**访问控制允许来源**

我允许什么起源？

Origin =组合方案、域和端口。

```
Access-Control-Allow-Origin: * --> allows all origin Access-Control-Allow-Origin: <origin> --> example.com
```

**允许**

允许哪些 http 请求方法？

```
Allow: GET, POST, HEAD
```

**内容处置**

来自服务器的一些数据不打算在网页上显示**。它们是用来下载的。**

```
**Content-Disposition: inline 
Content-Disposition: attachment 
Content-Disposition: attachment; filename="filename.jpg"**
```

**从服务器的角度来看，内容编码和内容语言与请求头相同。**

**类似地，**服务器**头相当于请求头中的**用户代理**。**

**我们将在下一篇文章中看到缓存和 cookies 的概念以及相应的标题。**

***原载于 2022 年 8 月 14 日 https://www.pansofarjun.com*[](https://www.pansofarjun.com/post/decoding-http-headers-basic)**。****