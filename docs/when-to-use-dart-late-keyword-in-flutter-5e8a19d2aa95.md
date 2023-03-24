# 什么时候在 Flutter 中使用 Dart late 关键字？

> 原文：<https://medium.com/nerd-for-tech/when-to-use-dart-late-keyword-in-flutter-5e8a19d2aa95?source=collection_archive---------0----------------------->

![](img/1e4949d75ae1ee52aca903bc2499ce6e.png)

我家附近海滩的傍晚天空(个人收藏)

# 什么时候不用晚了

很简单的说，`late`在运行时可能会和`LateInitializationError`不小心炸了的情况下是不好的。

我第一次在 Flutter 应用程序中使用`late`是在我试图避免从 ViewModel 访问远程加载数据的空检查时。

我写了这样的东西:

```
class MyViewModel {
  late SomeData someData;
  bool loaded = false; void load() async {
    someData = await apiService.fetchData();
    loaded = true;
  }} 
```

然后在我的视图中，我检查了`MyViewModel`是否被加载，然后才访问`someData`来构建 UI。

这种方法的问题是它不是[傻瓜证明](/wise-engineering/poka-yoke-in-software-design-e6a0d955a4d8)。查看`MyViewModel`的 API，不清楚在加载完成之前哪些字段/方法可以安全访问，哪些不可以。需要某种自定义机制来保证在使用其余的`MyViewModel`之前调用`load`。

幸运的是，有一个更好的解决方案来处理这种情况。对每个状态使用不同的类，例如加载状态和成功状态。然后[通过单个](https://github.com/felangel/bloc/blob/master/examples/flutter_shopping_cart/lib/catalog/view/catalog_page.dart#L18) `[state](https://github.com/felangel/bloc/blob/master/examples/flutter_shopping_cart/lib/catalog/view/catalog_page.dart#L18)` [属性向视图](https://github.com/felangel/bloc/blob/master/examples/flutter_shopping_cart/lib/catalog/view/catalog_page.dart#L18)公开这些，这样视图就不需要任何空检查。

# 什么时候迟到是好事

`late`有助于解决使用构造函数体实例方法的限制。

例如:

```
class MyBloc {
  final late Subscription<MyData> _streamSubscription; MyBloc({SomeRepository repository}) {
    _streamSubscription = _repository.listen(
      (value) => add(MyEvent(value))
    ); }}
```

注意，在这个例子中，我将`_streamSubscription`设为 final，这很酷，因为我知道这个字段应该只赋值一次。

这里使用`late`的另一种方法是使`_streamSubscription`可空。但这是错误的。我知道它将在构造函数体内被初始化，那么为什么要用空检查来污染代码呢？

有一种情况我可能会得到`LateInitializationError`。在初始化之前访问构造函数体中的`late`域。然而，构造函数体应该很小，因此容易理解。此外，试图在该字段初始化之前访问它是一个逻辑错误。在这种情况下，我可以让运行时错误引起我对这个潜在错误的注意(希望是在运行单元测试时)。

# 懒惰

`late`的另一个用途是延迟加载。

```
class MyClass { final late SomeData _someData = _someComplexCalculation();}
```

在这种情况下，我将推迟运行`_someComplexCalculation`，直到我第一次访问`_someData`。