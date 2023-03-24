# Dart/Flutter 中的单元测试流

> 原文：<https://medium.com/nerd-for-tech/unit-testing-streams-in-dart-flutter-6ed72c19f761?source=collection_archive---------0----------------------->

Stream 是 dart 中异步编程的构建块之一。大部分时间我们可能不会直接与它们打交道，但是它们在幕后为许多功能工作，如 Bloc、changeNotifier provider 等。但是有时我们需要创建我们自己的流，所以我们需要学习如何对流进行单元测试。这就是我们在这篇文章中将要看到的。

![](img/eb06e531ad768b4c4508ac1209420905.png)

照片由[拉胡尔·戴伊](https://unsplash.com/@dynamo10?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/streams?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄

因为这篇文章仅仅是关于单元测试流，所以我将只指定为我们的应用程序提供流服务的类。

**我们简单的 app 是做什么的？**

它将根据我们选择的开关以升序或降序运行计数器。

让我们为我们的计数器创建一个接口

## 计数器. dart

```
abstract class MyCounter{
  Stream<int> countStream(int bound);
}
```

这创建了一个契约，实现 MyCounter 的类将**覆盖 countStream()方法**。

我们将有两个接口 Mycounter 的实现

**1。正向计数器**

2.反向计数器

现在我们完成了设置，让我们编写测试

## 颤振 _ 测试相关性

我们不想自己为单元测试添加任何额外的依赖，因为 flutter 已经有了它。确保您的 dev_dependencies 下的 pubspec.yaml **中有`[flutter_test](https://api.flutter.dev/flutter/flutter_test/flutter_test-library.html)` 。**

```
dev_dependencies:
  flutter_test:
    sdk: flutter
  //other dependencies
```

## 创建测试文件

我们通常在`test` 文件夹下编写测试，开发人员在编写测试时通常会遵循一些惯例

1.  你的`test` 文件夹的结构应该和你的`lib`文件夹一样。
2.  所有的测试都必须以`*_test.dart ,`结尾，这不仅仅是一个惯例，它有助于 flutter 将那些 dart 文件识别为测试。

让我们测试一下我们的 MyCounter 实现之一`reverse_counter.dart` 并一个接一个地检查它们

我们已经写了 4 个测试用例，让我们一个一个来

1.  **emitsInOrder()**

> **emitsInOrder() —** 这个方法确保值按照我们指定的顺序发出。

*   ***为什么我们要在上面的代码中使用 emitsInOrder()。*** —我们正在测试从流中发出的值是否按降序排列`3->2->1->0`。
*   ***怎么用？***—emitsInOrder()以`Iterable matchers` 为参数。这就是为什么我们要给这个方法传递一个匹配器列表。
*   我们本可以传递`emitsInOrder([ equals(3), equals(2), equals(1),equals(0)] )` ，但是没有必要，因为我们不想显式地使用 *equals()* 。
*   ***我们什么时候用 emitsDone？*** —如果我们想要验证从流中发出的所有值都按照我们指定的顺序排列，我们使用这个匹配器。
*   如果我们想验证所有的值都是在不考虑顺序的情况下发出的，那么我们可以使用***emissinanyorder(Iterable Matcher)***

> `[*emitsInAnyOrder()*](https://pub.dev/documentation/test_api/latest/test_api/emitsInAnyOrder.html)`工作方式类似于 emitsInOrder()，但是它允许匹配器以任何顺序进行匹配。

*   为了只匹配一个事件，使用**发射(matcher)** —这是最简单的一个，并且只使用它来构建所有其他复杂的 Streammatcher。

## 2.emitsThrough() & emitsDone

这个测试看起来和第一个相似，对吗？但不完全是！

## 发射穿过

> **emit through(matcher)**—这将消耗由【matcher】匹配的所有事件，以及**在**之前的所有事件。如果流发出一个没有匹配[matcher]的 done 事件，这将失败并且不消耗任何事件。

*   ***为什么我们在上面的代码*中使用 emit through(0)？** —它消耗所有值，直到匹配到 0(等于(0))。所以它会消耗 `3->2->1->0`
*   ***当我们使用 emitThough()？*** —当我们不关心与【匹配器】匹配的值之前发出的值时。在上面的测试中，我们不关心值 3，2，1。

## emitsDone

> **emit done—**这个匹配器用于验证流中是否没有剩余的项目。(流已完成)

*   *为什么我们在上面的*代码中使用***emit sdone****？—我们使用它来确保在发出最后一个值“0”后**不会发出其他值。***
*   *当我们使用 emit sone*？—确保流中没有其他要发出的值。

## 3.ExpectAsync()

> **expect async 1(callback)——**这个函数用于包装一个回调(带一个参数)并确保测试框架保持等待，直到这个回调被调用[count]次。如果它没有被调用[count]次或任何匹配器失败(在回调中)，测试将失败。

> 基于回调的参数，expectAsync()有许多变体，如 expectAsync2()、expectAsync3()、…expectAsync6()

*   ***为什么我们在上面的代码中使用 ExpectAsync1(回调)？*** *—* 我们希望我们对 ***的回调恰好运行四次*** *(这是我们在****count****关键字参数中指定的)*，并且我们还希望检查所有发出的值是否都在范围 0 和 3 之间(包括 0 和 3 两个结束值& 3)。
*   ***我们什么时候用 ExpectAsyncN(回调)？—*** *当我们想要验证传递给回调函数的所有值都符合同一个匹配器，并确保回调函数被调用的次数正好为[count]次时。*

## 4.从不发射

> **never emissues(inner matcher)**—此匹配器用于检查流是否没有发出任何与内部匹配器匹配的值。

*   ***为什么我们在上面的代码中使用 never emitters(is negative)？*** *—我们用来检查我们的流是否没有发出任何负值。*
*   ***当我们使用 never emitters(inner matcher)时？—*** *当我们希望确保没有任何异常值(根据我们的业务逻辑)从我们的流中发出时。*

# 测试非确定性行为

所有上述行为都是确定的，

> 确定性-对于给定的输入，输出总是相同的。在我们的例子中，当我们将 3 传递给我们的反向流时，它将总是产生 3->2->1->0。

我们如何测试非确定性行为，flutter 为我们提供了答案。我们可以用`[mayEmit(](https://pub.dev/documentation/test_api/latest/test_api/mayEmit.html)) or [mayEmitMultiple(](https://pub.dev/documentation/test_api/latest/test_api/mayEmitMultiple.html)),` 来描述这些行为。让我们用一个例子来理解它们。

让我们创建 MyCounter 接口的另一个实现，名为 **SurpriseForwardCounter** ，它的工作方式类似于 ForwardCounter，但有时可能会在末尾打印一个随机数(一种不确定的行为)。

这个类和 ForwardCounter 一样，但是我们在最后引入了一些随机性。

但是等等。我们如何测试呢？让我们看看

## 1 . mayemitters()

> may emitters(matcher)—此匹配器将始终成功，如果它与[matcher]匹配，它将消耗流中的值，否则将消耗流中的值。

*   ***为什么我们在上面的代码中使用*may emitters(matcher)*？—*** *由于流可能不总是在最后发出随机值，所以我们使用 mayEmit()来消耗随机值，只要它在测试用例中没有失败。*
*   ***当我们使用*may emitters(matcher)*？—*** *当我们希望仅当一个值与[matcher]匹配时才使用它，如果不匹配，测试用例就会失败。它用于处理非确定性行为。*

> mayEmitMultiple(matcher) —工作方式类似于`mayEmit()`，但是它尽可能多地将事件与匹配器进行匹配。

> **scene Rio**——当你用`**while***(random.nextBool())*.`替换了`**if**(random.nextBool())`的 SurpriseForwardCounter 语句后，计数器会发出类似*0->1->2->3->random value 1->random value 2->…->random value n 的值，直到 random boolean 变为 false。在这些场景中，我们必须使用 mayEmitMultiple()来消耗所有的随机值。*

我已经介绍了关于单元测试流的大部分主题。要了解更多信息，您可以查看测试包的官方文档。

谢谢💖在阅读这篇文章时，如果你发现任何错误，欢迎在评论中提出来📃。如果你喜欢这篇文章，鼓掌👏