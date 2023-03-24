# Jasmine 是一个强大的单元测试框架

> 原文：<https://medium.com/nerd-for-tech/javascript-jasmine-a-strong-unit-testing-framework-e13bfb3d1fe7?source=collection_archive---------10----------------------->

![](img/009ed0f60fd25b40030774aa1f74f138.png)

## 介绍

Jasmine 可能是一个强大的 JavaScript 单元测试框架。它为测试同步和异步 JavaScript 代码提供了一个清晰的机制。Jasmine 是一个行为驱动的开发框架，它提供了描述性的测试用例，这些测试用例更关注业务价值，而不是技术细节。因为它是用简单的语言编写的，所以 Jasmine 测试经常被非程序员阅读，并且可以提供一个测试何时成功或失败以及失败背后的基本原理的透明描述。

这是开源的，很容易获得。它在 GitHub 上有近 15000 颗星。它在开发人员中的受欢迎程度意味着它是一个强大的社区，一旦我们陷入停顿，就可以获得文档。Angular 开发者特别青睐 Jasmine，因为 Jasmine 本身就包含在 Angular 项目中。

## 描述

尽管获得自动化测试的优势是有据可查的，但是修复任何新的测试框架通常是令人困惑和耗时的。如果我们正在编写 JavaScript，一种介于面向对象和函数式编程之间的语言，理解检查什么通常也很困难。

Jasmine 是一个用户行为模仿器，它允许我们执行测试用例，就像网站上的用户行为一样。这对测试前端的可见性很有价值。Jasmine 允许通过海关延迟和等待时间来实现用户行为的自动化。

## 使用茉莉的好处

使用 Jasmine 的主要好处包括:

*   由于几乎没有外部依赖性，开销更低
*   将几乎所有需要的工具开箱即用
*   维护前端与后端测试相似
*   编码几乎就像用舌头写字一样
*   广泛的文档，可用于许多框架
*   支持异步测试。
*   马克使用间谍申请测试替身。
*   有助于测试前端代码。[那是通过 Jasmine 的前端扩展 Jasmine-jQuery](https://www.technologiesinindustry4.com/)

![](img/22d9fe760b1f7fe90696e78a500c3cb6.png)

## 如何配置茉莉？

在我们进行设置工作之前，让我们先了解一下修复 Jasmine 的原则。这通常是我们在高层次上要做的事情:

*   运行代码来获得 Jasmine，并手动或通过包管理器下载它
*   初始化茉莉
*   创建一个规格(测试)文件
*   使 ASCII 文本文件可用于等级库文件
*   然后我们就可以测试。我们先排练一下《发现茉莉之路》的小字体。

下载并发现茉莉

当我们使用 Jasmine 的独立版本时，我们下载并手动放置在项目中。如果我们只是想确定它是如何工作的，这种开始方式是很好的。从 Github 上的 Jasmine 发布页面下载一个发行版。

然后创建一个替换目录，并创建一个名为 Jasmine 的替换子目录。

1.  mkdir first-jasmine 项目
2.  CD first-茉莉计划
3.  mkdir 茉莉

## 使用

Jasmine 的目标是易于阅读。一个简单的 hello world 测试看起来像下面的代码，其中 describe()描述了一组测试，它()是一个私有的测试规范。名称“it()”遵循行为驱动开发的思想，是测试名称中的主要单词，应该是一个完整的句子。用法遵循与 RSpec 几乎一样的语法。

下面的代码测试了这个函数

```
function helloWorld() { return 'Hello world!';}and confirms that its output is indeed the text "Hello world!".describe('Hello world', function() { it('says hello', function() { expect(helloWorld()).toEqual('Hello world!'); });});
```

Jasmine 提供了一套高端的内置匹配器。在上面的例子中，toEqual 检查从 helloWorld()函数返回的值和“Hello world！”之间的相等性字符串。这通常等同于在其他测试框架中使用的断言。Jasmine matches 只是返回一个布尔值:如果期望匹配(由于表明测试已经通过)，则为 true 如果期望不匹配，则为 false。一个诚实的做法是将一个期望放在一个私有的 it()测试规范中。

独特的内置匹配器 toThrow 用于确认是否抛出了异常。下面的代码证明抛出了一些异常。

```
describe ('Expect to throw an exception', function () {it('throws some exception', function () {expect( function(){ throw('Some exception'); }).toThrow('Some exception'); });});
```

Jasmine 还有许多其他特性，比如自定义匹配、间谍和对异步规范的支持。

## 茉莉试跑者

Jasmine 带有一个内置的测试跑步器。Jasmine tests 可以通过包含一个 easy SpecRunner.html[9]文件来运行浏览器测试，或者通过使用一个 easy JavaScript 测试运行器工具 Karma，将它用作各种语言(如 Nodejs、Python、Ruby 或(老方法))支持的指令测试运行器。

更多详情请访问:[https://www . technologiesinindustry 4 . com/JavaScript-jasmine-a-strong-unit-testing-framework/](https://www.technologiesinindustry4.com/javascript-jasmine-a-strong-unit-testing-framework/)