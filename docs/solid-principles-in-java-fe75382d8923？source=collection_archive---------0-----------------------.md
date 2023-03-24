# Java 中的坚实原理

> 原文：<https://medium.com/nerd-for-tech/solid-principles-in-java-fe75382d8923?source=collection_archive---------0----------------------->

![](img/c7b926c0c7b7c8749be1d4cd602b5416.png)

在软件工程中， **SOLID** 是 5 个[设计原则](https://principles.design/)的首字母缩写，旨在使软件设计更容易理解、灵活、健壮和可维护。采用这些实践也有助于避免代码味道。

这 5 个坚实的原则是:

*   单一责任原则
*   **O** —开闭原理
*   **L** —利斯科夫替代原理
*   **I** —界面偏析原理
*   **D** —依存倒置原则

尽管这些坚实的原则适用于任何编程语言，但在下一节中，我将使用专门用 JAVA 编写的例子来解释它们。

# 单一责任原则

这个原则规定"**一个类应该只有一个改变**的理由"，这意味着每个类应该有一个单一的责任。

```
public class Vehicle {
    public void details() {}
    public double price() {}
    public void addNewVehicle() {}
}
```

在这里，该类有多种理由进行更改，因为`Vehicle`类有三个独立的职责:打印细节、打印价格和向数据库添加新的车辆。

为了实现单一责任原则的目标，我们应该实现一个只执行单一功能的单独的类。

```
public class VehicleDetails {
    public void details() {}
}public class CalculateVehiclePrice {
    public double price() {}
}public class AddVehicle {
    public void addNewVehicle() {}
}
```

# 开闭原理

这个原则声明"**软件实体(类等。)应打开以进行扩展，但关闭以进行修改**。这意味着不用修改类中的任何东西，它应该是可扩展的。

让我们通过一个通知服务的例子来理解这个原理:

```
public class NotificationService{
    public void sendNotification(String medium) {
         if (medium.equals("email")) {}
    }
}
```

这里，如果你想引入一个新的媒介而不是电子邮件，比如说发送一个通知到一个移动号码，那么你需要修改 NotificationService 类中的源代码。

因此，为了克服这一点，你需要以这样一种方式设计你的代码，每个人都可以通过扩展它来重用你的功能，如果他们需要任何定制，他们可以扩展类并在其上添加他们的功能。

您可以创建一个新界面，如下所示:

```
public interface NotificationService {
    public void sendNotification(String medium);
}
```

电子邮件通知:

```
public class EmailNotification implements NotificationService {
    public void sendNotification(String medium){
        // write Logic using for sending email
    }
}
```

手机通知:

```
public class MobileNotification implements NotificationService {
    public void sendNotification(String medium){
        // write Logic using for sending notification via mobile
    }
}
```

# 利斯科夫替代原理

这个原则声明“**派生类必须能够替换它们的基类**”。换句话说，如果类 A 是类 B 的子类，那么我们应该能够在不中断程序当前行为的情况下用 A 替换 B。

考虑一个从 Rectangle 基类派生的 square 类的示例:

```
public class Rectangle {
    private double height;
    private double width;
    public void setHeight(double h) { height = h; }
    public void setWidht(double w) { width = w; }
}public class Square extends Rectangle {
    public void setHeight(double h) {
        super.setHeight(h);
        super.setWidth(h);
    }
    public void setWidth(double w) {
        super.setHeight(w);
        super.setWidth(w);
    }
}
```

在`Rectangle`类中，设置宽度和高度似乎完全合乎逻辑。然而，在 square 类中，SetWidth()和 SetHeight()没有意义，因为设置一个会改变另一个来匹配它。

在这种情况下，Square 没有通过 Liskov 替换测试，因为您不能用它的派生类 Square 替换 Rectangle 基类。Square 类有额外的约束，即高度和宽度必须相同。因此，用 Square 类替换 Rectangle 可能会导致意外的行为。

# 界面分离原理

这个原则适用于接口，它类似于单一责任原则。它声明"**客户永远不应该被强迫实现它不使用的接口，或者客户不应该被强迫依赖他们不使用的方法。**”。

让我们以车辆接口为例来理解这一点:

```
public interface Vehicle {
    public void drive();
    public void stop();
    public void refuel();
    public void openDoors();
}
```

假设我们现在使用这个 Vehicle 接口创建了一个`Bike`类

```
public class Bike implements Vehicle {
    public void drive() {}
    public void stop() {}
    public void refuel() {} // Can not be implemented
    public void openDoors() {}
}
```

由于自行车没有门，我们不能实现最后一个功能。

要解决这个问题，建议将接口分解成多个小接口，这样就不会强制任何类实现它不需要的任何接口或方法。

```
public interface Vehicle {
    public void drive();
    public void stop();
    public void refuel();
}public interface Doors{
    public void openDoors();
}
```

创建两个类— `Car`和`Bike`

```
public class Bike implements Vehicle {
    public void drive() {}
    public void stop() {}
    public void refuel() {}
}public class Car implements Vehicle, Door {
    public void drive() {}
    public void stop() {}
    public void refuel() {}
    public void openDoors() {}
}
```

# 从属倒置原则

依赖倒置原则(DIP)声明"**实体必须依赖于抽象(抽象类和接口)，而不是依赖于具体的实现(类)。此外，高级模块不能依赖于低级模块，但两者都应该依赖于抽象**。

假设有一家书店可以让顾客把他们喜欢的书放在特定的书架上。

为了实现这个功能，我们创建了一个`Book`类和一个`Shelf`类。Book 类将允许用户查看他们存放在书架上的每本书的评论。书架课会让他们在书架上增加一本书。举个例子，

```
public class Book {
    void seeReviews() {}
}public class Shelf {
     Book book;
     void addBook(Book book) {}
}
```

一切看起来都很好，但是由于高级的`Shelf`类依赖于低级的`Book`，上面的代码违反了依赖倒置原则。当商店要求我们让顾客也能在货架上添加他们自己的评论时，这一点就变得很明显了。为了满足需求，我们创建了一个新的`UserReview`类:

```
public class UserReview{
     void seeReviews() {}
}
```

现在，我们应该修改 Shelf 类，使它也可以接受用户评论。然而，这显然也会破坏开/关原则。

解决方案是为底层类(Book 和 UserReview)创建一个抽象层。我们将通过引入产品接口来做到这一点，两个类都将实现它。例如，下面的代码演示了这个概念。

```
public interface Product {
    void seeReviews();
}public class Book implements Product {
    public void seeReviews() {}
}public class UserReview implements Product {
    public void seeReviews() {}
}
```

现在，书架可以引用产品接口而不是它的实现(Book 和 UserReview)。重构后的代码还允许我们在以后引入新的产品类型(例如，杂志),顾客也可以把它们放在货架上。

```
public class Shelf {
    Product product;
    void addProduct(Product product) {}
    void customizeShelf() {}
}
```

# 结论

在本文中，您了解了可靠代码的五个原则。坚持坚实的原则可以使你的项目，可扩展，容易修改，测试良好，更少的复杂性。

*原载于*[*https://apoorvtyagi . tech*](https://apoorvtyagi.tech/solid-principles-in-java)*。*