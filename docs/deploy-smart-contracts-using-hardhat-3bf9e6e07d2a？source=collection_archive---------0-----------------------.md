# 使用 Hardhat 部署智能合同

> 原文：<https://medium.com/nerd-for-tech/deploy-smart-contracts-using-hardhat-3bf9e6e07d2a?source=collection_archive---------0----------------------->

![](img/80236192ec67403a4b6219c37e510dd3.png)

# 介绍

智能合约只是保存在区块链上的程序。满足预设条件时运行。它们通常用于自动履行协议。因此，所有参与者都可以直接确定结果，没有任何调解人的参与或时间损失。

[Hardhat 插件增强了将合同部署到任何网络的机制。它提供了一个编译、测试和部署 Solidity 智能合同的环境。在本帖中，我们将了解如何使用 Hardhat 部署智能合同。](https://www.technologiesinindustry4.com/)

# 描述

Hardhat 还改进了将姓名与地址相关联的机制。因此，测试和部署脚本可以通过仅仅移动名称指向的地址来重新配置，允许每个网络的不同配置。这也导致了更清晰的测试和部署脚本。

# 主要特征

*   [Hardhats trust ethers.js，第二代以太坊 JavaScript API 库。](https://www.technologiesinindustry4.com/)
*   它与 TypeScript——第二代 JavaScript——混合得很好。
*   用 Solidity 代码中的 console.log 功能带来了更好的调试能力。
*   通过网络确定性部署。
*   能够从配对网络访问部署。
*   在松露的帮助下，从 npm 包装等外部来源引入产品。
*   链配置导出。
*   列出已部署合同的地址及其 ABI。
*   部署时的库链接。

# 设置

准备好项目并作为依赖项安装 Hardhat。

```
$ npm init --yes$ npm install --save-dev hardhat
```

安装完成后，我们可能会运行 npx hardhat。这将在项目目录中创建一个 Hardhat 配置文件(hardhat.config.js)。

```
$ npx hardhat888    888                      888 888               888888    888                      888 888               888888    888                      888 888               8888888888888  8888b.  888d888 .d88888 88888b.   8888b.  888888888    888     "88b 888P"  d88" 888 888 "88b     "88b 888888    888 .d888888 888    888  888 888  888 .d888888 888888    888 888  888 888    Y88b 888 888  888 888  888 Y88b.888    888 "Y888888 888     "Y88888 888  888 "Y888888  "Y888Welcome to Hardhat v2.2.1What do you want to do? · Create an empty hardhat.config.jsConfig file created
```

*   [实体源文件(。sol)存储在联系人目录中。这和我们可能从其他语言中熟悉的 src 目录是一样的。](https://www.technologiesinindustry4.com/2021/11/speech-recognition-transformation.html)
*   我们现在可以编写第一个简单的智能契约，命名为 Box。
*   它将允许人们储存一个以后可以保存的价值。
*   我们将把这个文件保存为 contracts/Box.sol。
*   每个。sol 文件应该有单个合同的代码，并在它之后被调用。

```
/ contracts/Box.sol// SPDX-License-Identifier: MITpragma solidity ^0.8.0;contract Box { uint256 private _value; // Emitted when the stored value changes event ValueChanged(uint256 value); // Stores a new value in the contract function store(uint256 value) public { _value = value; emit ValueChanged(value); } // Reads the last stored value function retrieve() public view returns (uint256) { return _value; }}
```

[我们现在将生成一个脚本来部署我们的箱式合同。我们将这个文件保存为脚本或 deploy.js](https://www.technologiesinindustry4.com/2021/11/speech-recognition-transformation.html)

```
// scripts/deploy.jsasync function main () { // We receive the contract to deploy const Box = await ethers.getContractFactory('Box'); console.log('Deploying Box...'); const box = await Box.deploy(); await box.deployed(); console.log('Box deployed to:', box.address);}main() .then(() => process.exit(0)) .catch(error => { console.error(error); process.exit(1); });
```

现在，使用下面的命令在脚本中安装 ethers。

```
$ npm install --save-dev @nomiclabs/hardhat-ethers ethers
```

然后，我们需要添加配置。

```
/ hardhat.config.jsrequire('@nomiclabs/hardhat-ethers');...module.exports = {...};
```

[我们可以使用 run 命令将 Box contract 部署到本地网络(hard hat Network):](https://www.technologiesinindustry4.com/)

```
$ npx hardhat run --network localhost scripts/deploy.jsDeploying Box...Box deployed to: 0x5FbDB2315678afecb367f032d93F642f64180aa3
```

# 合同

*   [我们将从 Open Zeppelin 导入一个可靠性合同。](https://www.technologiesinindustry4.com/2021/11/cnn-image-classification.html)
*   在新的文件夹契约下创建一个名为 ERC20.sol 的文件。
*   ERC20 契约将在部署时接受名称和符号参数。

```
/SPDX-License-Identifier: Unlicensepragma solidity 0.8.3;import "@openzeppelin/contracts/token/ERC20/ERC20.sol";contract _ERC20 is ERC20 { constructor (string memory _name, string memory _symbol) ERC20(_name, _symbol) {}}
```

# 脚本部署

*   我们将控制 hardhat-deploy 插件来部署合同。
*   除了一系列功能之外，它还允许我们执行和跟踪部署。
*   第一个部署脚本位于 deploy 文件夹中，如下所示:

deployer 常量保持了 namedAccounts 特性的作用。它使我们能够根据地址在 hardhat.config.js 中给定网络的 accounts 数组中的索引，将自定义名称与地址关联起来

因此，在我们的示例中，将 deployer 设置为零允许我们使用 const { deployer } = await getNamedAccounts()来访问私钥。

[否则，参数必须按照智能合约所期望的顺序，并且标记可用于标识或分组合约以进行部署。](https://www.technologiesinindustry4.com/2021/11/speech-recognition-transformation.html)

# 部署

我们可以用下面的命令来编译我们的契约:

```
$ npx hardhat deploy --network kovan --tags ERC20
```

更多详情请访问:[https://www . technologiesinindustry 4 . com/2021/11/deploy-smart-contracts-using-hard hat . html](https://www.technologiesinindustry4.com/2021/11/deploy-smart-contracts-using-hardhat.html)