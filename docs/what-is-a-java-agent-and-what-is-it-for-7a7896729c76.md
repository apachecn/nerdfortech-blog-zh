# 什么是 Java 代理，它有什么用途？

> 原文：<https://medium.com/nerd-for-tech/what-is-a-java-agent-and-what-is-it-for-7a7896729c76?source=collection_archive---------5----------------------->

![](img/4f18a6c2cf9afb43749401061b8899b6.png)

最近，我们在那些帖子中看到了如何执行一些监控任务。今天我们将看到 Java 代理，它们是什么以及它们是如何工作的。这也可以帮助我们执行监测任务等。在这篇文章中，我们将尝试解释一些东西，首先是一些理论，然后是例子的帮助。

什么是 Java 代理？，它们基本上是 Java 库，包括实现 Java Instrumentation API 的类。从 JDK 1.5 版开始提供。这是一个非常简单的 API，但同时也非常强大。它为应用程序检测奠定了基础。

这个工具的主要功能是允许我们向某些类添加代码。有了这个附加功能，我们将能够重新编译主要用于监控和分析或事件记录的数据。允许我们获得有关应用程序的重要信息，这可以帮助我们解决问题或获得数据，否则这是不可能的。

这个插装 API 的主要特点是改变虚拟机正在执行的类的字节码。通过这种改变，我们将能够执行前面指出的操作。

但是让我们来关注一下，创建一个 Java 代理需要什么？我们只需遵循以下三个步骤:

1.  制定具体的方法。
2.  创建并正确配置清单。MF 文件。
3.  告诉虚拟机用 Java 代理考虑我们的库。

开发方法取决于我们何时希望代理与 JVM 中应用程序的执行相关联。检测 API 将为我们提供两种不同的方法:

*   premain :它将允许在 JVM 中执行任何其他应用程序之前执行 Java 代理及其相关方法。在 JVM 启动时必须指示 Java 代理。
*   *agentmain* :一旦 JVM 已经在运行一个应用程序，它将允许 Java 代理的执行。这种关联必须以编程方式完成。

清单的创建。MF 文件必须手动完成，尽管它可以通过 maven 插件包含在相关联的 jar 中。在这个文件中，我们必须配置不同的属性。一方面，它是包含 Java 代理方法及其行为属性的类。主要的有:

*   Premain-Class:表示实现 *premain* 方法的类。
*   Agent-Class:表示实现 *agentmain* 方法的类。
*   Can-Redefine-Classes:表示 Java 代理是否可以重定义它所干涉的类。
*   Can-Retransform-Classes:这允许指明 Java 代理是否可以转换干扰类。

一旦我们开发了执行逻辑的方法并正确配置了清单文件，我们就可以生成库了。为此，我们可以使用几个 maven 插件，这将使我们的工作更容易:

*   maven-shade-plugin:它允许我们创建包含依赖项的库，以防万一。
*   maven-jar-plugin:允许我们将清单文件与要创建的库相关联。

一旦我们有了这个库，第三步就是将它与 JVM 关联起来。如前所述，这可以通过两种方式实现:

如果我们已经实现了 *premain* 方法并配置了 Premain-Class 属性，我们将不得不在启动我们想要干扰的应用程序时指明库。示例:

```
java -javaagent:/full/path/to/javaAgent.jar -jar app.jar
```

如果我们实现了 *agentmain* 方法并配置了代理类属性，我们必须通过 Java Attach API 以编程方式关联 Java 代理。

```
File agentFile = Paths.get("agent.jar").toFile();
VirtualMachine jvm = VirtualMachine.attach(VirtualMachine.list().get(**0**).id());
jvm.loadAgent(agentFile.getAbsolutePath());
// jvm.detach(); //to finish association
```

现在我们知道了理论，所以我们可以去实践了。让我们看几个例子。一个简单基本，另一个稍微复杂一点。

在最简单的例子中，我们将只实现 *premain* 方法，并且我们将设置几个与其执行相关的日志。

例如，如果我们执行典型的*静态 void main* 方法，并添加 java 代理作为 JVM 参数，我们可以看到类似如下的输出:

```
[INFO ] 2021-03-04 [main] BasicAgentExample - Start from premain. agentArgs: null
[INFO ] 2021-03-04 [main] BasicAgentExample - Classname: sun/misc/PostVMInitHook....
```

正如我们所看到的，如果我们稍微看一下 Instrumentation 对象中可用的方法，我们将有可能转换或重新定义一个类的功能。但是这必须通过修改这些类的字节的复杂操作来完成。

在下面这个更复杂的例子中，我们将转换将要干预的类。但是正如我们已经指出的，这些操作是复杂的。因此，为了更容易地执行它们，我们将利用[字节伙伴库](https://github.com/raphw/byte-buddy)。

这个例子的想法是创建一个 Java 代理，它允许我们测量某个方法被调用了多少次。此外，我们还将过滤我们想要干预的类，避免在我们不感兴趣的类上执行任务，例如来自 JVM 的那些类。在我们的例子中，我们将只干预那些扩展抽象类的类中的方法。

首先，我们实现了 *premain* 方法:

*AgentBuilder* 构造函数将允许我们通过 *type* 方法和 *ElementMatchers* 实用程序来指示我们想要干预的元素。 *transform* 方法将允许我们指出我们想要在类的哪个元素上执行转换，在这个例子中是在已经通过过滤器的类的方法上。

下一步是创建一个*通知*，它是一个与方面编程(AOP)相关的元素，允许我们在特定的点执行代码。我们创建的*建议*可以有不同的类型，这取决于我们希望它们何时被考虑到那个固定点:在那个特定点之前或之后。在这个例子中，我们将在访问满足过滤器的方法之前执行我们的*建议*。

现在我们只需要创建我们想要工具化的类。

为了测试我们的 Java 代理，我们将执行上面显示的类。在清单文件中正确指示 Java 代理库和实现该代理的类。输出如下所示:

```
[INFO ] 2021-03-04 [main]  - CounterMeasureAdvice: Enter into: public static int ExampleThree.fibonacci(int)
[INFO ] 2021-03-04 [main]  - Origin public static int ExampleThree.fibonacci(int) invoked 14 times
[INFO ] 2021-03-04 [main]  - CounterMeasureAdvice: Enter into: public static int ExampleThree.fibonacci(int)
[INFO ] 2021-03-04 [main]  - Origin public static int ExampleThree.fibonacci(int) invoked 15 times
```

可以看到，Java 代理的使用有很多可能性。即使有了 Byte Buddy 库，也可以用简单的方式做复杂的事情。