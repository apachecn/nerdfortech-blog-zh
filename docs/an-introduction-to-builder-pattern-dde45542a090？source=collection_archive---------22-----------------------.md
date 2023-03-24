# 构建器模式介绍

> 原文：<https://medium.com/nerd-for-tech/an-introduction-to-builder-pattern-dde45542a090?source=collection_archive---------22----------------------->

![](img/586536872b0642b7110cd8b3da01fd5b.png)

构建器模式介绍

构建者模式属于创造性设计模式。

当类中有多个变量时，我们必须通过一次包含多个参数来创建这样的实例。在这种情况下，我们可以手动实现构造函数，但这既困难又耗时。作为解决方案，我们可以使用 telescope 方法创建多个构造函数。但是，这里也由于传递空值等原因。，必须手动处理每个场景(NullPointerExceptions)。我们可以不使用构造函数，而是使用 setters 为类中声明的变量赋值，但是在这里，可变性可能会导致逻辑实现出现一些问题。作为所有这些问题的解决方案，我们可以使用构建器模式。这提供了不变性，在这里，不需要创建构造函数，只需要传递希望包含在实例中的参数。

下面描述了一个使用 java 和 JavaScript 的构建器模式的例子。

假设，银行需要保存每个客户的账户信息。在这里，它保存了每个客户的强制详细信息，包括客户的唯一帐号、客户姓名。和相应的分支名称。除此之外，它还保存了帐户类型和存款金额的信息，作为可选字段。

首先，让我们考虑 java 实现。

这里，我使用了 AccoutTelescope1 和 AccoutTelescope2 类来提供望远镜实现的一些示例。请参考以下代码。

在 Account 类中，我实现了以下内容，将构建器模式属性注入到代码中。

在这里，我实现了一个名为 AccountBuilder 的静态类，当您访问 AccountBuilder 类时，它会用您传递的参数直接为字段 accNo、holder、city 赋值。我已经提供了 setType(String 类型)和 setDeposit(double deposit)方法来将可选参数添加到静态类中(在访问静态类时已经创建)。我再次实现了 build()方法，通过使用该类的现有 accountBuilder 对象实例化它来返回 Account 类型的对象。参考下面的代码。

最后，我在 MainApplication 类中实现了 main 方法，通过 toString()方法显示输出。在这里，我从 AccoutTelescope1 和 AccoutTelescope2 类创建了对象，以演示这些对象如何使用传递的参数。同样，我使用 AccountBuilder 和 Account 类通过使用 setType(String 类型)和 setDeposit(double deposit)方法添加可选字段来打印输出。参考下面的代码。

下面定义了输出。

```
From AccountTelescope1
Account No: 001-234-332
Holder: Hasini
City: Colombo
Account Type: null
Amount of Deposited: 0.0From AccountTelescope2
Account No: 001-500-200
Holder: Hasini
City: Colombo
Account Type: Fixed-Deposit
Amount of Deposited: 0.0Account No: 001-400-165
Holder: Hasini
City: Colombo
Account Type: Current
Amount of Deposited: 5000.0Account No: 001-400-900
Holder: Hasini
City: Colombo
Account Type: Fixed-Deposit
Amount of Deposited: 7000.0
```

现在，让我们考虑相同场景的 JavaScript 实现。

按照上面的代码，我通过向 AccountClass 类传递一些参数，创建了两个实例。

下面定义了输出。

```
AccountClass {
  accNo: '001-234-332',
  holder: 'Hasini',
  city: 'Colombo',
  type: undefined,
  deposit: undefined
}
AccountClass {
  accNo: '001-500-200',
  holder: 'Hasini',
  city: 'Colombo',
  type: 'Fixed-Deposit',
  deposit: undefined
}
```

这是 JavaScript 中实例化类的常用方式，但是因为使用了 builder 模式，我引入了另一个名为 AcountBuilder 的类。参考下面的代码。

这里我也使用了与 java 实现中相同的逻辑来集成构建器模式。

下面定义了接收到的输出。

```
Account1 {
  accNo: '001-400-165',
  holder: 'Hasini',
  city: 'Colombo',
  type: 'Current',
  deposit: 5000
}
Account1 {
  accNo: '001-400-900',
  holder: 'Hasini',
  city: 'Colombo',
  type: 'Fixed-Deposit',
  deposit: 7000
}
```

在 JavaScript 中，我们可以使用另一种技术来实现 builder 模式，而不使用 AcountBuilder 类。在这里，我们可以将可选参数作为 JSON 对象传递。参考下面的代码。

下面定义了接收到的输出。

```
Account1 {
  accNo: '001-400-165',
  holder: 'Hasini',
  city: 'Colombo',
  type: 'Current',
  deposit: 5000
}
Account1 {
  accNo: '001-400-900',
  holder: 'Hasini',
  city: 'Colombo',
  type: 'Fixed-Deposit',
  deposit: 7000
}
```

本文到此结束，我希望您对构建器模式和实现有所了解。

[](https://github.com/HasiniSandunika/Design-Patterns-Java.git) [## GitHub-HasiniSandunika/Design-Patterns-Java:用 Java 实现设计模式。

### 用 Java 实现设计模式。为 HasiniSandunika/Design-Patterns-Java 开发做出贡献，创建…

github.com](https://github.com/HasiniSandunika/Design-Patterns-Java.git) 

**参考文献**

*   https://www.youtube.com/watch?v=ubG_5_QnbkE&list = PLD-myte BG 3 x 86 i3 uyaxwzkfvtuy 2g MDO&index = 4
*   【https://www.javatpoint.com/builder-design-pattern 
*   [https://www . tutorialspoint . com/design _ pattern/builder _ pattern . htm](https://www.tutorialspoint.com/design_pattern/builder_pattern.htm)