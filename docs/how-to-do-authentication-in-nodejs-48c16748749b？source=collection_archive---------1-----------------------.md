# NodeJS 怎么做认证？

> 原文：<https://medium.com/nerd-for-tech/how-to-do-authentication-in-nodejs-48c16748749b?source=collection_archive---------1----------------------->

身份验证是大多数重要的 web 应用程序需要解决的问题，对于应该如何做有很多(相互冲突的)观点。让我们看看解决这个问题的一些方法，以及进一步细节的链接。

当我刚开始编程时，我在一家小型初创公司工作。
我们的 CTO 领导软件架构，并将许多代码编写职责委托给我们这些级别较低的员工。
在构建我们的产品时，我们需要验证我们的用户(就像现在大多数应用程序所做的那样)。

认证是证明用户的身份。授权就是弄清楚被授权的用户被允许做什么。

那么，我如何认证一个用户呢？

![](img/14271b5eb0836b3bbb96a56c59ac33a0.png)

由[马库斯·斯皮斯克](https://unsplash.com/@markusspiske?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

# 选项 1:第三方服务

当我们开始时，我们的 CTO 做出了一个明智的决定，为此使用第三方服务。[谷歌的 Firebase](https://firebase.google.com/products/auth) 有一个简洁的认证子系统，负责在云上安全地存储凭证，并且开箱即用，有许多附加功能，比如可以使用谷歌、推特或脸书账户进行认证。

还有各种其他的选择(比如 [Okta](https://www.okta.com/) )，但是我想把重点放在我自己的个人经历上。

委托的好处是实现起来相对较快，并且安全风险由一个不是你自己的实体来承担。
不利方面可能是成本和供应商锁定。

# 选项 2:使用图书馆

大多数开发人员都不是安全专家，而且密码学很难搞清楚。普遍的看法是，最好使用一个成熟的、经过良好测试的实现(比如 [OpenSSL](https://www.openssl.org/) )，而不是实现自己的实现。
那么，为什么认证应该是不同的呢？

现在一个流行的选择是[passport . js](http://www.passportjs.org/)(NPM 上每周 946k 的下载量)，这是一个 [Express.js](https://expressjs.com/) 专门为认证而构建的中间件。它也支持许多认证策略(google、twitter、facebook 等等)。与第三方云服务不同，这完全是内部部署，没有许可成本(Passport 是开源的，有 MIT 许可)。
但是如果你没有使用 Express.js 呢？例如，如果你使用[帆](https://sailsjs.com/)会怎么样？[哈比神](https://hapi.dev/)？

当谷歌搜索认证时，你可能会发现另一种流行的技术叫做 [JWT](https://jwt.io/) 。它不是一种认证技术，而是用于授权。一个流行的用例是使用 Passport.js 之类的东西来验证用户，然后向他们发送一个 JWT。

# 选项 3:自己卷

在我的第一份工作中工作了一年多一点后，它的技术领导发生了变化，一个新产品的开发开始了。新任首席技术官决定由我们自己来实施身份认证。

我很欣赏 CTO 不仅根据工程的合理性，还根据其他因素(如上市时间和成本)做出决策。
实现自己的安全性有很多缺点，而且通常是不明智的。

简单认证

这或多或少是我们 CTO 设计的:
1。用户向服务器发送了他们的用户名和密码，假设通过 HTTPS
2 建立了加密连接。服务器从数据库中获取用户的记录(基于用户名)
3。存储在用户记录中的唯一的 salt 与提供的密码连接，结果被散列
4。将这个散列与记录中存储的散列进行比较(确保使用[timingsafequal()](https://nodejs.org/api/crypto.html#crypto_crypto_timingsafeequal_a_b)而不是相等运算符！).如果它们相同，则意味着用户提供了正确的密码，身份验证成功

听起来很棒，不是吗？超级简单，非常优雅。
*凭证不是以纯文本形式发送的(因为使用了 HTTPS，所以通道是加密的)
*密码不是以纯文本形式存储在数据库中。每个人都说盐和散列你的密码，我们在这里做。

然而，它并不是没有缺陷。对于技术演示或概念验证来说，这可能很好，但这可能会受到多种攻击。例如:
*这容易受到重放攻击。
*最薄弱的环节是通信信道(希望加密得很好)

我们怎样才能做得更好？让我们从[挑战-响应认证](https://en.wikipedia.org/wiki/Challenge%E2%80%93response_authentication)中得到启发。有大量的行业标准协议(如 [Kerberos](https://en.wikipedia.org/wiki/Kerberos_(protocol)) 、[急停](https://en.wikipedia.org/wiki/Salted_Challenge_Response_Authentication_Mechanism)等)，但让我们假设我们遭受的[不是这里发明的](https://en.wikipedia.org/wiki/Not_invented_here)。

我们在这里试图做的是双重的。不要在通信信道中发送密码
2。使身份验证唯一，这样它就不能被重放

这是我想到的:

1.  用户向服务器发送他的用户名
2.  服务器从数据库中获取用户的记录
3.  服务器用用户的 salt 和迭代计数，加上一些随机字节(让我们称之为“nonce”)来响应
4.  用户将 salt 应用到他的密码上并对其进行散列，将 nonce 附加到结果上并再次进行散列
5.  用户向服务器发送这个散列
6.  服务器将接收到的散列与用户记录中使用 nonce 散列的散列进行比较(确保使用[timingsafequal()](https://nodejs.org/api/crypto.html#crypto_crypto_timingsafeequal_a_b)而不是等式运算符！).如果它们相同，则认证成功。

示例实现:
服务器端:[https://github . com/soryy 708/nodejs-Cr-auth/blob/master/src/API/routes/auth . js # L31](https://github.com/soryy708/nodejs-cr-auth/blob/master/src/api/routes/auth.js#L31)
客户端:[https://github . com/soryy 708/nodejs-Cr-auth/blob/master/src/web/index . js # L16](https://github.com/soryy708/nodejs-cr-auth/blob/master/src/web/index.js#L16)

那么我们这里有什么？首先让我们看一下好处:
*密码根本不通过通信通道发送
*身份验证是唯一的，因为它是用 nonce 散列的；因此无法重播
*服务器不以明文形式存储密码

但是缺点是什么呢？
*服务器和客户端之间的对话有更多的步骤，增加了实现难度
* nonce 需要在两个 API 调用之间保持，这违反了无状态性
*我们正在使用 hash 中的 hash，这可能会无意中增加冲突概率

我不是安全专家，这个想法是我在一张餐巾纸上想出来的(打个比方)。我很想听听你对它的评论，最好是把它彻底遗忘！