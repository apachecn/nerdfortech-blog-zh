# 利斯科夫替代原理

> 原文：<https://medium.com/nerd-for-tech/the-liskov-substitution-principle-e35ce940571?source=collection_archive---------8----------------------->

这使事情变得有点复杂，因为这个原则规定对象应该可以用它们的子类型替换，而不影响程序的正确性。

如果你对 OOP 编程有所了解，继承是实现这一原则时的思考方式。

这里打个比方更好理解。假设鸵鸟是一种鸟，但实际上不会飞。通过用一个`fly`方法拥有`Bird`类，所有实例化的鸟都应该会飞，对吗？此时，有了一个扩展了`Bird`的`Ostrich`类，你可以覆盖`Bird`类中的`fly`方法，让它不被实现，这当然是一个设计缺陷，因为你不应该在所有使用`Bird`对象的地方都使用`Ostrich`对象。

利斯科夫通过认可“是-A”思维方式的改变，建立了一种严格的思维模式。

查看另一个例子，以及如何实现。

我们有一个带有`getCabinWidth`方法的通用`Car`类，但也有一个从`Car`本身继承的`RacingCar`类，这个`RacingCar`没有实现`getCabinWidth`方法，而是使用了`getCockpitWidth`方法。

在一个循环交互之后，在多个`Cars`和`RacingCar`的几个实例化之后，您将不得不处理由于不存在而未实现的方法。因此，最好的解决方案是实现一个泛型类，它可以从`Cars`和`RacingCars`中实现，这个新的泛型类，姑且称之为`Vehicle`将有一个名为`getInteriorWidth`的方法。

然后，每个继承的方法将调用它们各自的 cabin 的方法，实例结构将以良好的形式正确地调用每个方法。

如果替代测试失败，这整个原则想要打破等级制度。

这里是另一个方法，这次用代码解释。

因此，我们有一个从另一个名为`InHouseProduct`的类继承而来的`Product`类，它在`Product`类中设置的初始折扣之后提供 1.5 倍的折扣。

```
public class Product {
  protected double discount = 20;
  public double getDiscount() {
    return discount
  }
}
```

和

```
public class InHouseProduct extends Product { 
  public void applyExtraDiscount() {
    discount = discount * 1.5
  }
}
```

这些都是在 ArrayList 中实例化和实现的，当循环时，您需要评估实例是否与`InHouseProduct`匹配，然后调用`applyExtraDiscount`

下面是完整的实现，它叫做`PricingUtils`

```
import java.util.ArrayList;public class PricingUtils { public static void main(String[] args) {
   Product p1 = new Product();
   Product p2 = new Product();
   Product p3 = new InHouseProduct();
 } List<Product> productList = new ArrayList<>();
 productList.add(p1);
 productList.add(p2);
 productList.add(p3); for(Product product: productList) {
  if(product instanceof InHouseProduct) {
   ((InHouseProduct) product).applyExtraDiscount();
  }
    System.out.println(product.getDiscount());
  }
}
```

这都是违反 LSP 的。要解决这个问题，你需要做一些小事。

首先，让我们覆盖`InHouseProduct`上的`getDiscount`方法，并在其中应用额外的折扣。`Product`类保持不变。

```
public class Product {
  protected double discount = 20;
  public double getDiscount() {
    return discount
  }
}public class InHouseProduct extends Product { [@Override](http://twitter.com/Override)
  public double getDiscount() {
      this.applyExtraDiscount();
      return discount;
  } public void applyExtraDiscount() {
    discount = discount * 1.5
  }
}
```

然后，在实现中，您可以直接调用`PricingUtils`类中的`getDiscount`方法，而不是请求实例

```
import java.util.ArrayList;public class PricingUtils { public static void main(String[] args) {
   Product p1 = new Product();
   Product p2 = new Product();
   Product p3 = new InHouseProduct();
  } List<Product> productList = new ArrayList<>();
  productList.add(p1);
  productList.add(p2);
  productList.add(p3); for(Product product: productList) {
    System.out.println(product.getDiscount());
  }
}
```

在我看来，这是最难理解的原理。都是侧重于讲而不是问。我很高兴有很多不同的方式来解释这一点。

我建议你花时间阅读更多的补充材料来阐明这个原则。

接下来让我们转到接口分离原则。