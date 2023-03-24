# 从 PHP 中的微服务开始

> 原文：<https://medium.com/nerd-for-tech/starting-with-microservices-in-php-6e3c411f3d27?source=collection_archive---------0----------------------->

![](img/e50597acd7cb5523034a3457928bcb75.png)

这是对“[如何集成微服务](https://dariuszgafka.medium.com/how-to-integrate-microservices-a506fe2d1a48)”帖子的跟进。
先读一读以前的文章，感觉受到了鼓励。
在本文中，我们将利用 PHP 将理论应用于实践。

我们将使用[生态交错带框架](https://github.com/ecotoneFramework/ecotone)和 [RabbitMQ](https://www.rabbitmq.com/) ，将两种服务整合在一起。

# 在 PHP 中实现消息传递

在开始之前，我们需要为使用的[框架](https://docs.ecotone.tech/install-php-service-bus) (Symfony/Laravel/Lite)启用 [RabbitMQ 模块](https://docs.ecotone.tech/modules/amqp-support-rabbitmq#configuration)。

我们将构建两个服务。
消费消息的服务，我们称之为“订单服务”和发布消息的服务，*“我的服务”*。
命名很重要，我们需要[在配置](https://docs.ecotone.tech/messaging/service-application-configuration#ecotone-core-configuration)中设置服务名。

在“*订单服务*中，我们将使消费者能够消费来自其他微服务的消息。

在“*我的服务*中，我们将启用分布式总线，这将允许我们向其他微服务发送事件和命令。

# 向订单服务发送命令

让我们从定义*订单服务*中的[命令处理程序](https://blog.ecotone.tech/cqrs-in-php/)开始。

命令处理程序在*“place order”*路由键下可用。

我们现在可以使用分布式总线从 *My Service* 发送命令来下订单:

1.  命令应该发送到的服务名
2.  路由到*订单服务*内的命令处理程序
3.  要发送的数据(命令的有效载荷)
4.  和可选的数据内容类型

执行该代码后，命令消息将被发送到*订单服务*。
*订单服务*现在可以通过运行以下命令来消耗它:

```
(bin/console|artisan) ecotone:run order_service -vvv
```

# 发布事件

假设我们想取消用户的所有订单，以防他的帐户被禁止。
*我的服务*将发布关于用户被禁止的事件，而*订单服务*将订阅该事件。

事件处理程序现在将订阅用路由关键字“ *user.was_banned* ”发布的事件。

1.  事件的名称(路由关键字)
2.  要发送的数据(事件的有效载荷)
3.  有效负载的可选内容类型

执行该代码后，事件消息将被传递给*订单服务*。
*订单服务*现在可以通过跑步来消耗:

```
(bin/console|artisan) ecotone:run order_service -vvv
```

# 进入更多细节…

![](img/bfacd6ca96251dd2521c0ad4f775082c.png)

# 路由方式

```
#[Distributed]
#[CommandHandler("placeOrder")]
public function placeOrder(PlaceOrderCommand $command): void
```

我们使用*“place order”路由名称*将命令路由到特定的处理程序。

```
$this->distributedBus->sendCommand("order_service","placeOrder",...)
```

您可能会遇到这样的解决方案:路由是基于类名的，或者需要在服务之间共享实际的类实现。
服务之间共享 PHP 类，使其成为公共 API。我们不能再简单地更改类名，因为这会中断其他服务。这个职业变得很难改变，因为现在修改涉及到其他人。

第二个选择是同意非 PHP 的公共 API。
我们可以用 JSON 模式代替 PHP 类，并基于自定义名称进行路由，如*“place order”，就像我们在上面的例子*中所做的一样。

> 生态交错带是灵活的，可以根据你的需要进行调整。
> 如果通过共享类来耦合服务在您的环境中没问题，那么您可以这样做。如果你想分离服务，你可以使用自定义名称。

# 使用具有自定义路由的类

即使如此，我们已经使用了自定义路由，我们仍然期待 PlaceOrderCommand 类。

```
#[Distributed]
#[CommandHandler("placeOrder")]
public function placeOrder(PlaceOrderCommand $command): void
```

基于路由，生态区知道它应该执行哪些处理程序，并基于方法声明知道它应该反序列化到哪个类。
你所需要做的就是注册[媒体类型转换器](https://docs.ecotone.tech/messaging/conversion/conversion)，这样 Content 就知道如何将给定的内容类型反序列化为 PHP 类。

> 如果您想使用简单类型，也可以为 string 键入提示以获取 JSON，或者为 array 键入提示。
> 
> 也可以没有任何参数，只根据路由名执行处理程序。

# 发送/发布类

如果您在发布者端注册了[媒体类型转换器](https://docs.ecotone.tech/messaging/conversion/conversion)，那么您可以发送命令和事件类。在发送到 RabbitMQ 之前，econtero 会处理序列化。

# 传递元数据

有些情况下，您会希望用一些元数据来丰富消息。
例如，您可以发布事件并添加当前登录用户的个人 Id。如果我们将它放在有效载荷中，它很容易模糊事件，特别是可能有更多的元数据要存储。

消息类似于信件，信件包含标题(元数据)。
Ecotone 提供的解决方案允许我们以直接的方式添加和发送带有命令和事件的元数据。

假设我们有审计服务，它存储了谁禁止了用户的信息。

或者，如果你想更具体，你可以直接传递具体的头

# 交货保证

有些情况下，您可能希望一次发送多条消息。

如果在发送第二个命令之前或期间，由于任何原因，我们的代码会失败，那么我们可能会陷入只有第一个命令发出的情况。
这会造成服务之间的不一致。

如果你使用本地的[命令](https://docs.ecotone.tech/modelling/command-handling/external-command-handlers) / [事件](https://docs.ecotone.tech/modelling/event-handling/handling-events)处理程序，默认情况下，econtero 在 RabbitMQ 事务中包装这些处理程序。
这保证了您的所有消息将在成功流结束时一起发送。

# 处理大量消息

如果我们将发送大量消息，那么我们将希望只将消息发送给能够处理特定消息的服务，以避免系统上不必要的负载。

> 生态区仅向订阅它的服务发送事件。
> 命令只发送给特定的目标服务。

# 处理错误消息

可能会出现事件或命令处理程序失败的情况。在这种情况下，RabbitMQ 将尝试重新传递消息，直到成功。在此期间，其他消息将被阻止。

生态交错带[有解决方案](https://docs.ecotone.tech/modelling/asynchronous-handling#handling-error-messages)，允许你设置延迟重试。
因此，如果消息失败，我们可以在 X 分钟后重试，在此期间，其他消息将被解除阻止。
在规定的重试次数后，您可以将错误信息存储在[死信](https://docs.ecotone.tech/modules/dbal-support#dead-letter)(数据库)中，以便进一步调查。

# 摘要

生态交错区为构建微服务提供了丰富的支持。
由于它是基于消息传递概念构建的，所以所有工具都是自然的扩展。
消息传递提供了稳定的基础，有助于跳过分布式架构带来的许多复杂性。
econtero 的目标是为开发者提供强大且易于使用的可靠工具。

如果你想看 econtero Lite 中微服务集成的演示实现，你可以在这里查看。
跟进生态交错带框架[点击此处](https://github.com/ecotoneFramework/ecotone)。