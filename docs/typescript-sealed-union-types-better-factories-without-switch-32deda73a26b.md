# Typescript 密封联合类型—没有开关的更好的工厂

> 原文：<https://medium.com/nerd-for-tech/typescript-sealed-union-types-better-factories-without-switch-32deda73a26b?source=collection_archive---------0----------------------->

![](img/d3bf9a5b2e3a430f06c0a36922fc24cf.png)

安东尼奥·巴蒂尼奇从 [Pexels](https://www.pexels.com/photo/black-screen-with-code-4164418/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 拍摄的照片

如果你曾经偶然发现一个奇怪的编程世界，那么你可能听说过[工厂方法模式](https://en.wikipedia.org/wiki/Factory_method_pattern)。它真的很有用，可以让你的生活更轻松(不要相信我，读一读它是关于什么的，试着理解它的利弊)。

如果您将它与 typescript 中的[字符串文字类型](https://www.typescriptlang.org/docs/handbook/literal-types.html)和 [javascript 对象](https://www.w3schools.com/js/js_objects.asp)相结合，您将能够立即创造出惊人的东西。

让我们看一个例子:

尽管这个例子有它的缺点，但它可以向您展示如何利用前面提到的概念创建有区别的联合。我们使用`_tag`属性来区分不同的工厂。它具有字符串文字类型的联合类型，因此被定义为可能的字符串值的有限集合。通过这种方式，typescript 编译器知道允许的值，现代 IDE(或编辑器)可以基于这一点很容易地帮助您。

之后我们定义了所有工厂都必须遵守的接口。这样我们就可以充分利用多态的力量，面向接口而不是实现编程。

具体工厂(`Factory1`、`Factor2`、`Factory3`)以这种方式实现，只是作为示例，实现完全取决于业务逻辑。

有趣的部分来了。我们将抛弃`switch`语句，转而支持 javascript 对象(或者本质上是 map，如果你想在不同的编程语言中使用这种策略的话)。那么这个物体是由什么组成的呢？如果您已经阅读了这篇文章，那么键实际上是用于`_tag`字段的 union 中的字符串文字，而值是应该从有区别的 union 中返回三个工厂之一的函数。为了实现这个类型`FactoryT`已经被定义为一个函数返回类型。另外，即使我们可以用前面提到的键和值创建一个对象，我还是决定用一个函数，以防万一你想传递一些创建将被返回的对象所需的选项。

最后有一个用法的例子，当然这是人为的，这就是为什么它写着`clientFactories[factoryType || "default"]`，因为通常你可能不知道你是否有工厂类型以及它是否是正确的，所以最安全的是退回到`"default"`。

您可以使用泛型、不同的参数类型等使这变得更复杂。或者用字符串类型和类型映射来简化它。由你决定:)

如果您对如何改进这一点有任何意见，请写下来。另外，如果你喜欢这篇文章，请鼓掌并跟我来。

# 文学

1.  [https://refactoring . guru/design-patterns/factory-method/typescript/example](https://refactoring.guru/design-patterns/factory-method/typescript/example)
2.  [https://www . logic big . com/tutorials/misc/typescript/discribed-unions . html](https://www.logicbig.com/tutorials/misc/typescript/discriminated-unions.html)
3.  [https://mariusschulz . com/blog/string-literal-types-in-typescript](https://mariusschulz.com/blog/string-literal-types-in-typescript)
4.  [https://medium . com/codex/factory-pattern-type-script-implementation-with-type-map-ea 422 f 38862](/codex/factory-pattern-type-script-implementation-with-type-map-ea422f38862)