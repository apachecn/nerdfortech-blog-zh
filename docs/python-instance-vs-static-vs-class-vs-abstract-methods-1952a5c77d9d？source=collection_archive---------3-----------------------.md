# Python:实例 vs 静态 vs 类 vs 抽象方法

> 原文：<https://medium.com/nerd-for-tech/python-instance-vs-static-vs-class-vs-abstract-methods-1952a5c77d9d?source=collection_archive---------3----------------------->

Python 为 OOPS 概念提供了广泛的灵活性，但是它被低估了/不为人所知。今天，让我们来看看 Python 脚本中不同方法的用法。

1.  实例方法
2.  静态法
3.  分类方法
4.  抽象方法

![](img/bc0a0b88ab23caf355325a77c3c68333.png)

凯文·Ku 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# **实例方法**

实例方法是我们在 Python 中创建类时经常使用的非常基本和简单的方法。如果我们想打印一个实例变量或实例方法，我们必须创建一个所需类的对象。它们访问唯一的数据，即实例方法将能够访问每个实例唯一的数据和属性。

```
class RECTANGLE:

    def number_of_sides(self):
        print(“I have 2 sides”)
```

1.  实例方法将 self 作为第一个参数。
2.  实例方法不需要 Decorator。

# 静态法

静态方法以某种方式与类相关，但不需要访问任何特定于类的数据。即 **self** ，不一定是该方法的第一个参数，甚至不需要实例化一个实例

静态方法将不能访问 claas 中的任何东西，完全自包含/隔离模式

```
class RECTANGLE:

     def number_of_sides(self):
          print(“I have 2 sides”) @staticmethod
     def info():
         print("inside the Square class")
```

1.  当一个类中有一个与类逻辑相关的**方法，但不一定与任何特定的实例交互时，使用静态方法。**
2.  静态方法是使用 **@staticmethod** 装饰器创建的。

# 分类方法

类方法是绑定到类而不是类的对象的方法。类方法知道它们的类。也就是说，它们不能访问特定的实例数据。在 class 方法中，函数参数的第一个参数是 class，这是一个隐式的第一个参数。

类方法可以访问指定类中的有限方法，它可以修改类的特定细节

```
class RECTANGLE: name = "RECTANGLE"
    def number_of_sides(self):
        print(“I have 2 sides”)

    @classmethod
    def info_class_name(cls):
        print(cls.name)
```

*   类方法是使用 **@classmethod** 装饰器创建的。
*   class 方法可以访问类的状态，因为它接受指向类而不是对象实例的类参数。

# 抽象方法

抽象方法是有声明但没有实现的方法。当我们设计大型功能单元时，我们使用一个抽象类。一个抽象类可以被认为是其他类的蓝图。

在 Python 中，抽象类没有直接的前向实现，我们需要导入 abc(抽象基类)。 ***ABC*** 的工作原理是将基类的方法修饰为抽象，然后将具体的类注册为概念基的实现。

```
from abc import ABC, abstractmethod
class POLYGON(ABC): @abstractmethod
    def number_of_sides(self):
        passclass RECTANGLE(POLYGON):

    def number_of_sides(self):
        print(“I have 2 sides”)
```

1.  抽象方法是使用 **@abstractmethod** 装饰器创建的。
2.  它们重写基类的属性。

我希望这篇文章给出了理解 Python OOPS 清单中不同类型方法的要点，请在评论中分享您的想法。