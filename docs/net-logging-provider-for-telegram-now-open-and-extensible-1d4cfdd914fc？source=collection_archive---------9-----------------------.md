# 。电报的网络日志提供者——现在是开放的和可扩展的

> 原文：<https://medium.com/nerd-for-tech/net-logging-provider-for-telegram-now-open-and-extensible-1d4cfdd914fc?source=collection_archive---------9----------------------->

前一段时间，我写了一本关于[的出版物。NET Telegram](https://andrey-gubskiy.medium.com/net-logging-provider-for-telegram-b30d087dbfe2?source=your_stories_page----------------------------------------)的日志提供者——一个简单的库，允许向 [Telegram](https://web.telegram.org/) 写日志。

现在我想谈谈我所做的一些改进。我想让这个库更加开放和灵活。所以我添加了很多日志过程定制的选项。

# 改进的日志级别过滤

首先，我更新了日志级别过滤。电报记录器现在考虑不同名称空间的日志级别。

以前，只能通过代码配置电报记录器的全局日志级别:

```
var options = new TelegramLoggerOptions
{
 **...**
 **LogLevel** **= LogLevel.Information**,
 **...**
};
```

或者通过 appsettings.json:

```
{
  "Logging": {
    "LogLevel": {
       ...
    },
    "Telegram": {
      **"LogLevel": "Warning",**
      ...
    }
  },
  ...
}
```

现在记录器配置可以更方便地完成，如[登录所述。网芯和 ASP.NET 芯](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging/?view=aspnetcore-6.0#configure-logging)。

通过 appsettings.json 文件进行配置:

```
{
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft": "Warning",
      "Microsoft.Hosting.Lifetime": "Information"
    },
    "Telegram": {
      **"LogLevel": {
        "Default": "Error",
        "WebApp.Controllers": "Warning"
      }**,
      ...
    }
  },
  ...
}
```

LogLevel 指定记录选定类别的最低级别。当指定了日志级别时，将为指定级别及更高级别的消息启用日志记录。有关更多信息，请参见微软文档门户网站上的[日志级别](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging/?view=aspnetcore-6.0#llvl)。

# 使用自定义日志编写器

现在，开发人员可以使用自己的实现将数据写入 Telegram。自定义编写器应实现 *ILogWriter* 接口:

```
var **customLogWriter = new CustomLogWriter();**
logBuilder.AddTelegram(options, **customLogWriter**);
```

# 使用自定义消息格式化程序

现在可以使用 ITelegramMessageFormatter 来实现定制消息格式。

```
private ITelegramMessageFormatter CreateFormatter(string name)
{
    return new CustomAceTelegramMessageFormatter(name);
}logBuilder..AddTelegram(options, CreateFormatter)
```

要使用自定义消息格式化程序委托 *Func <字符串，ITelegramMessageFormatter>*应传递给扩展方法 *AddTelegram* 。委托，因为格式化程序需要知道哪个类别用于呈现消息。