# Log4J2 和 JUnit 5 |如何用 JUnit 5 测试日志

> 原文：<https://medium.com/nerd-for-tech/log4j2-and-junit-5-how-to-test-logs-with-junit-5-e5fa3eec82d2?source=collection_archive---------3----------------------->

![](img/da5a1ebedd1653e1f8ea2e08f5bb9a93.png)

照片由 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的 [Petri R](https://unsplash.com/@petri_r?utm_source=medium&utm_medium=referral) 拍摄

最近我遇到了一个问题，我需要为我的应用程序记录一些警告。因为我通常从事测试驱动的工作，这对我来说非常重要，我可以为它创建一些单元测试。我的测试引擎是 [JUnit](https://junit.org/junit5/) 5。我找到了日志库 [Log4J](https://logging.apache.org/log4j/log4j-2.2/index.html) ，从版本 2 (Log4J2)开始有了新名字，用的就是这个。

# Log4J2 是什么？

Apache Log4j 2 是 Log4j 的升级版，Log4j 在业界很常见，它提供了对其前身 Log4j 1.x 的重大改进，并提供了 Logback 中的许多改进，同时修复了 Logback 架构中的一些固有问题。

# 如何使用 Log4J2

在我们使用 Log4J2 之前，我们必须配置一些东西。

## 属国

首先，我们来看看[的依赖关系](https://logging.apache.org/log4j/log4j-2.2/maven-artifacts.html):

如您所见，我为 log4j 添加了两个实现，一个是 api，另一个是核心本身。应该是同一个版本。另外，我增加了 hakky54 的[日志捕获者](https://github.com/Hakky54/log-captor)。这对以后的单元测试非常重要。

## 配置文件

Log4J 需要[配置](https://logging.apache.org/log4j/log4j-2.2/manual/configuration.html)文件，这样它可以只显示相关的日志，而不是全部。

1.  Log4j 将检查“log4j.configurationFile”系统属性，如果设置了该属性，将尝试使用与文件扩展名匹配的 ConfigurationFactory 加载配置。
2.  如果没有设置系统属性，YAML 配置工厂将在类路径中查找 log4j2-test.yaml 或 log4j2-test.yml。
3.  如果没有找到这样的文件，JSON ConfigurationFactory 将在类路径中查找 log4j2-test.json 或 log4j2-test.jsn。
4.  如果没有找到这样的文件，XML ConfigurationFactory 将在类路径中查找 log4j2-test.xml。
5.  如果找不到测试文件，YAML 配置工厂将在类路径中查找 log4j2.yaml 或 log4j2.yml。
6.  如果找不到 YAML 文件，JSON ConfigurationFactory 将在类路径中查找 log4j2.json 或 log4j2.jsn。
7.  如果找不到 JSON 文件，XML ConfigurationFactory 将尝试在类路径中找到 log4j2.xml。
8.  如果找不到配置文件，将使用默认配置。这将导致日志记录输出到控制台。

这是一个基本 xml 配置文件的示例。它使用控制台以定义的模式打印出日志。它看起来会像这样:

```
07:39:56.263 [main] WARN  ClassToLog - Error Message
```

可能最重要的部分是 Logger 标签。我们可以定义它的名称，它通常是我们需要记录的类的名称。

接下来是关卡。这是我们可以定义的日志级别，确切地说应该记录什么。例如，可能的值有:错误、警告、信息等。

可加性也很重要。如果我们把这部分去掉，信息会打印两次。第一次来自根记录器，第二次来自自定义记录器。为了防止这种情况，我们必须将 additivity 设置为 false。

关于日志级别的更多信息，我创建了另一个[博客](/codex/log4j2-create-custom-log-levels-and-how-to-use-them-48685e133fd1):

## 如何记录

这是我要记录的班级。它在日志管理器的帮助下创建一个新的日志记录器。getLogger()也可以有一个 string 参数，但是因为我们无论如何都会使用 classname，这是默认的，所以我们不必设置它。

然后我们可以调用 logger.warn(" ")，并在其中显示错误消息。它不一定是一个字符串。它也可能是一个例外或一个数字。

我们也可以设置一些其他的值来代替 warn。此属性的有效值为“跟踪”、“调试”、“信息”、“警告”、“错误”和“致命”。

# 如何测试日志

因为我们喜欢用单元测试来测试一切，所以让我们也用日志来测试。遗憾的是，JUnit 中没有集成 appender 来测试日志。我们需要自己定义它，因此会产生大量样板代码。

更好的解决方案是使用我之前在 build.gradle.kts 中添加的插件。它为我们管理所有的东西，非常容易使用。

1.  创建我们想要测试的类的对象，这样我们可以稍后对它执行函数调用。
2.  创建日志捕获器。类名需要与之前定义的相同。
3.  现在我们可以调用应该记录警告的函数。
4.  使用 logCaptor.warnLogs，我们可以准确地看到为这个类记录器记录了哪些级别为“warning”的日志。因为我们在这个类中只记录了一个警告，所以我们可以使用 containsExactly()。

# 反射

## 什么进展顺利？

Log4J2 的日志本身工作得非常好，非常快。我能够很容易地记录一些非常基础的东西。此外，logCaptor 的表现比预期的要好。

## 有哪些需要改进的地方？

我遇到的最大问题是，有很多关于如何用 JUnit 测试日志的资源，但实际上它们都使用旧版本的 Log4J，或者使用与 JUnit5 非常不同的 JUnit4。例如，JUnit 5 中不赞成使用规则注释。我自己尝试了很多编写 appender 的方法，但是总是失败。最后，logCaptor 对我来说是唯一真正的解决方案。

希望这篇文章能帮到你。快乐伐木😊