# 面向对象编程的四大支柱

> 原文：<https://medium.com/nerd-for-tech/the-four-pillars-of-object-oriented-programming-39efe4e87afc?source=collection_archive---------0----------------------->

![](img/c9a1f2c313da3ec9a0ecea48f69413d7.png)

照片由 [Jorge Salvador](https://unsplash.com/@jsshotz?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

在我最近的一次采访中，有人问我，面向对象编程的四大支柱是什么。面向对象编程(OOP)是一种依赖于类和对象概念的编程范式。它通过使用类似于蓝图的类，将软件程序构造成简单的、可重复使用的代码片段，从而创建称为对象的单个实例。让我们以汽车为例。`Car`类是蓝图。这是一个对象能做什么的模型或标准。对象是类/工作实体的实例。根据`Car`类蓝图制造的汽车是该类的对象。我们可以通过方法修改应用于类的所有实例的类状态。它的一些方法可能是向前行驶、向后倒车和摇下车窗。这些是`Car`类的一个实例可以采取的动作。这种类型的编程遵循 OOP 的四个支柱。OOP 的四大支柱是抽象、封装、继承和多态。

# 抽象

抽象意味着只向用户显示必要的细节。回到我们的汽车例子，我们不需要知道启动引擎的内部机制。我们只想按下按钮或转动钥匙来启动引擎。抽象就是这样，我们只暴露必要的细节，隐藏不相关的信息。代码中的一个例子是通过使用`.toLowerCase()`将字符串转换成小写字母。我们不需要知道语言是如何做到的，我们只需要输入。这可以降低我们代码的复杂性。

# 包装

封装意味着将数据和方法一起包装成一个单元，就像一个类一样。它也是建立在数据隐藏的思想上。一个类的变量将对其他类隐藏，只能通过它们当前类的方法来访问。这主要是为了安全，所以我们不会无意中改变一个对象的属性。我们将属性封装在对象中，通过将属性设置为 private 可以做到这一点。我们可以提供公共的 setter 和 getter 方法来修改类的某些属性。

# 遗产

继承意味着将属性从父类传递给子类。父类是具有所有基本属性和方法的基类，子类除了它们自己的属性或方法之外，还将具有所有与父类相同的属性和方法。它有助于重用、定制和增强现有代码。例如，我们可以有一个包含属性名称、性别和品种的`Dog`类。我们可以把这个父类的一个子类叫做`Puppy`。这个`Puppy`类将继承封装在`Dog`类中的相同属性和方法，并添加了它们自己的相关方法。

# 多态性

多态性意味着子类可以定义自己独特的行为，并且仍然共享其父类的相同方法或行为。让我们想想我们的`Dog`父类和`Puppy`子类。这个`Dog`类有一个`talk()`方法，它将打印出“Woof！”。这个方法将被`Puppy`类继承。然而，这是一只小狗，还没有一个强有力的树皮。我们可以修改`talk()`方法，改为打印出“awoo”。多态的另一个特点是父类不会因为子类的改变而改变。即使我们把`Puppy`的`talk()`改成了“awoo ”,狗还是会“汪汪”叫。多态性允许特定于类的行为和更多可重用的代码。

# 资源

[https://www.educative.io/blog/object-oriented-programming](https://www.educative.io/blog/object-oriented-programming)

[https://www . freecodecamp . org/news/four-post-of-object-oriented-programming/](https://www.freecodecamp.org/news/four-pillars-of-object-oriented-programming/)

[https://www . freecodecamp . org/news/object-oriented-programming-concepts-21bb 035 f 7260/](https://www.freecodecamp.org/news/object-oriented-programming-concepts-21bb035f7260/)

[https://www.youtube.com/watch?v=pTB0EiLXUC8](https://www.youtube.com/watch?v=pTB0EiLXUC8)

[https://www.youtube.com/watch?v=1ONhXmQuWP8](https://www.youtube.com/watch?v=1ONhXmQuWP8)