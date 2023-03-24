# Matt 的花絮# 94——用 TypeScript 编写安全枚举

> 原文：<https://medium.com/nerd-for-tech/matts-tidbits-94-writing-safe-enums-in-typescript-2955706a1e5c?source=collection_archive---------11----------------------->

![](img/38aac84142469fb3e19295382d341213.png)

[上次我写过如何入门 React Native。](/nerd-for-tech/matts-tidbits-93-getting-started-with-react-native-acdec42c67f)本周，我有一个关于用 TypeScript 编写安全枚举的快速花絮！

许多编程语言都支持枚举，但它们在 TypeScript 中的工作方式略有不同。

定义和使用枚举非常简单——可以这样做:

```
enum MyEnum {
  A,
  B,
  C,
}function foo(enum: MyEnum) {}foo(MyEnum.A);
```

真正让我吃惊的是，这样做也是完全正确的:

```
foo(42);
```

为什么？！！？说实话，这对我来说真的一点意义都没有。如果我已经经历了定义一个枚举(并将枚举指定为函数的参数)的所有麻烦，为什么我可以指定任意数字来代替这个枚举，并且它仍然是有效的代码？

<5 minute meditation break/>

好消息是，有一种方法可以保证你的枚举不能被这样使用！

与许多其他编程语言一样，枚举通常表示为整数—第一个枚举值等于 0，第二个等于 1，依此类推。但是，TypeScript 支持另外两种类型的枚举:基于字符串的枚举和异类枚举。

不幸的是，异构枚举和数值枚举有同样的问题。但是，基于字符串的枚举没有！

因此，如果您将枚举定义更改为如下所示:

```
enum MyEnum {
  A = 'A',
  B = 'B',
  C = 'C',
}
```

现在，调用我们的`foo`函数的唯一方法是传递一个有效的枚举值——传递任意数字(或字符串)将产生以下编译错误:

```
Argument of type '42' is not assignable to parameter of type 'MyEnum'.
```

可能有些情况下，您希望能够为枚举指定任意数值，但是我将坚持使用基于字符串的版本！

关于 TypeScript 中枚举的更多信息，请查看官方手册:【https://www.typescriptlang.org/docs/handbook/enums.html

如果有人知道为什么 TypeScript 枚举被设计成这种行为，请在评论中告诉我！

有兴趣和我一起在埃森哲出色的数字产品团队工作吗？我们正在招聘！