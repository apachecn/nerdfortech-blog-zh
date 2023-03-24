# 开闭原则

> 原文：<https://medium.com/nerd-for-tech/the-open-closed-principle-a4937889ca2d?source=collection_archive---------5----------------------->

这是符合坚实原则的第二条规则。

这个原则规定，软件组件应该对修改关闭，但对扩展开放。在某种程度上，这变得很奇怪，但很容易理解。

**Closed** 部分规定，无论何时向软件组件添加新功能，您都不必修改现有功能。

另一方面，**打开**部分设置了软件组件应该是可扩展的，以向其添加新功能或新行为。

如果你熟悉 Java，这个原理实现了很多接口。

事情是这样的。

让我们想象一家保险公司向客户销售不同类型的计划。我们有一个`VehicleInsurance`和一个`HealthInsurance`，它们实现了一个`isLoyalCustomer`方法来检查客户是否可以得到折扣。

代码如下:

```
public class HealthInsurance {
  public boolean isLoyalCustomer() {
    return true;
  }
}
```

这是另一个类:

```
public class VehicleInsurance {
  public boolean isLoyalCustomer() {
    return true;
  }
}
```

在这一点上，没有什么问题，只是两个类与一个重复的方法，没有什么真正的悲剧。

另一个类叫做`InsurancePremiumDiscount`，它对客户购买的每一种保险实现了一个`calculatePremiumDiscountPercent`，以便计算折扣。

```
public class InsurancePremiumDiscount  {public int calculatePremiumDiscountPercent(HealthInsurance customer) {
  if(customer.isLoyalCustomer()) {
    return 20;
  }
  return 0;
}public int calculatePremiumDiscountPercent(VehicleInsurance customer) {
    if(customer.isLoyalCustomer()) {
      return 20;
    }
    return 0;
  }
}
```

如您所见，`isLoyalCustomer`方法是在一个客户实例之后调用的，当然，`calculatePremiumDiscount`是为公司提供的每种保险类型复制的。

这种方法一点都不实用，第一个初始集里有一个重复的方法，每个险种里面都有，所以重复的保费计算方法在`InsurancePremiumDiscount`里面

我们可以使用 **OCP** 来解决这个问题，但是这里有一些要点。

首先，这有利于维护，因为添加新功能很容易，这可以节省编码时间，因为也需要解耦，这也适用于 **SRP** 原则，但目标是通过编写更少的代码来实现。

还记得 Java 接口吗？好吧，通过使用接口，您将动态地为创建保险计划的任何人实现客户概要。因此，您将删除任何形式的重复。

接口结构在所有使用它们的类中强制实现，并且只在它们的定义中声明。

下面是应用了 **OCP** 后的示例。

这是新的`CustomerInterface`这个声明了`isLoyalCustomer`

```
public interface CustomerProfile {
  public boolean isLoyalCustomer();
}
```

现在，我们可以根据需要选择所有的保险类别。

下面是带有`CustomerProfile`实现的`HealthInsurance`

```
public class HealthInsurance implements CustomerProfile {
  public boolean isLoyalCustomer() {
    return true;
  }
}
```

于是有了`VehicleInsurance`和新的`HomeInsurance`级。

```
public class VehicleInsurance implements CustomerProfile {
  public boolean isLoyalCustomer() {
    return true;
  }
}public class HomeInsurance implements CustomerProfile {
  public boolean isLoyalCustomer() {
    return true;
  }
}
```

更酷的是`InsurancePremiumDiscount`类的新实现，这是因为使用了`CustomerProfile`接口作为`calculatePremiumDiscountPercent`的构造函数，对复制造成了一点影响。

这是它现在的样子。

```
public class InsurancePremiumDiscountCalculator { public int calculatePremiumDiscountPercent(CustomerProfile customer) {
  if(customer.isLoyalCustomer()) {
    return 20;
  }
    return 0;
  }
}
```

从各方面来看这都很棒。但是，事实是，当代码增长时，简单性会使您的整体设计变得复杂。

就想法而言，这个原则很好，但有时不值得你去思考。

让我们在下一篇文章中继续讨论利斯科夫替代原理。