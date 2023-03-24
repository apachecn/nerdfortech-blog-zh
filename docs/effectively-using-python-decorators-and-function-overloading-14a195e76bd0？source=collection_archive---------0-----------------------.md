# 有效地使用 python 装饰器和函数重载

> 原文：<https://medium.com/nerd-for-tech/effectively-using-python-decorators-and-function-overloading-14a195e76bd0?source=collection_archive---------0----------------------->

![](img/14523c43e2f38a29dae4c3a7f32644e0.png)

Decorators 是 python 中使用最多的特性之一。在这个博客中，我们将讨论，

*   装修工
*   python 如何执行 decorators
*   定义装饰者的各种方式
*   singledispatch 是一个装饰器，用于 python 标准库中定义的函数重载。

在这篇博客中，我们将通过烤披萨的例子来理解上面提到的概念。

# 装饰基础

一个[装饰器](https://en.wikipedia.org/wiki/Python_syntax_and_semantics#Decorators)是一个可调用的，它作为一个包装器工作，把一个函数作为一个参数，并用被装饰的函数执行一些处理。

抽样输出

```
Just adding more cheese here...
Just baking pizza here...
And Done!
```

在上面的代码中，我们嵌套了函数*添加 _ 奶酪*和*内部 _ 函数*。函数 *adding_cheese* 以函数为自变量，返回 *inner_function* 。在 *inner_function* 中，在调用作为参数传递给 *adding_cheese* 的函数前后执行打印语句。在函数 *baking_pizza* 之上添加*@ add _ cheese*，用*add _ cheese*装饰 *baking_pizza* ，换句话说*add _ cheese*是装饰函数 *baking_pizza* 是装饰函数*。*

正如您在示例输出中看到的，我们看到了在 *baking_pizza* 和 *inner_function* 中定义的打印语句。

![](img/7cf14421166773329d13dc7725ed7952.png)

# 装饰者执行

看看 python 如何执行 decorators 是很有趣的。下面是一个演示程序。

抽样输出

```
Adding topping <function adding_olives at 0x10c9cad08>...
Adding topping <function adding_onions at 0x10c9cad90>...
Adding topping <function adding_tomatoes at 0x10c9cae18>...
Stated adding toppings
Toppings added so far [<function adding_olives at 0x10c9cad08>, <function adding_onions at 0x10c9cad90>, <function adding_tomatoes at 0x10c9cae18>]
Added Olives...
Added Onions...
Added tomatoes...
This is getting out of hand now. GET OUT!!!
```

在上面的代码中，decorator*adding _ toppings*向 list *toppings* 添加装饰函数。在主函数中，我们打印列表*浇头*的内容，并调用函数*添加橄榄*、*添加洋葱*、*添加番茄*和*添加苹果*。函数 *adding_toppings* 打印哪个函数被修饰，然后添加到列表 *toppings* 中。

样本输出显示了一个有趣的结果。Decorators 在模块加载后立即运行，而被修饰的函数在被调用时运行。在示例输出中，前三行用于*添加浇头*装饰器执行，它被添加到函数*添加橄榄*、*添加洋葱*和*添加番茄*的顶部。有趣的是看到 *toppings* 没有列出函数 *adding_apple* ，那是因为函数 *adding_apple* 没有用 *@adding_toppings* 修饰。

# 堆叠装饰者

我们可以将多个装饰器堆叠到一个函数中。让我们回忆一下用来介绍装饰者的在比萨饼中加入奶酪的例子。在函数 *baking_pizza* 中，让我们添加两个堆叠的装饰器，而不是一个。

抽样输出

```
Just adding more cheese here...
Just adding some more cheese here...
Just baking pizza here...
And Done!
```

当两个或更多装饰器被添加到一个函数中时，首先调用外部装饰器，然后调用其他堆叠的装饰器。从示例输出中我们可以看到，首先调用外部装饰器*add _ cheese*，然后调用第二个装饰器 add _ more _ cheese。

# 函数参数和返回值

让我们考虑接受函数参数并返回一个值的修饰函数的情况。

抽样输出

```
Just adding more cheese here...
Blah Blah baking pizza here...
And done!
```

在上面的代码示例中，函数 *baking_pizza* 将 *name* 作为参数，并返回一个字符串。为了处理函数参数，我们使用 *args* 和 *kwargs* 来接受 *input_function* 中的输入参数。同样， *inner_function* 在函数执行返回由函数 *baking_pizza* 返回的值后，返回变量 *ret_val* 。

# 班级中的装饰者

让我们考虑一下在一个类中定义 decorator 的时候，

抽样输出

```
Just adding more cheese here...
Blah Blah baking pizza here...
And Done!
```

在上面的代码示例中，类 *pizza* 有一个装饰函数*add _ cheese*，它装饰函数 *baking_pizza* 。在这种情况下，我们使用 *@pizza.adding_cheese* 进行装饰。

# 参数化装饰器

参数为程序员提供了更大的灵活性，可以根据条件有效地使用装饰器。

抽样输出

```
Baking supreme_veggie_pizza pizza with thin crust
Just baking pizza here...
And Done!
Baking farmhouse_pizza pizza with normal crust
Just baking pizza here...
And Done!
```

在上面的示例代码中，decorator *pizza_crust* 将 *crust* 作为参数。在函数*中，supreme_veggie_pizza* 装饰器被设置为*外壳*‘瘦’，而在函数*中，农舍 _pizza* 装饰器被不带参数地调用，因此装饰器选择*外壳*的默认值，即‘正常’。

# 装饰演示

有了关于装饰者的所有基本信息，让我们试着实现一个函数，它将根据比萨饼订单计算用户可以得到的最大折扣。这将说明我们如何在程序中利用装饰器。

抽样输出

```
discount = 0.2
```

在上面的示例代码中， *order_dict* 模仿订购比萨饼的用户的输入。函数 *seasonal_discount* 、 *buy_2_pizza* 和 *new_customer* 读取 *order_dict* 以确定折扣是否适用。在任何时候，如果我们必须删除一个类别的折扣，那么只需从函数中删除 decorator 即可。因此，通过最小的改变，我们可以在程序执行中增加或删除一个功能。

# 使用 Singledispatch 的函数重载

Python 在标准库中定义了一些装饰器，如[属性](https://docs.python.org/3/howto/descriptor.html#properties)、[静态方法](https://docs.python.org/3/library/functions.html#staticmethod)、[类方法](https://docs.python.org/3/library/functions.html#classmethod)、 [lru_cache](https://docs.python.org/3/library/functools.html) 、 [singledispatch](https://docs.python.org/3/library/functools.html#functools.singledispatch) 等。在本节中，我们将以 singledispatch 为例，它用于覆盖函数。下面是一个示例程序，其中我们覆盖了 *get_cost* 函数来返回不同外壳类型的成本。

抽样输出

```
thin_crust_cost = 25
```

在上面的示例代码中，我们定义了三个数据类 *ThinCrust* 、 *NormalCrust* 和 *ThickCrust* 。函数 *get_cost* 用 singledispatch 修饰，被用 *@get_cost.register* 修饰的函数覆盖。根据 *get_cost* 函数调用中的对象类型，返回的 cost 会有所不同。这是避免多个 if/elseif/else 条件的好方法。

我希望这对你有用。我们可以在理解 python 中的工厂类模式时扩展这些知识，我将在以后的博客中介绍。感谢您阅读博客。