# 如何在 macOS 上下载安装 node.js 和 npm

> 原文：<https://medium.com/nerd-for-tech/how-to-download-and-install-node-js-and-npm-on-macos-e58ae1506d71?source=collection_archive---------5----------------------->

![](img/ec438cba61482f3ca13f703bcde4e166.png)

[安德鲁·尼尔](https://unsplash.com/@andrewtneel?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

Node.js 是一个免费的开源跨平台运行时环境，用于开发服务器端和网络应用程序。Node.js 程序是用 JavaScript 编写的，在 OS X、微软 Windows 和 Linux 上的 Node.js 运行时上运行。Node.js 还拥有丰富的 JavaScript 模块库，这使得开发 Node.js web 应用程序变得更加容易。节点包管理缩写为 NPM。Node.js 是一个免费的开源跨平台运行时环境，用于开发服务器端和网络应用程序。Node.js 程序是用 JavaScript 编写的，在 OS X、微软 Windows 和 Linux 上的 Node.js 运行时上运行。Node.js 还拥有丰富的 JavaScript 模块库，这使得开发 Node.js web 应用程序变得更加容易。节点包管理缩写为 NPM。

**下载。来自 node.js 官网的 pkg 文件**

访问以下网站，点击 macOS 标志开始下载。

[](https://nodejs.org/en/download/) [## 下载| Node.js

### Node.js 是基于 Chrome 的 V8 JavaScript 引擎构建的 JavaScript 运行时。

nodejs.org](https://nodejs.org/en/download/) 

**开始安装 node.js**

下载后。你需要打开网站上的 pkg 文件。这将启动一个安装程序，引导您完成安装过程。这很简单。您可以按照以下步骤完成这些步骤:

1.  介绍

*   选择继续

2.许可证

*   选择*继续*
*   选择*同意*

3.安装类型

*   选择*安装*
*   您需要输入一次 mac 密码才能开始安装。
*   选择*安装软件*

4.摘要

*   选择*关闭*

**安装和更新*NPM***

Node.js 总是附带特定版本的 npm。npm 不会由 Node.js 本身自动更新。因此，最佳实践是在安装 node.js 后检查 npm 版本。。因此，通常总会有比预装特定版本的 Node 版本更新的 npm 版本。

您可以使用以下命令更新 *npm*

*$ sudo npm 安装 npm —全局*

**验证下载**

现在检查 node.js 和 npm 是否正确安装了最新版本。您可以使用以下命令检查它们。

用于检查 node.js 版本。

*$ node -v*

用于检查 npm 版本。

*$ npm -v*

现在您已经安装了 *Node.js* 和 *npm* ，并准备好在 Mac 上使用。是时候开始探索了！