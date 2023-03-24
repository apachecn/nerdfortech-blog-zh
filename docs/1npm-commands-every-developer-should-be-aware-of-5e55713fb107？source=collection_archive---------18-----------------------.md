# 每个开发人员都应该知道的 12 条 NPM 命令

> 原文：<https://medium.com/nerd-for-tech/1npm-commands-every-developer-should-be-aware-of-5e55713fb107?source=collection_archive---------18----------------------->

npm 是 node js 的包管理解决方案，如果你认为 npm 代表节点包管理，那么你就错了，它不是首字母缩写。npm 将模块放在适当位置，以便节点可以找到它们，并智能地管理依赖冲突。最常见的是，它用于发布、发现、安装和开发节点程序。

![](img/40cc23e6d13e3012a89eb3b6806c6ec1.png)

由[保罗·埃施-洛朗](https://unsplash.com/@pinjasaur?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

一些 npm 命令，我相信每个开发人员都必须知道

1.  npm i
    这里 I 代表安装。它安装 package.json 中提到的所有包
    。
2.  *NPM install-production*
    它安装 package.json 中提到的所有包，除了
    dev 依赖项。
3.  *npm i request*
    它安装一个名为“request”的包，你可以使用你的
    包名。
4.  NPM install—save-dev request
    它将特定的包作为开发依赖项进行安装，在我的例子
    中，它的名字是“request”。
5.  *npm list*
    列出当前
    目录下所有依赖项的版本和名称。
6.  *npm 更新* 它更新当前目录中的所有生产包。
7.  npm install -g nodemon
    它在你的机器上全局安装一个包，带有-g 标志。在这种情况下，nodemon 将被全局安装。
8.  *npm 移除请求*
    它卸载/移除
    当前目录中以前安装的节点模块。
9.  *npm -v*
    它显示您系统上安装的 npm 版本。
10.  npm 博士
    它检查我们的环境，以便我们的 npm 安装有管理我们的 JavaScript 包所需的
    。
11.  *npm 过时*
    该命令将检查注册表，查看是否有任何(或特定)已安装的软件包当前已经过时。
12.  *npm 审计& npm 审计修复*
    扫描您的项目漏洞，并自动安装任何与易受攻击的依赖项兼容的更新

感谢阅读更多关于 npm 命令的信息，点击[查看](https://docs.npmjs.com)