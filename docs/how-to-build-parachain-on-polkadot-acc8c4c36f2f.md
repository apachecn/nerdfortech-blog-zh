# 如何在 Polkadot 上建立 Parachain？

> 原文：<https://medium.com/nerd-for-tech/how-to-build-parachain-on-polkadot-acc8c4c36f2f?source=collection_archive---------4----------------------->

![](img/00ba1f65c6e29bdba8058fba9a98f9b9.png)

Polkadot 是一个互操作性项目，旨在联合所有区块链。Polkadot 独特的多链结构允许私人和公共区块链相互通信。因此，波尔卡多特可以被认为是一项革命性的创新，因为它充当了不同区块链之间的连接纽带。通过 parachains，Polkadot 网络能够并行处理事务，这有助于它作为一个互连的区块链网络为自己开辟一个利基市场。

# 那么，什么是副链呢？

Polkadot parachain 可以是私有、公共或混合区块链。它也可以是用于 dApp 开发的协议。事实上，副链不一定是区块链，甚至 dApp 也可以发展成副链。因此，副链是项目特定的、定制的区块链或 dApps，集成在 Polkadot 网络中。每个副链获得与连接到 Polkadot 中继链的其他副链的内在互操作性。中继链为副链提供共享安全性、跨链互操作性和一致性。副链和 polkdaot 中继链共同构成了区块链的一个可互操作的和谐生态系统。

# 波尔卡多的架构是如何运作的？

Polkadot 网络有两个工作层:第 0 层和第 1 层。第 1 层包含副链，而第 0 层包含主中继链，所有副链都平行于主中继链。整理器是将副链连接到中继链的特殊节点。它们充当副链的完整节点，并携带与之相关的重要信息。

副链可以有自己的本地令牌，以及关于实现的约定。此外，它们还定义了 collator 节点的激励过程。副链共享 Polkadot 的互操作性和可组合性，这使它们能够与其他副链通信，从而防止主中继链被通过它的每个事务阻塞。中继链除了与整个网络共享安全性之外，还管理赌注交易和治理。Pokadot 方法的一个独特属性是，其他加密项目可以购买副链。为了获得这些罕见的副链位置之一，项目必须赢得拍卖。这些拍卖通常在副链准备好进行测试和部署后举行。点令牌是这些拍卖中的出价货币。在项目获得一个副链之后，他们可以根据自己的需求定制它。

# 设置副链

在波尔卡多特上将你的区块链部署为[降落伞需要你遵循下面提到的步骤](https://www.leewayhertz.com/build-parachain-on-polkadot/)

**编写运行时逻辑**

使用 Polkadot 的底层创建链运行时逻辑。这个过程将类似于创建任何其他 solo 链的运行时逻辑。当您创建逻辑时，请考虑使用以下基底链模板。

`1# Clone the parachain templategit clone https://github.com/substrate-developer-hub/substrate-parachain-template# Switch into the parachain template directorycd substrate-parachain-template# Checkout the proper commitgit checkout polkadot-v0.9.16# Build the parachain template collatorcargo build --release# Check if the help page prints to ensure the node is built correctly./target/release/parachain-collator --help`

**构建 Wasm 可执行文件**

下一步是将运行时逻辑编译成 Wasm 可执行文件。Wasm 代码 blob 将包含链的完整状态转换函数。为了将 parachain 或 parathread 部署到 Polkadot，您将需要这个 Wasm 代码 blob。

**验证 Wasm 代码**

Polkadot 验证器将通过检查您提交的 Wasm 代码来验证 parachain 或 parathread 的状态转换。

**将整理器节点投入使用**

然后，验证器将检查最新的状态转换，并为此使用您的 collator 节点。您的 collator 节点是您的 parachain 的维护者。因此，它必须为您的链生成新的候选块，并将它们发送给 Polkadot 验证器，这样它们就可以将它们包含在 Polkadot 中继链中。

**使用积云将您的链逻辑转换为副链**

Substrate 的内置网络层仅支持单链。它不支持任何连接到中继链的链。积云延伸在这里可以有所帮助。通过将你的区块链变成副线程或副链，Cumulus extension 允许你的衬底构建的链逻辑与 Polkadot 兼容。

# 测试副链

## 洛可可测试网

testnet Rococo 可以用来测试波尔卡多副链，甚至是外部开发的。洛可可的副链测试确保副链和中继链之间的传输和消息不会未经检查就通过。信息首先到达中继链，从那里它们被发送到副链。洛可可使用积云和 HRMP 进行测试。

## 获取 ROC 令牌

要测试副链，您需要 ROC 令牌。ROC 代币在洛可可龙头频道的 Matrix 上有售。您可以使用下面提到的命令来获取 ROC 令牌。

`1!drip YOUR_ROCOCO_ADDRESS​`

## 注册并建立一个洛可可副线程

注册中继链时，rococo 副链共享相同的运行时代码，但使用不同的副链 id。

要运行洛可可整理器，您需要编译以下二进制文件:

`1cargo build --release --locked -p polkadot-collator`

构建完可执行文件后，就该为您的 parachain 启动整理器了:

`1./target/release/polkadot-collator --chain $CHAIN --validator`

# 展开副链

一旦你的副链通过了跨链测试，它就可以被部署到 Polkadot 系统上了。为此，您需要获得一个副链插槽。

基于基底的链在其地址格式中使用 SS58 编码。检查哪个链对应于特定的前缀，以及哪些前缀当前可用。

**获得一个副链槽**

为了让 parachains 连接到 Polkadot 的网络，它们需要占用任何可用的插槽。Parachain 插槽是 Polkadot 上的有限资源，它们只在少数情况下解锁，并且每隔几个月就会变得可用。副链必须有一个副链插槽，以确保其包含在中继链中。

**通过蜡烛拍卖分配副链位置**

Parachain 插槽是通过修改区块链证券蜡烛拍卖出售。

蜡烛拍卖是一种在线拍卖，竞标者出价最高，直到出价最高者胜出。在线蜡烛拍卖通常使用一个随机数来标记拍卖的结束，因此，投标人不知道开始阶段的持续时间。Parachain 蜡烛拍卖被修改，因为它不使用随机数来确定其持续时间。它为投标人提供了一个预先确定的开标阶段，并追溯确定了截止期。

值得注意的是，在开标阶段的后半部分进行的投标可能会被拒绝，因为追溯确定的截止时间可能发生在投标提交之前。

# 尾注

从互操作性到可伸缩性和共享安全性，拥有副链的好处是无穷无尽的。此外，Polkadot 是一个快速发展的区块链协议，初创公司和企业都将其作为区块链开发的基础。所以，如果你想开发一个 parachain 项目或者测试你的 parachain，那么寻求专业帮助将是最好的选择。然而，只与波尔卡多特专家签订合同。