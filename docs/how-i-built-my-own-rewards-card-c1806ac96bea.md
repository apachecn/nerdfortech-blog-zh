# 我如何制作自己的奖励卡

> 原文：<https://medium.com/nerd-for-tech/how-i-built-my-own-rewards-card-c1806ac96bea?source=collection_archive---------5----------------------->

![](img/8abb9d0c35b28f1fd53212faffdb7e44.png)

在亚马逊引起我的注意之前，我在参观实体技术零售商以检查新的和令人兴奋的产品中找到了乐趣。我拜访次数最多的一家零售商是百思买。大约在那个时候，有人问我是否有兴趣在结账过程中注册一张百思买积分卡。听说我在百思买购买的每一件商品都可以兑换积分，从而获得财务奖励，这引起了我的注意，我立即注册了。

我很兴奋。

我还会对百思买积分卡感到兴奋吗？不完全是…主要是因为我的购物习惯已经改变了，老实说，我不能告诉你我最后一次向一次性电子超市的领导者下订单是什么时候。

我不认为我的经历与大多数在百思买积分卡计划宣布时加入的人有什么不同。因为从单一来源购买尽可能多的商品而获得奖励的吸引力不再具有吸引力——当奖励的资金必须花在认识到客户忠诚度的实体上时。

当我更多地思考这个问题时，我看到购买基于现金的 Discover/Mastercard/Visa 礼品卡是多么容易，我开始想知道提供一种现代的奖励卡是多么容易，消费者可以在任何接受该卡的地方使用该卡。

# 奖励卡概念

奖励卡概念的核心是激励购物者在当今全球市场的无尽选择中选择您的企业。他们从你这里买的越多，他们期望得到的回报就越多。

由于今天的购物者对可以在任何地方使用的奖励更感兴趣，理想的奖励卡概念应该利用诸如 Discover、Mastercard 或 Visa 之类的货币技术，这些技术目前在任何地方都被广泛接受。

# 举个例子:维斯特父子公司

例如，让我们假设 Vester & Son's 是一家在线零售商，希望通过奖励卡计划增加销售额。当购物者注册 Vester & Son 的奖励计划时，他们只需要在 Vester & Son 的电子商务网站上拥有一个帐户。这对大多数客户来说并不困难，因为他们在每次购买时已经提供了以下必需的信息:

*   全名
*   电子邮件地址
*   电话号码
*   邮寄地址

一旦顾客购物金额超过 100 美元，Vester & Son's 将提供一张预装总购物金额 10%的发现卡。也就是说，在 Vester & Son 的产品上每花 10 美元，就会得到 1 美元的回报。不错的交易，对吧？

顾客可以在任何接受 Discover 的地方使用 Vester & Son 的 Rewards Discover 卡。

# 使用 Marqeta 作为奖励卡来源

