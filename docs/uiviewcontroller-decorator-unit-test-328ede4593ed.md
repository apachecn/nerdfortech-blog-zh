# UIViewController + Decorator =单元测试

> 原文：<https://medium.com/nerd-for-tech/uiviewcontroller-decorator-unit-test-328ede4593ed?source=collection_archive---------1----------------------->

![](img/27e8cafff3802392903b0ff521726c5e.png)

你好。

今天我想给大家展示一下 **UIViewController** 中的一些 UI 对象配置是如何从 ***不可测试*** 变成 ***可测试的！***

使用**装饰器**模式有什么好处？

**Decorator** 是结构化软件设计模式，在它的帮助下，我们可以分离 UI 属性的静态设置，如:

*   短信，
*   文本颜色，
*   字体，
*   边框颜色，
*   边框宽度，
*   等等。

当然，我们可以用封装的设置创建特定的类。为了本文的目的，让我们举一个简单的例子:

为 **UIButton** 定义一些类:
**BorderButton** -为**图层设置属性的类. border**
**bold font button**-为**设置字体的类。boldSystemFont(ofSize:)**

现在让我们假设 **BoldFontButton** 继承了 **BorderButton** ，因为我们希望 **BoldFontButton** 有边框。这一次一切都好。但是，现在有一种情况，我们希望 **BoldFontButton** 没有边框。在这种情况下，我们需要:

*   从 **BoldFontButton** 中移除继承，并在所有位置添加配置边框，
*   删除视图中不想要边框的边框，

这两种情况都会造成困难，尤其是第二种情况，因为它模糊了类的意图 **BoldFontButton** 并且我们不能编写测试来检查正确的值。

为了解决这个问题，我们可以使用**装饰**模式，这给了我们将一些 UI 配置转移到外部类的可能性，而且由于像上面例子中的 **UIButton** 这样的 UI 类，不会为继承不需要的行为付出代价。

有两种方法:

1.  为边框和粗体字创建专用的**装饰器**，
2.  为视图创建一个专用的**装饰器**，我们将使用它。

我将在这里提出第二个解决方案。

## **解决方案**

首先，让我们用私有的 **UIButton** 设置声明 **UIViewController** :

现在，让我们为我们的 **ViewController** 声明 **Decorator** ，并将所有设置移到那里:

和 [**注入**](/nerd-for-tech/another-dependency-injection-81371b3434e6) 我们的**装饰器**在到**视图控制器:**

从这一点来看，我们有可能测试我们对 **UIButton** 的配置:

仅此而已。搞定了。

# **结论:**

通过这种方法，我们获得了以下好处:

*   **UIViewController** 不会因为 UI 对象的配置而膨胀，
*   测试用户界面配置的可能性，
*   可读的 UI 配置，
*   UI 配置独立于 **UIViewController。**

感谢您花时间阅读到目前为止。

祝你好运！

**全码**:[https://github.com/Rafal-Prazynski/Decorator.Medium](https://github.com/Rafal-Prazynski/Decorator.Medium)