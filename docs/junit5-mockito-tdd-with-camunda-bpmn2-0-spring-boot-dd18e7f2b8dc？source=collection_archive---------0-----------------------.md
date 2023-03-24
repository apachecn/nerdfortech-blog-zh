# 6 月 5 日，Mockito，TDD，带 Camunda BPMN2.0 弹簧靴

> 原文：<https://medium.com/nerd-for-tech/junit5-mockito-tdd-with-camunda-bpmn2-0-spring-boot-dd18e7f2b8dc?source=collection_archive---------0----------------------->

> *“哦不！！更新的代码有效。但是……现有的工作流程正在崩溃。怎么回事？”*

如果您现有的流程有一个*测试用例*，并且您已经在部署之前运行了测试用例，那么这可能是可以避免的。如果这是一个自动化的部署，那么你将有**测试步骤**在某个地方会失败。

> 这个博客回答的问题是**Camunda 如何帮助编写 cam unda 工作流的测试用例**？

*你可以阅读我早前的博客来* [*了解 BPMN 2.0 卡蒙达样片*](/nerd-for-tech/bpmn2-0-camunda-workflow-spring-boot-application-2381f3d42e5f) *。你可以在 GitHub* *上找到这两个博客的* [***代码。***](https://github.com/sourabhparsekar/camunda-masala-noodles)

![](img/9207e30b5d31866ef65dcb514af37970.png)

[**软件测试**](https://rezaid.co.uk/services/software-testing/) 检查软件应用程序和产品的缺陷和错误，以确保其性能高效可靠。

软件测试是一个连续的过程，用于识别、评估和确认业务需求规格是否得到满足。

[***Camunda 工作流引擎***](https://camunda.com/developers/getting-started/) 嵌入 springboot 在测试用例方面没有什么不同。我们可以为每个 camunda 流程步骤(单元测试)或整个流程(集成测试)编写测试，但我们必须为两者都编写测试，以符合业务需求规范。

# JUnit 5 是什么？

根据 JUnit 用户指南:

JUnit 5 由来自三个不同子项目的几个不同模块组成。

[**JUnit 5**](https://junit.org/junit5/docs/current/user-guide/)**=*JUnit 平台* + *JUnit 木星*+*JUnit Vintage***

**JUnit 平台**作为[在 JVM 上发布测试框架](https://junit.org/junit5/docs/current/user-guide/#launcher-api)的基础。它还定义了用于开发在平台上运行的测试框架的`[**TestEngine**](https://junit.org/junit5/docs/current/api/org.junit.platform.engine/org/junit/platform/engine/TestEngine.html)` API。

**JUnit Jupiter** 是新的[编程模型](https://junit.org/junit5/docs/current/user-guide/#writing-tests)和[扩展模型](https://junit.org/junit5/docs/current/user-guide/#extensions)的组合，用于在 JUnit 5 中编写测试和扩展。Jupiter 子项目提供了一个`**TestEngine**` 用于在平台上运行基于 Jupiter 的测试。

**JUnit Vintage** 为在平台上运行基于 JUnit 3 和 JUnit 4 的测试提供了一个`**TestEngine**` 。它要求 JUnit 4.12 或更高版本出现在类/模块路径中。

您可以访问 [JUnit 5 用户指南](https://junit.org/junit5/docs/current/user-guide/#overview)了解更多信息。

# 什么是莫奇托？

顾名思义，Mockito 框架用于创建 Mocks，从接口到最终的类。它让你用一个干净简单的 API 编写漂亮的测试。Mockito 测试可读性很强，并且产生干净的验证错误

可以访问 [Mockito 框架](https://site.mockito.org/)了解更多。

# 什么是 Camunda Mockito？

如果你做过 Camunda 工作流代码，写过一些测试，那么你肯定知道 Camunda 中有太多的东西需要模仿和 stub。Camunda Mockito 简化了这个模拟和测试过程。

camunda-bpm-mockito 是 Camunda BPM 流程引擎的社区扩展，旨在简化和自动化流程应用程序的模拟。

可以访问[cam unda-BPM-mock ITO](https://github.com/camunda-community-hub/camunda-bpm-mockito)github 资源库了解更多。

我们将使用 JUnit 5 和 Camunda Mockito 来编写单元和集成测试。然而，我们还可以用 Camunda 工作流做一件事，即 TDD——测试驱动开发

# 什么是 TDD？

这是一个有争议的话题。然而，根据定义，“[测试驱动开发](https://www.agilealliance.org/glossary/tdd/#q=~(infinite~false~filters~(postType~(~'page~'post~'aa_book~'aa_event_session~'aa_experience_report~'aa_glossary~'aa_research_paper~'aa_video)~tags~(~'tdd))~searchTerm~'~sort~false~sortDirection~'asc~page~1))”指的是三种活动紧密交织的编程风格:编码、测试([单元测试](https://www.agilealliance.org/glossary/unit-test/))和设计。

下面是我们在使用 TDD 时可以记住的一些规则:

*   编写一个“单一的”单元测试，描述程序的一个方面
*   运行测试，这应该会失败，因为程序缺少该特性
*   编写“刚好够用”的代码，尽可能简单，以使测试通过
*   “重构”代码，直到它符合[简单性标准](https://www.agilealliance.org/glossary/rules-of-simplicity/)
*   重复，随着时间“积累”单元测试

例如，假设我们正在为一个计算器程序编写 add 功能。

我们知道 add 做什么，所以我们编写一个测试来将两个整数相加。要解决编译问题，请编写将通过测试的函数和代码。

因此，您将最终写出可测试和可追踪的代码。

就像任何其他方面一样，TDD 方法也有缺陷。调整需要时间，开发人员要在测试用例与代码之间来回跳跃。但是，总的来说，TDD 方法提高了代码的设计质量。随着时间的推移，它有助于减少产品中的缺陷。

有了 Camunda，我们就有了实施 TDD 的大好机会。ca munda 工作流程中的每一步都可以被视为一个单元。对于每个单元，可以使用该步骤的预期输入和输出来编写测试用例。在这一点上，如果您运行测试，那么它将会失败，这在 TDD 中是预料之中的。然后，我们可以添加最少的代码来通过测试。

让我们结合上一节中的所有细节，在 Camunda 中实现单元和集成测试用例。*你可以在 GitHub* 上找到 [***代码***](https://github.com/sourabhparsekar/camunda-masala-noodles)

![](img/fe0bc9886fec8623b7e05be2277b93c9.png)

6 月 5 日，springboot 嵌入式 Camunda 引擎的 Mockito 测试

为了使用 JUnit5、Mockito、CamundaMockito，我们必须首先设置我们的 *POM(项目对象模型)文件*。以下是 Camunda 引擎 7.15.0 和 Springboot 2.4.3 的最小依赖项。

> [**Camunda mocks ITO**](https://www.mvndoc.com/c/org.camunda.bpm.extension.mockito/camunda-bpm-mockito/index.html?org/camunda/bpm/extension/mockito/CamundaMockito.html)**javadocs**描述了可以使用它创建的各种 mock。在测试用例中你会经常看到的一个例子是使用类 [DelegateExecutionFake](https://www.mvndoc.com/c/org.camunda.bpm.extension.mockito/camunda-bpm-mockito/org/camunda/bpm/extension/mockito/delegate/DelegateExecutionFake.html) 。

DelegateExecutionFake 可能不支持所有操作，但是如果需要，我们可以扩展它并实现模拟。

它提供了一个假的/模仿的 delegateExecution 来测试简单的委托/侦听器，而没有模仿。

该项目的一个测试展示了假 delegateExecution()的使用。

这里，我们触发了 OrderOnline JavaDelegate 步骤，然后尝试检查变量集的值。

这也可以使用 Mockito 来完成，但是 CamundaMockito 包含了用于模仿的锅炉板代码。

另一个例子可能是假的 VariableScope，当我们从 BPMN 工作流中调用的活动接收响应时，就需要它。

对于更多模拟类，您可以查看 camunda-mockito 的 [javadocs](https://www.mvndoc.com/c/org.camunda.bpm.extension.mockito/camunda-bpm-mockito/index.html?org/camunda/bpm/extension/mockito/CamundaMockito.html)

我们可以将此视为单元测试用例，并使用 TDD 方法来设计测试。另外，这些样本是使用 JUnit 5 Jupiter TestEngine 开发的。

## 现在，我们来谈谈集成测试。一个完整的流量测试！！

我使用 Mockito 为 Camunda 流编写集成测试。想想我们的[卡蒙达面条项目](https://github.com/sourabhparsekar/camunda-masala-noodles)，我们根据现有的配料来煮面。Camunda 告诉我们食谱，然后希望我们告诉我们是否能够烹饪它。

[为了测试整个流程](https://camunda.com/blog/2020/10/testing-entire-process-paths/)，我们可以创建一个模拟实例[***process scenario***](https://github.com/camunda-community-hub/camunda-platform-scenario/blob/master/runner/src/main/java/org/camunda/bpm/scenario/ProcessScenario.java)接口，它扩展了 [*Runnable*](https://github.com/camunda-community-hub/camunda-platform-scenario/blob/b22b8e971b3122675550ce63d306506a84203522/runner/src/main/java/org/camunda/bpm/scenario/run/Runnable.java#L6) 。然后，当我们想要调用一个 Camunda 流时，比如说“CookMasalaVeggiesNoodles”，我们可以简单地通过使用 Scenario.run()和使用 key 来启动这个过程。

```
ProcessScenario scenario = **Mockito.mock(ProcessScenario.class)**;//this will trigger the BPMN Flow to invoke the cooking process
Scenario handler = **Scenario.*run*(scenario).startByKey**("CookMasalaVeggiesNoodles", variables)**.execute()**;
```

让我们检查我们运行的流的一个输出变量值。我们可以从[***BpmnAwareTests***](https://github.com/camunda/camunda-bpm-assert/blob/master/core/src/main/java/org/camunda/bpm/engine/test/assertions/bpmn/BpmnAwareTests.java)*中使用*as sert that()*。*代码如下所示，我们获取流程实例变量并检查其中是否存在键值。

```
// check the final output if it has all the values
*assertThat*(**handler.instance(scenario)).variables().containsEntry**(Constants.*DID_WE_EAT_NOODLES*, true);
```

流程测试的剩余部分是验证 bpmn 流程步骤是否被访问。这可以使用 ***Mockito*** 中的 *verify()* 进行验证。代码看起来像下面这样，它也采用流程场景实例，并检查 id 为“LetsCook”的步骤是否已经完成/开始。

```
// check the order of the desired steps in the success flow // process start  
***verify***(scenario, ***times*(1)**).**hasFinished**("Start_Process"); // exclusive gateway  
***verify***(scenario, ***atMostOnce*()**).**hasCompleted**("CanWeCook"); // cooking service step  
***verify***(scenario, *atMostOnce*()).**hasCompleted**("LetsCook"); // check for the event  
***verify***(scenario, *atMostOnce*()).**hasCompleted**("IsItReady"); // All went well so call eat service  
***verify***(scenario, *atMostOnce*()).**hasCompleted**("LetUsEat");  
***verify***(scenario, ***never*()**).**hasStarted**("OrderOnline"); // end event  
***verify***(scenario, *times*(1)).**hasFinished**("End_Process"); 
```

除此之外，ProcessScenario 可以使用 Mockito 来扩展，以模拟事件或其他步骤。如果我们希望事件网关说“IsItReady”来捕捉消息“IsReady ”,那么我们可以使用下面的代码。或者，您可以传递事件网关可能需要的变量，以供进一步处理。

```
//event based gateway response set for mocking
*when*(**scenario.waitsAtEventBasedGateway("IsItReady")**).thenReturn(gateway -> {
    System.*out*.println("ReceivedEvent");
    Map recVariables = new HashMap<String, Object>();
    **gateway.getEventSubscription("IsReady").receive(recVariables);**
});
```

但是，如果我们希望事件网关失败，比如说超时场景，那么我们可以像下面这样写它。我们基本上声明它是空的，即什么也不做。因此导致立即超时的情况，而不是等待事件。

```
//event based gateway timeout
*when*(**scenario.waitsAtEventBasedGateway("IsItReady")**).thenReturn(gateway -> {
    System.*out*.println("Do Nothing to simulate a timeout");
});
```

对于***process scenario***使用上述概念的测试用例如下，或者您也可以在 [Github](https://github.com/sourabhparsekar/camunda-masala-noodles) 库中查看。

人们只能体会到通过使用 JUnit5、Mockito 和 CamundaMockito 框架在不同的步骤/流程中移动并验证完整的处理来测试整个流程是多么容易。

# 结论

查看我们上面的单元测试和集成测试，我们可以得出结论，Junit5、Mockito 和 Camunda Mockito 将帮助我们:

*   模拟侦听器和委托行为——由于委托和侦听器方法是无效的，它们可以修改流程变量或引发错误。您可以使用这些选项，而不是弄乱 mockito 的 doAnswer()。
*   模拟子进程——子进程能够等待计时器、设置变量、等待消息、发送消息、抛出异常或做任何你想做的事情。
*   自动模仿进程中的所有表达式和委托，而无需显式注册模仿

*希望你已经发现这是一个* ***有趣的*** *使用嵌入式卡蒙达工作流引擎开发的测试烹饪方便面的故事。*

请留下您的反馈或给予一些掌声。如往常一样，对于在设置您的本地环境方面的任何疑问或问题，我们可以通过电子邮件或下面的评论进行联系。

*下次见，快乐烹饪！！*

# 参考资料:

[](https://junit.org/junit5/docs/current/user-guide/) [## JUnit 5 用户指南

### 所有事件:Event [type = STARTED，test descriptor = JupiterEngineDescriptor:[engine:JUnit-Jupiter]，时间戳=…

junit.org](https://junit.org/junit5/docs/current/user-guide/) [](https://rezaid.co.uk/7-reasons-why-software-testing-is-necessary/) [## 为什么软件测试是必要的 7 个基本原因

### 什么是软件测试？软件测试是检查软件应用程序和产品的错误和缺陷的过程

rezaid.co.uk](https://rezaid.co.uk/7-reasons-why-software-testing-is-necessary/) [](/nerd-for-tech/bpmn2-0-camunda-workflow-spring-boot-application-2381f3d42e5f) [## BPMN2.0 - camunda 工作流春季启动应用程序

### 有意思……这种与 spring boot 集成的 DIY camunda 工作流程旨在给你一种 camunda…

medium.com](/nerd-for-tech/bpmn2-0-camunda-workflow-spring-boot-application-2381f3d42e5f) [](https://github.com/sourabhparsekar/camunda-masala-noodles) [## GitHub-sourabhparsekar/cam unda-masala-noodles:这个独立的流程应用程序就是一个例子…

### 这个独立的流程应用程序是使用 Camunda 工作流引擎和 Springboot 的一个例子。至于用例，我们…

github.com](https://github.com/sourabhparsekar/camunda-masala-noodles)  [## camunda-bpm-mockito 4.0.0 API

### 您的浏览器禁用了 JavaScript。本文档旨在使用框架功能查看。如果你看到…

www.mvndoc.com](https://www.mvndoc.com/c/org.camunda.bpm.extension.mockito/camunda-bpm-mockito/index.html?org/camunda/bpm/extension/mockito/CamundaMockito.html) [](https://camunda.com/) [## 工作流和决策自动化平台| Camunda

### BPMN 工作流和 DMN 决策自动化的开源平台。立即下载。

camunda.com](https://camunda.com/) [](https://camunda.com/blog/2020/10/testing-entire-process-paths/) [## 测试整个过程路径- Camunda

### 为什么我应该测试我的过程模型？简短的回答是:从技术角度来看，BPMN 是一个编程…

camunda.com](https://camunda.com/blog/2020/10/testing-entire-process-paths/)