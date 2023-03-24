# 依存倒置原则

> 原文：<https://medium.com/nerd-for-tech/the-dependency-inversion-principle-a51209b70efb?source=collection_archive---------4----------------------->

这最后一个原则在设计软件及其职责时有很大关系。大型软件是以模块的形式出现的，正如前面的许多原则所述，开发人员试图避免它们之间的紧密结合，但是，大多数时候情况并非如此。

这个原则规定高级模块不应该依赖于低级模块，两者都应该依赖于代码抽象，我把这看作是一个中间人组件。

抽象不应该依赖于细节，恰恰相反。

高层模块指的是主要的业务功能，当然这个就要看别人了。低级模块处理实现细节

在下面的例子中，一个 SQL 目录和存储库，`ProductCatalog`是高级模块，`SQLProductRepository`类是低级模块

这是代码。

`ProductCatalog`类实例化一个`SQLProductRepository`并调用方法`gettAllProductsNames()`

```
// starting as high-level - depending on the SQlProductRepository
public class ProductCatalog {
  public void listAllProducts() {
    SQLProductRepository sqlProductRepository = new SQLProductRepository();
    sqlProductRepository.getAllProductNames();
  }
}
```

而`SQLProductRepository`类包含了运行 Select 语句并返回产品列表的方法代码。

```
import java.util.Arrays;
import java.util.List;// starting as low-level
public class SQlProductRepository {
  public List<String> getAllProductNames() {
    return Array.asList("item1", "item2");
  }
}
```

另一个常见的例子是在电子商务网站上，支付处理器是高级模块，每个支付解决方案都是低级模块。

这当然是相对的，因为你的代码可以有一个更低的模块，但是有 N 个模块在更高的位置。同样，在这个原则中，最高的依赖于较低的(也可能是最低的)级别。

上面的例子违反了这个原则，因为两个类都依赖于产品。两者都应该依赖于抽象，代码抽象。

为此:

1.  为`ProductCatalog`类创建一个接口，姑且称之为`ProductRepository`
2.  SQLProductRepository 将实现`ProductRepository`接口
3.  `SQLProductRepository`将以某种方式从`SQLProductRepository`对象检索到`ProductCatalog`，即“以某种方式”是一个 ProductFactory，这是一个返回`SQLProductRepository`的单一方法

这都是关于使用抽象。

综上所述，`ProductCatalog`将与`ProductRepository`对话，从而与`SQLProductRepository`对话

细节依赖于抽象，它们根据定义是严格的。

这是代码

这是`ProductCatalog`类。

```
// starting as high-level - depending on the SQlProductRepository
public class ProductCatalog {private ProductRepository productRepository;public ProductCatalog(ProductRepository productRepository) {
    this.productRepository = productRepository;
  }public void listAllProducts() {List<String> getAllProductNames = productRepository.getAllProductNames();// going for the factory method
    // ProductRepository productRepository = ProductFactory.create();
    // productRepository.getAllProductNames();// going for the SQL low-level repository 
    // SQLProductRepository sqlProductRepository = new SQLProductRepository();
    // sqlProductRepository.getAllProductNames();}
}
```

下面是`ProductFactory`类。

```
public class ProductFactory {
  public static ProductRepository create() {
    // instantiate and return a SQlProductRepository
    return new SQLProductRepository();
  }
}
```

下面是`ProductRepository`类。

```
import java.util.List;public interface ProductRepository {
  public List<String> getAllProductNames();
}
```

还有`SQLProductRepository`级。

```
import java.util.Arrays;
import java.util.List;// starting as low-level
public class SQlProductRepository {
  public List<String> getAllProductNames() {
    return Array.asList("item1", "item2");
  }
}
```

应用程序:

```
public class MainApplication {
  public static void main(String[] args) {
    // dependency injection
    ProductRepository productRepository = ProductFactory.create();
    // product catalog
    ProductCatalog productCatalog = new ProductCatalog(productRepository);
    // listing products
    productCatalog.listAllProducts();
  }
}
```

此时，我们将依赖注入到`ProductCatalog`中，而不是`ProductCatalog`担心依赖的实例化。所有需要的独立性都是从外部注入的，而不用担心如何解决依赖性。

**依赖注入**避免了紧耦合，领先一步，完全脱离了一个类，让它的依赖被实例化。

还有另一个术语与这个原则相关，它就是**控制反转(IoC)** 。此时，`ProductCatalog`有一个接受了`productRepository`对象的构造函数。

注入发生在`ProductCatalog`之外，注入仍然发生在`MainApplication`控制流上，这是主线程执行。

但是，如果我们希望所有的注入都在一个单独的线程/上下文中呢？通过使用框架，主控制流与注入保持隔离。

许多框架会负责注入类的必需依赖项，而不是自己去做。

这是一个巨大而完全不同的世界。