在我今年早些时候发表的“[利用 Marqeta 在 Spring Boot 建立支付服务](https://betterprogramming.pub/build-an-uber-like-payment-service-using-spring-boot-f6cfb45f67d8)”一文中，我详细介绍了 Marqeta 为优步、DoorDash 和 Square(仅举几例)的热门服务使用的以下交易流程:

![](img/83e37a8c1362e4a4a03f2cb020210ca0.png)

事实证明，使用 Marqeta 为全球接受的奖励卡提供资金遵循非常相似的流程:

![](img/5464ba9383134c9f6f5678b96ebec7a7.png)

在本例中，Vester & Son's 为奖励卡计划提供了资金来源。由于每个客户都有资格参加该计划，因此基于 Discover 的奖励卡上的资金可供使用。

虽然顾客可以使用 Discover 卡在 Vester & Son's 购物，但没有什么可以阻止从任何地方购买任何东西，包括从 Vester & Son 的竞争对手那里购买。

## 创建奖励卡计划

利用 Marqeta API，我能够使用以下 cURL 命令为 Vester & Son 的奖励卡程序建立一个新程序:

```
curl --location --request POST '[https://sandbox-api.marqeta.com/v3/fundingsources/program'](https://sandbox-api.marqeta.com/v3/fundingsources/program') \
--header 'accept: application/json' \
--header 'Content-Type: application/json' \
--header 'Authorization: Basic APPLICATION_TOKEN_GOES_HERE:ADMIN_ACCESS_TOKEN_GOES_HERE' \
--data-raw '{"name":"vester_rewards_card_program","active":true}'
```

返回了以下响应，其中包括本文后面将引用的令牌属性:

# 一个简单的例子(使用 cURL 命令)

对于“利用 Marqeta 在 Spring Boot 构建支付服务”这篇文章(如上所述)，我创建了一个 Spring Boot API 服务作为 Marqeta API 的前端，它可以在 GitLab 的以下 URL 找到:

https://gitlab.com/johnjvester/marqeta-example

我将在本出版物中继续使用这项服务。

# 寻找客户

Spring Boot 用户 API 得到了增强，可以返回给定客户的 Marqeta 用户数据，这样做是利用用户令牌作为 URI 中的惟一键。使用 [Randy Kern](https://twitter.com/randykern) 用户令牌(来自我之前的出版物)，我们可以发送以下 cURL 请求:

```
curl --location -X GET 'localhost:9999/users/1017b62c-6b61-4fcd-b663-5c81feab6524'
```

该请求返回以下响应有效负载:

```
{
    "token": "7193b62c-6b61-4fcd-b663-5c81feab6524",
    "createdTime": 1628946073000,
    "lastModifiedTime": 1628946074000,
    "metadata": {},
    "active": true,
    "firstName": "Randy",
    "lastName": "Kern",
    "usersParentAccount": false,
    "corporateCardHolder": false,
    "accountHolderGroupToken": "DEFAULT_AHG",
    "status": "ACTIVE"
}
```

## 查找奖励卡

一旦顾客购买了 100 美元，Vester & Son 的电子商务网站就会自动建立一张新的 Vester & Son 的奖励卡。为了模拟这个动作，我们向 Marqeta API 发送以下 cURL 请求:

```
curl --location --request POST '[https://sandbox-api.marqeta.com/v3/fundingsources/paymentcard'](https://sandbox-api.marqeta.com/v3/fundingsources/paymentcard') \
--header 'accept: application/json' \
--header 'Content-Type: application/json' \
--header 'Authorization: Basic APPLICATION_TOKEN_GOES_HERE:ADMIN_ACCESS_TOKEN_GOES_HERE’ \
--data-raw '{"postal_code":"46077","account_number":"6559906559906557","exp_date":"1225","cvv_number":"123","user_token":"1017b62c-6b61-4fcd-b663-5c81feab6524","is_default_account":true}'
```

响应有效负载包括为 Randy Kern 客户新创建的 Discover 卡信息:

```
{
  "created_time": "2021-12-28T11:54:08Z",
  "last_modified_time": "2021-12-28T11:54:08Z",
  "type": "paymentcard",
  "token": "2ee44d0b-5d00-4744-af2d-8ab9c8c606b8",
  "account_suffix": "6557",
  "account_type": "DISCOVER",
  "active": true,
  "is_default_account": true,
  "exp_date": "1225",
  "user_token": "1017b62c-6b61-4fcd-b663-5c81feab6524"
}
```

请注意:卡的到期日期可能必须符合卡提供商的标准(不能是无限的)。在这些情况下，Vester & Son 的奖励卡计划将具备必要的业务逻辑，以便在到期日临近时向客户发送新卡。

下面是维斯特父子公司给兰迪·克恩的奖励卡的一个例子:

![](img/5e631d5de83fbfb20289ce807cc7e169.png)

识别用户令牌后，Spring Boot 服务可以通过以下 cURL 命令轻松定位 Randy Kern 用户的 Vester & Son's rewards card 支付卡:

```
curl --location --request GET 'localhost:9999/paymentcards/user/7193b62c-6b61-4fcd-b663-5c81feab6524'
```

以下响应包括与 Randy Kern 客户相关的所有支付卡:

```
[
    {
        "token": "2ee44d0b-5d00-4744-af2d-8ab9c8c606b8",
        "createdTime": 1640692448000,
        "lastModifiedTime": 1640692448000,
        "type": "paymentcard",
        "active": true,
        "userToken": "7193b62c-6b61-4fcd-b663-5c81feab6524",
        "accountSuffix": "6557",
        "accountType": "DISCOVER",
        "expDate": "1225",
        "defaultAccount": true
    }
]
```

使用支付卡令牌，我们发送了以下 cURL 请求来检索单个支付卡:

```
curl --location --request GET 'localhost:9999/paymentcards/2ee44d0b-5d00-4744-af2d-8ab9c8c606b8'
```

这将返回仅限于支付卡令牌的有效负载，前提是:

```
{
    "token": "2ee44d0b-5d00-4744-af2d-8ab9c8c606b8",
    "createdTime": 1640692448000,
    "lastModifiedTime": 1640692448000,
    "type": "paymentcard",
    "active": true,
    "userToken": "7193b62c-6b61-4fcd-b663-5c81feab6524",
    "accountSuffix": "6557",
    "accountType": "DISCOVER",
    "expDate": "1225",
    "defaultAccount": true
}
```

Vester & Son 的电子商务网站将为加入 Vester & Son 的奖励卡计划的每个客户存储用户令牌和支付卡令牌。这将使交叉引用给定客户的给定奖励卡变得容易。

随着客户赢得更多奖励，该计划会增加客户奖励卡上可用于消费的资金。添加资金就像调用 API 一样简单。从那里，顾客可以在任何接受 Discover 的地方消费他们的 Vester & Son 奖励。

# 结论

从 2021 年开始，我一直努力实践以下使命宣言，我觉得这适用于任何 IT 专业人士:

> *“将您的时间集中在提供扩展您知识产权价值的特性/功能上。将框架、产品和服务用于其他一切。”*
> 
> *——j·维斯特*

Marqeta 当然符合我的使命陈述，因为他们的服务提供了创建奖励卡计划的所有必要组件，允许在接受所选卡产品(如 Discover、Mastercard、Visa)的任何地方进行购买。

回想起来，百思买奖励计划并不是我购买技术时使用的第一个计划。当我还在上大学的时候， [EggHead 软件](https://en.wikipedia.org/wiki/Egghead_Software)商店在全美蓬勃发展，为蓬勃发展的个人电脑市场提供无穷无尽的软件和配件。

注册一张 EggHead 优惠卡给了我全年所有购物和其他特价商品 5%的折扣。大学版的我没有意识到他们可能会跟踪我的购买，以帮助引导我未来的购买……但老实说，我认为大学版的我也不会在意。我在攒钱，买新软件用。

但是奖励卡的概念起了作用……我选择先去 EggHead 购物。

如果您对使用我为本文创建的 Spring Boot 服务感兴趣，可以从 GitLab 的以下 URL 获得该项目:

[https://gitlab.com/johnjvester/marqeta-example](https://gitlab.com/johnjvester/marqeta-example)

祝你今天过得愉快！