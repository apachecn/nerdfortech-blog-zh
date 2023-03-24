# 以太坊 JavaScript API- web3.js

> 原文：<https://medium.com/nerd-for-tech/ethereum-javascript-api-web3-js-c02e4434cd52?source=collection_archive---------4----------------------->

![](img/6b3112af34979c8984df0790609ed920.png)

## 介绍

在这篇文章中，我们将讨论如何开始使用 Web3.js 库。以太坊 [JavaScript](https://www.technologiesinindustry4.com/javascript-yui-test-framework/) API web3.js 是库的集合。它允许我们使用 HTTP、IPC 或 WebSocket 与本地或远程以太坊节点建立联系。

Web3.js 允许我们完成开发与以太坊区块链交互的客户端的职责。它是一个库的集合，使我们能够做像这样的活动；

*   将乙醚从一个帐户转到另一个帐户
*   从智能合约交付和写入数据
*   签订明智的合同，并且
*   更加

## 描述

web3.js 库是一个开源的 JavaScript 库。它是由以太坊基金会制作的。它包括通过 JavaScript 对象符号—远程过程调用(JSON-RPC)协议与以太坊节点连接的函数。这是一个 JavaScript 库，使开发者能够与以太坊区块链进行交互。Web3.js 目前的版本是 1.2.9。它由四个模块组成。

## 什么是模块？

JavaScript 中的模块是在大型程序中具有精确功能的代码。模块将是独立的。因此，如果我们从一个库、程序或应用程序中删除一个模块，整个库、程序或应用程序不会停止工作。

## 模块组成了 web3.js

Web3.js 有一个名为 Web3 的关键类。该库的大部分功能都源于这个类。以下是组成 web3js 的五个附加模块:

web3-eth

web3-eth 模块具有允许 web3.js 用户与以太坊区块链交互的功能。这些功能是智能交互的；

*   智能合同
*   外部拥有的账户
*   节点
*   开采的区块

下面给出三个说明性的例子:

*   getBalance 使我们能够成为给定块中地址的 ETH balance。
*   eth.signTransaction 允许我们签署交易。
*   eth.sendSignedTransaction 允许我们将已签名的交易发送到区块链以太坊。

网络 3-嘘

web3-shh 模块允许我们使用耳语协议。耳语是一种信息协议。它旨在简单地广播消息，并用于低级异步通信。下面显示了两个描述性插图:

*   嘘。在网络上发布一条悄悄话。
*   shh.subscribe 订阅传入的耳语消息。

**web3-bzz**

web3-bzz 模块使我们能够与 Swarm 交互。Swarm 是一个去中心化的存储平台。它也是一种内容分发服务。它可以作为一个家来存储文件，例如我们的分散应用程序的图像或视频。下面给出了两个描述性的例子:

*   bzz。上传允许我们上传文件和文件夹到 Swarm
*   bzz。下载使我们能够从 Swarm 下载文件和文件夹

**网三网**

web3-net 模块允许我们与以太坊节点的网络属性相关联。通过使用 web3-net 可以让我们找到关于节点的信息。下面给出两个说明性的例子:

*   . net.getID 继续网络 ID
*   . net.getPeerCount 处理与该节点关联的对等方的数量。

**web3-utils**

web3-utils 模块为我们提供了实用功能。我们可以在以太坊 Dapp 中与其他 web3.js 模块一起使用。效用函数是一个可返回的函数，用于简化代码编写，这在 JavaScript 和其他编程语言中很常见。Web3-utils 包含实用函数。它改变数字，确认某个值是否发生在特定条件下，并搜索数据集。三幅描述性插图如下所示:

*   把乙醚换成魏。
*   utils.hexToNumberString 将十六进制值更改为字符串。
*   utils。如果提供的字符串是有效的以太坊地址，则为地址格式。

## 装置

我们可以在终端中安装带有 NPM 的 Web3.js 库，如下所示:

```
$ npm install web3
```

## Infura RPC URL

我们需要访问以太坊节点，以便通过主网上的 JSON RPC 连接到以太坊节点。我们有一些方法可以做到这一点。首先，我们可以用 Geth 或奇偶校验运行我们自己的以太坊节点。尽管如此，这需要我们从区块链下载大量数据并同步。如果我们以前尝试过这样做，这将是一个巨大的麻烦。

我们可以使用 Infura 来访问以太坊节点，而不需要手动运行，主要是为了适用性。Infura 是一个免费提供远程以太坊节点的设施。我们需要做的是注册，获得一个 API 密钥和我们想要连接的网络的 RPC URL。

注册后，我们的 Infura RPC URL 应该是这样的:

https://mainnet.infura.io/YOUR_INFURA_API_KEY

## 用 Web3.js 从智能合约中读取数据

我们可以用 web3.eth.Contract()函数实现以太坊智能合约的 JavaScript 表示。该函数需要两个参数:

*   一个是智能合同 ABI
*   一个用于智能合同地址。

智能契约 ABI 是一个 JSON 数组。它定义了一个精确的智能合约是如何工作的。下面是 ABI 的一个实例:

```
const abi = [{"constant":true,"inputs":[],"name":"mintingFini
```

如何使用 Web3.js 部署智能合约

我们可以通过许多方式将智能合约部署到以太坊区块链。在 Web3.js 本身中，有多种方法可以部署它们。我们将使用 app.js 文件，并对其进行如下设置:

```
var Tx = require('ethereumjs-tx')const Web3 = require('web3')const web3 = new Web3('https://ropsten.infura.io/YOUR_INFURA_API_KEY')const account1 = '' // Your account address 1const privateKey1 = Buffer.from('YOUR_PRIVATE_KEY_1', 'hex')This example has consisted on three basic steps:
```

*   构建事务对象
*   签署交易
*   发送交易

到目前为止，我们一直在构建一个交易并将其发送到网络。公正的差异是交易参数。

接下来，我们将像这样构建事务对象:

```
const txObject = { nonce:    web3.utils.toHex(txCount), gasLimit: web3.utils.toHex(1000000), // Raise the gas limit to a much higher amount gasPrice: web3.utils.toHex(web3.utils.toWei('10', 'gwei')), data: data}
```

数据参数是十六进制智能协定的编译后的字节码表示。为了获得这个价值，我们首先需要一个智能契约。然后我们需要编译它。我们欢迎使用任何我们喜欢的智能合约。不过，我们将使用一个 ERC-20 令牌智能契约以及附带的 Web3.js。我们可以在编译完契约后将数据值赋给一个变量。

现在，我们还可以通过获取事务计数来分配 nonce 值:

```
web3.eth.getTransactionCount(account1, (err, txCount) => { const data = '' // Your data value goes here... const txObject = { nonce:    web3.utils.toHex(txCount), gasLimit: web3.utils.toHex(1000000), gasPrice: web3.utils.toHex(web3.utils.toWei('10', 'gwei')), data: data }})
```

更多详情请访问:[https://www . technologiesinindustry 4 . com/ether eum-JavaScript-API-web 3-js/](https://www.technologiesinindustry4.com/ethereum-javascript-api-web3-js/)