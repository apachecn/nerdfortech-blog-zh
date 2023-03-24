# 将您的智能合约部署到公共测试网

> 原文：<https://medium.com/nerd-for-tech/deploy-your-smart-contracts-to-a-public-testnet-7f9aef3f6039?source=collection_archive---------4----------------------->

## 将智能合约部署到公共测试网的快速指南

![](img/b89772dcffc13162bf358351dfc4b816.png)

安德烈·拉林在 Unsplash[拍摄的照片](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

欢迎阅读以太坊编程基础的另一篇文章。在以前的文章中，我们已经看到了开发一个分散式应用程序的过程和基本工具，在今天的文章中，我们将看到如何在一个公共测试网中部署一个 dapp，我还会给你一些资源，你可以用它们来加强你作为一个以太坊开发者的技能。

# 什么是 Infura

Infura 的主要目标是提供对以太坊和 IPFS 网络的即时访问，而无需设置自己的以太坊或 IPFS 节点。

> Infura 由尖端的微服务驱动架构提供支持，该架构可动态扩展以支持我们的 API。开发人员可以通过 HTTPS 和 WebSocket 连接到以太坊和 IPFS，那里的请求响应时间比其他服务和自托管解决方案快 20 倍。我们的 API 套件始终拥有最新的网络更新，并在所有网络变化期间保持服务可用性。—来自 Infura [产品描述](https://infura.io/product)。

# 部署到 kovan testnet

在这一节中，我将向您展示将智能合约部署到 kovan testnet 所需的步骤。
你可以在[以太坊 stackexchange](https://ethereum.stackexchange.com/questions/27048/comparison-of-the-different-testnets) 这里找到不同以太坊测试网的详细对比。

1.  **在 Infura 中创建一个项目:**为了使用 infura API，您需要创建一个帐户和一个新项目，因为您需要项目 Id 以便能够向 API 发送请求(更多详细信息请参见[此链接](https://blog.infura.io/getting-started-with-infura-28e41844cc89/))。
2.  **在 MetaMask 中获取您帐户的助记符:**如果您尚未保存您的帐户助记符，您可以转到 MetaMask 设置，选择安全和隐私，然后单击“显示种子短语”并输入您的密码以显示助记符。
3.  **创建一个文件来保存项目秘密，如助记符和 infura 项目 id 私有:**在 truffle 项目的根目录下创建一个名为`.secrets.json`的 json 文件，并添加以下内容:

```
{
   "mnemonic": <your mnemonic>,
   "projectId": <your project Id>
}
```

**4。** **安装** `**HDWalletProvider**` **:** 为了部署智能合约，我们必须将带有合约字节码的事务发送到网络，并且为了能够签署该事务，我们需要来自 truffle 框架的`hdwallet-provider`包:`npm install @truffle/hdwallet-provider`

**5。更新 truffle-config.js 文件:**从导入依赖项和解析`.secrets.json`文件开始。(关于配置的更多细节可在[块菌文档](https://www.trufflesuite.com/docs/truffle/reference/configuration#providers)中找到)。

```
const HDWalletProvider = require("@truffle/hdwallet-provider");const fs = require("fs");
const secrets = JSON.parse(fs.readFileSync(".secrets.json").toString().trim());
```

在“网络”部分，添加一个新网络，如下所示:

```
kovan: {
   networkCheckTimeout: 10000,
   provider: () => {
      return new HDWalletProvider(
        secrets.mnemonic,
        `wss://kovan.infura.io/ws/v3/${secrets.projectId}`
      );
   },
   network_id: "42",
},
```

现在，您可以进入终端，使用以下命令将您的项目部署到 Kovan 网络:

```
truffle migrate --network kovan
```

# 下一步是什么

现在你已经有了撰写智能合同的基础，并且开发了你自己的 dapps，你已经准备好进入下一个阶段了。

在这一部分，我会给你一些资源，你可以用它们来提高你作为以太坊开发者的技能:

## 书籍:

[](https://www.amazon.com/Mastering-Ethereum-Building-Smart-Contracts-ebook/dp/B07KGLNL76) [## 掌握以太坊:构建智能合约和 DApps

### 掌握以太坊:建立智能合同和 DApps - Kindle 版

www.amazon.com](https://www.amazon.com/Mastering-Ethereum-Building-Smart-Contracts-ebook/dp/B07KGLNL76) [](https://www.amazon.com/Hands-Contract-Development-Solidity-Ethereum/dp/1492045268/ref=sr_1_1?crid=1BEZJDSK8C00C&dchild=1&keywords=hands+on+smart+contract+development&qid=1614801020&sprefix=hands+on+smart+contract+%2Cdigital-text%2C661&sr=8-1) [## 使用 Solidity 和 Ethereum 进行智能合约开发:从基础到部署

### 使用 Solidity 和以太坊进行智能合约开发:从基础到部署

www.amazon.com](https://www.amazon.com/Hands-Contract-Development-Solidity-Ethereum/dp/1492045268/ref=sr_1_1?crid=1BEZJDSK8C00C&dchild=1&keywords=hands+on+smart+contract+development&qid=1614801020&sprefix=hands+on+smart+contract+%2Cdigital-text%2C661&sr=8-1) [](https://www.amazon.com/Mastering-Blockchain-Programming-Solidity-production-ready-ebook/dp/B07W5F8S1L/ref=sr_1_1?dchild=1&keywords=Mastering+Blockchain+Programming+with+Solidity&qid=1614801145&sr=8-1) [## 用 Solidity 掌握区块链编程:为以太坊编写生产就绪的智能合约…

### Amazon.com:用 Solidity 掌握区块链编程:为以太坊编写生产就绪的智能合约…

www.amazon.com](https://www.amazon.com/Mastering-Blockchain-Programming-Solidity-production-ready-ebook/dp/B07W5F8S1L/ref=sr_1_1?dchild=1&keywords=Mastering+Blockchain+Programming+with+Solidity&qid=1614801145&sr=8-1) 

## 课程:

*   来自[eatheblock-pro](https://eattheblocks-pro.teachable.com/p/6-figure-blockchain-developer)的 6 位数区块链开发者。
*   [Consensys 学院课程](https://learn.consensys.net/catalog)。

## 其他资源:

 [## 以太坊智能合约最佳实践

### 本文档为中级 Solidity 程序员提供了安全考虑的基本知识。这是…

consensys.github.io](https://consensys.github.io/smart-contract-best-practices/) [](https://github.com/ConsenSys/ethereum-developer-tools-list) [## ConsenSys/以太坊-开发者-工具-列表

### 以太坊可用开发工具和平台指南。-ConsenSys/以太坊-开发者-工具-列表

github.com](https://github.com/ConsenSys/ethereum-developer-tools-list)  [## 安全考虑-可靠性 0.8.2 文档

### 虽然构建按预期工作的软件通常很容易，但检查没有人能使用它却要困难得多…

docs.soliditylang.org](https://docs.soliditylang.org/en/v0.8.2/security-considerations.html) 

# 需要记住的一些提示

*   在为生产环境编译智能契约时启用优化器，以优化生成的字节码。
*   如果一个函数需要将以太网传输给接收方，传输操作应该是该函数执行的最后一件事，以防接收方是带有恶意可支付回退(重入攻击)的智能合约。如果在转移之后需要执行一些其他操作，可以考虑使用 openzeppelin 的 ReentrancyGuard。(参见[此链接](https://docs.soliditylang.org/en/v0.8.2/security-considerations.html#use-the-checks-effects-interactions-pattern))
*   不要在像循环或 if/else 语句这样的控制结构中使用以太传输。(与上一点相同，在恶意回退的情况下，如果攻击者无法窃取以太网，他可以执行一些 DOS 攻击)。
*   最好使用[撤回模式](https://github.com/wissalHaji/solidity-coding-advices/tree/master/design-patterns/security#withdrawal-pattern)转移乙醚，以避免重入攻击。
*   不要在一个事务中使用无界循环，这可能导致一个事务由于气体成本超过块气体限制而永远无法到达区块链。(参见[此链接](https://github.com/wissalHaji/solidity-coding-advices/blob/master/best-practices/be-careful-with-loops.md))
*   如果合同不应该接收乙醚，则不要使用`address(this).balance`基于合同余额进行任何逻辑，因为乙醚可以使用`selfdestrct(recipient)`强制发送到合同。
*   使用 openzeppelin 的 SafeMath 库来避免整数溢出和下溢。
*   如果需要根据用户角色限制对功能的访问，可以使用 openzeppelin 库中的 AccessControl 契约。
*   请勿使用`tx.origin`进行授权。
*   在使用函数的参数之前，总是用 require 语句检查它们是否有效。

# 结论

这是本系列中介绍以太坊编程基础的最后一篇文章。如果你觉得你需要更高级的话题，这里有一些开始:[神谕](https://ethereum.org/en/developers/docs/oracles/)，[无气交易](https://docs.openzeppelin.com/learn/sending-gasless-transactions)。
你可能还想看看 github 的一些工具:
[scaffold-eth](https://github.com/austintgriffith/scaffold-eth)
[solidity-template](https://github.com/paulrberg/solidity-template)

往期文章:
[学扎实:简介](/better-programming/learn-solidity-introduction-327b1f9eb30e)
[学扎实:变量第一部分](/better-programming/learn-solidity-variables-part-1-657fc27c2cc1)
[学扎实:变量第二部分](/better-programming/learn-solidity-variables-part-2-f3b842f5bfb8)
[学扎实:变量第三部分](/better-programming/learn-solidity-variables-part-3-3b02ca71cf06)
[学扎实:函数](/better-programming/learn-solidity-functions-ddd8ea24c00d)
[学扎实:智能契约创建与继承](/better-programming/learn-solidity-smart-contract-creation-and-inheritance-8424adac3570)
[学扎实:工厂模式](/better-programming/learn-solidity-the-factory-pattern-75d11c3e7d29)
[用 web3 构建您的第一个 dapp](/better-programming/build-your-first-dapp-with-web3-js-9a7306d16a61)