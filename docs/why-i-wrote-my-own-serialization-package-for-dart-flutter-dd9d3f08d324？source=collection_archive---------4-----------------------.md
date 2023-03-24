# 为什么我要为 Dart/Flutter 编写自己的序列化包。

> 原文：<https://medium.com/nerd-for-tech/why-i-wrote-my-own-serialization-package-for-dart-flutter-dd9d3f08d324?source=collection_archive---------4----------------------->

当你用 Flutter 编写了一个连接到某种 API 的应用程序时，你可能至少遇到过一次 json 序列化。如果不是，那么 json 序列化就是将数据模型转换成有效的 json，并从服务器接收或发送数据，如下例所示:

```
class Person { 
  String name;
  int age;
  Person(this.name, this.age);
}// turn this
var person = Person("Thomas", 23); 
// into this
var json = '{"name": "Thomas", "age": 23}';
```

在我的第一个应用程序中，我实际上是手动完成的…这是一个乏味的过程。对于每个类，您必须定义`fromMap`和`toMap`方法，创建或解析原始的`Map`，然后使用 Dart SDK 的一些内置函数(`jsonDecode`和`jsonEncode`)将它们转换成 json 字符串。

```
class Person {
  ... factory Person.fromMap(Map<String, dynamic> map) {
    return Person(map["name"], map["age"]);
  } Map<String, dynamic> toMap() {
    return {"name": this.name, "age": this.age};
  }
}
```

这使得每一个数据类都膨胀起来，尤其是拥有 10 个或更多属性的类。您会发现大多数时候这些实用函数比实际的类本身占用更多的代码行。此外，如果您希望反序列化是类型安全的，并处理带有有意义的错误消息的无效 json 对象，您将不得不投入更多的工作。

此外，每次更改任何类属性时，都必须调整映射函数。

我通常更喜欢手动编写代码，而不是对所有事情都使用包，但在这种情况下，这只会增加不必要的重复工作，污染您的代码库，并增加维护成本。

![](img/4c2e8cf00c3cdc5531d3d5bd4fbbe878.png)

