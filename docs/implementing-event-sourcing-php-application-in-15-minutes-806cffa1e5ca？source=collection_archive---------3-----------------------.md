# 在 15 分钟内实现事件源 PHP 应用程序

> 原文：<https://medium.com/nerd-for-tech/implementing-event-sourcing-php-application-in-15-minutes-806cffa1e5ca?source=collection_archive---------3----------------------->

![](img/6d99363601afc422c47162eb83b7429d.png)

在这篇文章中，我们将直接跳到代码，我们将在 15 分钟内实现事件源应用程序。在开始之前，唯一值得一读的是来自[上一篇](/nerd-for-tech/starting-with-event-sourcing-in-php-161a83597d69)的事件概述。没有时间可以浪费了，我们开始吧。

# 第一分钟—设置项目

1.  创建一个空目录“App”并在*中运行“*作曲初始化”*。*将应用程序命名为“*生态区/应用程序”*，并使用默认值。
    处理后我们应该有 composer.json:

```
{
    "name": "ecotone/app",
    "autoload": {
        "psr-4": {
            "Ecotone\\App\\": "src/"
        }
    },
    "require": {}
}
```

2.现在，让我们要求生态区建兴包事件采购

```
composer require ecotone/lite-event-sourcing-starter
```

# 第三分钟—实施

我们将创建电子钱包，将保持所有交易的日志。
我们将在内存实现中做到不在数据库配置上浪费任何时间。

> 生态交错区由经过良好测试且坚固的[事件存储库](https://github.com/prooph/event-store)提供动力，用于存储事件。
> 除了在内存实现上，我们可以切换到 PostgreSQL/MySQL/MariaDB。

我们将对 Wallet 进行三种可能的操作:

1.  注册新钱包
2.  向钱包中添加钱
3.  从钱包中减去钱

> 请记住，我们是在事件源世界中，所以在执行动作之后，我们返回事件。
> *如果我们想从之前的事件中重建状态，以验证给定的动作是否可以执行，我们可以实现类似* onWalletWasRegistered *的方法。*

让我们看看我们的活动是什么样的:

# 第 9 分钟—建筑投影

所以现在我们希望能够回答这样一个问题，给定的钱包里现在有多少钱。为此，我们将利用投影，它订阅事件并进行计算。

我们的预测会在事件发生时重新计算当前的钱包余额。这也暴露了通过“*getWalletBalance”*来查询它可能性。

# 第 13 分钟—运行示例

我们现在注册钱包，然后加 100 减 40。在这些事件发生后，我们的预测应该会告诉我们，当前的数量是 60。

在项目的根目录下创建名为" *run_example.php"* 的文件

现在，当我们运行它时，结果将是 60。

# 第 15 分钟—总结

我们已经建立了事件源钱包。
生态交错带的目标是直线前进的构型。我们只定义了该功能所需的类，配置量很少。得益于此，我们可以快速实现新功能，并保持代码整洁。

如果你想了解生态交错带框架[，请点击这里](https://github.com/ecotoneframework/ecotone)。
如果你想看上面的实现，去[这个库](https://github.com/dgafka/php-event-sourcing-application-in-15-minutes)。