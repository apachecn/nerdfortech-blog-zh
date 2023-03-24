# Matt 的花絮#95 —在 TypeScript 中定义常数

> 原文：<https://medium.com/nerd-for-tech/matts-tidbits-95-defining-constants-in-typescript-1a74d00b08a1?source=collection_archive---------7----------------------->

![](img/38aac84142469fb3e19295382d341213.png)

[上次我写了如何在 TypeScript 中编写安全的枚举。](/nerd-for-tech/matts-tidbits-94-writing-safe-enums-in-typescript-2955706a1e5c)本周的花絮也和 TypeScript 有关——如何定义常数！

在 TypeScript 中定义一个常量可能非常简单——您只需编写类似于`const SOME_CONSTANT="FOO";`的代码。然而，`const`关键字只有在文件范围或函数内部定义变量时才有效。如果你想把你的常量放在某种命名的对象下(这在其他语言如 Java、C++等中很常见),该怎么办？)

最明显的方法(也是我开始所做的)，就是简单地定义一个对象来包含我的常量，就像这样:

```
const NamedVariables = {
  SOME_CONSTANT_1: "FOO",
  SOME_CONSTANT_2: 42,
  DEFAULT_FONT_WEIGHT: "700",
};
```

这允许我这样使用这个:`NamedVariables.SOME_CONSTANT_1`。太好了！

然而，有一个小问题。这在某些情况下很好，但是在 TypeScript 需要一个属于联合类型的值的情况下，比如指定一个`TextStyle`的`fontWeight`属性，您可能会看到这样的错误:

```
Type 'string' is not assignable to type '"normal" | "bold" | "100" | "200" | "300" | "400" | "500" | "600" | "700" | "800" | "900" | undefined'.
```

如果你像我一样，你可能会为此挠头好一阵子。直到你意识到(做一些狂热的谷歌搜索)对象属性在默认情况下不是恒定的。

上面的`const NamedVariables`中的`const`意味着对该对象的引用是不变的——所以在它已经被定义之后做类似于`NamedVariables = <something else>`的事情是非法的。这很好——我们绝对不希望有人重新分配我们的 constants 对象。但是怎么才能把它的性质定义为常数呢？

我发现的第一件事是使用`[Object.freeze()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/freeze)`——它返回一个属性为只读的对象。所以，如果我们用我们的例子来试一下:

```
const NamedVariables = Object.freeze({
  SOME_CONSTANT_1: "FOO",
  SOME_CONSTANT_2: 42,
  DEFAULT_FONT_WEIGHT: "700",
});
```

不幸的是，如果我们试图在联合类型中使用`DEFAULT_FONT_WEIGHT`，这仍然不起作用。这是为什么呢？据我所知，这是因为 TypeScript 不认为这是一个真正的常量——object . freeze()使用了一些有点疯狂的运行时检查，如果您试图修改它，就会抛出错误。所以，把这个归因于一个限制(也许是暂时的？也许是永久的？)的打字稿。

那么…有什么办法可以实现这个呢？好消息是，是的！

下面是我们如何将一个对象的所有属性都视为常量:

```
const NamedVariables = {
  SOME_CONSTANT_1: "FOO",
  SOME_CONSTANT_2: 42,
  DEFAULT_FONT_WEIGHT: "700",
} **as const**;
```

就是这样——结尾的两个小关键字`[as const](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-3-4.html#const-assertions)` —是一个附加的类型脚本(不是 JavaScript 的一部分),它指示类型检查器对象的所有属性都应该被视为常量。

我强烈建议将它添加到您想要成为常量的所有对象中。让类型系统帮助你！

你对常数的定义不同吗？我很想在评论中听到你的意见！

有兴趣和我一起在埃森哲出色的数字产品团队工作吗？[我们正在招聘！](https://www.intrepid.io/careers)