照片由 [Elisa Ventur](https://unsplash.com/@elisa_ventur?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

## 所以让我们来看看更聪明的方法:

总体思路很简单，您可能已经想到了这一点:让某个算法根据类的属性计算出如何进行序列化。在 Flutter 中，这是通过代码生成(也称为元编程)来完成的，其中一个脚本分析你的代码，并生成与它一起使用的其他代码。

对于 Flutter，或者更一般的 Dart，当然已经有现成的包可以实现 json 序列化。最受欢迎的是

*   [***JSON _ serializable***](https://pub.dev/packages/json_serializable)，1160+喜欢上 pub.dev
*   [***dart _ JSON _ mapper***](https://pub.dev/packages/dart_json_mapper)，pub.dev 上 130+赞

随着一些更小的软件包，这两个共享了大量的用户，第一个甚至是谷歌团队自己制作的最受欢迎的。所以，当看这些数字时，有一些很强的选项可供选择，对吗？

我不会说这些包本身不好，它们实际上在大多数用例中工作得很好，并且做了它们应该做的事情。但是，当我同时使用这两个工具时，总会有一些我不喜欢的或大或小的东西，或者一些不支持的用例——至少没有一些大的变通方法。

总的来说足够了，以至于我有再做一遍的冲动。这是我的第一个飞镖包裹:

[](https://pub.dev/packages/dart_mappable) [## dart _ mappable | Dart 包

### 想象一个映射和序列化包:没有讨厌的样板代码，没有缩小/丑陋的生成文件，没有…

公共开发](https://pub.dev/packages/dart_mappable) 

我试图把两个包中最好的部分放在一起，去掉我不喜欢的部分，并加入一些我自己的魔法来创建这个包。

因此，让我们来谈谈我认为这些软件包没有那么好的地方，以及我如何解决一些核心问题。

# 丑陋的代码

我把这作为第一点，因为有些人可能不认为代码风格是一个重要的因素。谁在乎代码是否好看，对吗？不对。我猜说这种话的人从来没有在大型代码库项目中工作过，或者在大型时间框架中工作过，或者在中型到大型团队中工作过。也许是一个从来没有自己写过一行代码的产品所有者会说的话。

好的代码确实有所不同。丑陋的样板代码糟透了，还有语言黑客或任何奇怪的语法。我不想为了使用序列化包而损害我的代码库，也不想被迫以一种奇怪的方式编写我的类。

对于 *json_serializable* ，这意味着没有样板代码，尤其是没有美元符号或私有混合符号(`_$MyMixin`🤢).

相比之下，我喜欢 *dart_json_mapper* 使用一些简单的调用进行编码和解码，而没有任何样板代码。因此，您会注意到我的包的面向公众的 API 与来自 *dart_json_mapper* 的 API 非常相似。

```
// deserialize json to type Person
Person person = Mapper.fromJson(json);// serialize a person to json
String json = Mapper.toJson(person);
```

# 丑陋的代码

又来了？是的，但是不同。对我来说，代码的生成部分仍然是代码库的一部分。对我来说，重要的是我能够阅读和理解生成的代码，因为我重视任何其他具有良好代码质量的包。我不会接受难看的或缩小的生成代码，在那里我不知道发生了什么。我不得不相信有一个潜在的魔法在起作用，它只是做它应该做的。

相反，我喜欢检查生成的代码并弄清楚它是如何工作的。

对于这一点， *json_serializable* 是明显的赢家，因为它为每个类生成可读的函数。*另一方面，dart_json_mapper* 依赖于 [*reflectable*](https://pub.dev/packages/reflectable) 并使用反射来分析你的类并解析或构建 json。因为这是在运行时完成的，所以您无法检查任何生成的序列化函数，因为没有任何序列化函数。

json_serializable 的另一个缺点是它为每个数据类文件生成一个文件。这将不可避免地将您的项目与一堆文件聚集在一起。您只需要为所有模型生成一个文件。虽然这个文件可能会变得相当大，但它仍然可以快速搜索，不会污染你的文件检查器。

# 编译时检查

我非常喜欢强类型语言，因为你把代码中部分可能的错误移到了编译时。任何已经在编译时被捕获的 bug 都是很棒的，显然比运行时异常好得多。

代码生成有很大的可能性来编写大量的，但是非常强类型的代码，因为你不必关心编写简洁的代码。如前所述， *dart_json_mapper* 在运行时使用反射，因此完全错过了这个机会。

# 实施开销

使用 *reflectable* ，你实际上只能在一个项目中使用一次，即使它在一个包中。这是因为*可反射*静态初始化，因此任何第二次初始化都会覆盖第一次初始化。

另外 *reflectable* 是一个相当大的图书馆。代码生成已经给了你所有的分析能力和你需要的类的元信息， *dart_json_mapper* 完全忽略它们，把问题转移到 *reflectable* 上。

# 怎么样

有一些特性是**dart _ mapable**所独有的。其中之一是序列化过程中内置的灵活性和可扩展性。

您的模型可能会有许多自定义用例。大多数情况下，json 键到类属性的一对一映射会很好，但在少数情况下，您的序列化包应该会让您有可能做出改变。这就是为什么**dart _ mapable**带有[定制钩子](https://pub.dev/packages/dart_mappable#encoding--decoding-hooks)的原因，你可以用它来控制和修改(反)序列化过程的每一个部分。

另一个热门话题是泛型。这真的很难。当使用自定义泛型类型时，反序列化带来了巨大的挑战。这是因为实际上不可能从复合类型中向下钻取泛型类型信息。更具体地说，当你有一个类型`A<B>`时，没有办法到达类型`B`。所以当你有一个嵌套的 json 对象时，你知道把外部对象解码成什么，但是当使用像`fromJson<A<B>>(jsonString)`这样的通用解码函数时，你不知道把内部对象解码成什么。Stackoverflow 上有几个关于这个问题的问题，通常答案是“你不能”。

因此，每个包都有自己的技巧或解决方法来处理泛型。 *dart_json_mapper* 使用 type_adapters，在很大程度上你必须为每个组合类型手动定义，因此不再是真正的“通用”了。 *json_serializable* 在其文档中没有涉及泛型，但是在 Github 和 Stackoverflow 上有一些问题，以及[本文](https://wamae.medium.com/generics-and-json-serialization-in-flutter-a8d335840d7b)。总的来说，在一些简单的情况下这是可能的，但是你必须手动处理内部类型。不完全是我所说的“就这么简单”。

与其他的不同，**dart _ mapable**允许您自然地使用现成的泛型类，不需要任何额外的工作。是的，你只需按一下`A<B> object = Mapper.fromJson(jsonString)`，它就会正常工作。我知道我刚才说这是不可能的，这是从语言的角度来看，但是通过代码生成的能力和一些技巧，我可以让它工作(这是我很自豪的💪).如果人们感兴趣，我可能会写一篇单独的文章，更深入地解释这是如何工作的。

# 包裹

还有一些我没有在本文中介绍的特性。我在软件包自述文件中记录了所有这些，因此如果您正在寻找更多的技术解释和使用指南，请前往[软件包发布和开发页面](https://pub.dev/packages/dart_mappable)。

总的来说，这个包还在开发中，根据您的使用情况，可能还会有一些问题。然而，我在我自己的一些项目以及我目前工作的公司中使用了它，到目前为止，它表现得相当好。

如果您对这个包有任何进一步的问题或反馈，请随时在 Github 上添加问题。欢迎任何反馈😃。