# 用 get_it — Flutter 实现服务定位器设计模式

> 原文：<https://medium.com/nerd-for-tech/implement-service-locator-design-pattern-with-get-it-flutter-5e50671bbbcb?source=collection_archive---------0----------------------->

在本文中，我们将看到如何在 flutter 中使用 get_it 包实现服务定位器设计模式。

![](img/98461a40cf6aa11ea9cb6f69714a65a9.png)

马库斯·斯皮斯克在 [Unsplash](https://unsplash.com/s/photos/dependency-injection?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

# 服务定位器设计模式

在服务定位器设计模式中，我们维护一个中央注册中心，当服务消费者或服务客户请求服务时，它提供服务的实例。它与依赖注入非常相似，但它们并不相同。

如果你不懂设计模式，相信看完这篇文章你就懂了。

# 为什么我们需要这些？

我们不做新的东西，除非我们以前做的方式有错误。让我们来看看为什么我们要选择像服务定位器或依赖注入这样的设计模式。

让我们从一个例子开始，

我们想为我们的应用程序创建一个定制的记录器，让我们为它创建一个接口。

```
abstract class Logger{
  void log(String msg);
}
```

假设我们想要创建两个实现，一个简单的控制台记录器和一个文件记录器。

*   控制台日志

*   文件记录器

如果你想在你的 flutter 应用中使用它，你会怎么做？

1.  创建所需记录器的实例。
2.  只需将要记录的消息传递给 log()函数。

```
void main(){
  Logger myLogger = ConsoleLogger();
  //Logger myLogger = FileLogger();
  myLogger.log('file accessed');
}
```

方法有什么问题？

*   调用方法/类，在本例中是 main()，必须创建 Logger 类的实例，这是不好的。如果您想在多个类中拥有实例，该怎么办？你必须自己到处实例化这个方法。
*   你可能会说，好吧，我将声明它为单例。问题解决了！不，这还不够！因为很难对单例进行单元测试，很多时候你都不想要单例。

我们不可能模仿 ConsoleLogger 类。

*   您可以通过继承的小部件或使用提供者来提供对象，但这会变得非常麻烦，而且如果不传递 buildContext，您就无法访问 UI 外部的对象。

所以我们如何克服下面的问题，

1.  对客户端类隐藏实例创建。
2.  使单元测试变得容易。
3.  避免像 provider 或 Redux 那样用特殊的小部件来访问数据，使 UI 树变得杂乱。

**我们可以使用 get_it 包**

## get_it 包

将 get_it 添加到您的 pubspec.yaml 文件中，

```
dependencies:
  **get_it: ^7.1.4**
```

正如我之前提到的

在服务定位器设计模式中，我们维护一个**中央注册中心**，它提供不同接口的实现。它非常类似于依赖注入，但它们并不相同。

我们将在一个方法下注册所有实例，并在调用 runApp()之前从 main()调用实例，以确保在应用程序启动之前注册每个实例，

**定位器.镖**

```
import 'package:get_it/get_it.dart';**final serviceLocator = GetIt.instance**; // GetIt.I is also valid
void **setUp()**{
 serviceLocator.registerLazySingleton<Logger>(
               () => ConsoleLogger());
 serviceLocator.registerSingleton<Model>(()=> MyModel());
 // register more instances
}
```

我们正在全局声明 serviceLocator 实例，以便我们可以在应用程序中的任何地方访问它。这里的**中央注册中心是 GetIt.instance，**，它保存所有注册的实例。

setUp()(或者您选择的任何名称)方法是我们注册所有实例的地方。

```
void main(){
     **setUp();** //call this method before runApp()
     runApp(MyApp());
}
```

要获得注册的实例，请使用 get <t>()方法</t>

```
Logger logger = serviceLocator.**get<Logger>()**; 
```

get_it 提供了各种注册实例的方法。我们一个一个来看。

## 不同的注册方式

## 1.registerSingleton(T 实例)

我们使用这个方法注册一个单例实例(创建一个实例，每当我们调用 get <t>())时，返回同一个实例)。</t>

```
serviceLocator.**registerSingleton**<Model>(MyModel());
```

## 2 . registerlazysingleton(factory func<t>factory func)</t>

该方法也用于注册单例实例，但它是延迟注册的，对象仅在第一次调用 get <t>()时实例化，而 registerSingleton 方法在调用 setUp()方法时实例化实例，并使实例随时可用。</t>

```
serviceLocator.**registerLazySingleton**<Model>(()=>MyModel());
```

## 3 .注册工厂(FactoryFunc <t>factoryFunc)</t>

我们使用这个方法在每次调用 get < T >()时返回 T 类型的新实例

```
serviceLocator.**registerFactory**<Model>(()=>MyModel());
```

请记住，您不能使用 registerFactory()动态地将任何参数传递给构造函数。

假设您的 MyModel()类有一个参数，

```
class MyModel{
     String name;
     MyModel(this.name);
}
```

如何在 get_it 中注册这个实例？

## 4 . registerFactoryParam(factory func

Using this method we can instantiate instances that take at most 2 parameters in their constructor.

Consider this model class,

```
class Person{
     String name;
     int age;
     **MyModel(this.name,this.age);**
}
```

You can register an instance of Person class using,

```
serviceLocator
  .**registerFactoryParam<Person,String,int>**
   ((name, age) => Person(name,age));
```

> **registerFactoryParam<Type，Param1，param 2>**
> 
> Type:要注册的类的类型
> 
> Param1:第一个参数的类型
> 
> Param2:第二个参数的类型

要获取实例，

```
var logger = serviceLocator.get<Person>**(param1:'user',param2: 20)**;
```

如果只需要一个实例，将 **void** 设置为第二个参数

```
serviceLocator
  .registerFactoryParam<MyModel,String,void>
   ((name, _) => MyModel(name,20));
```

那么不需要传递 param2，

```
var logger = serviceLocator.get<Person>**(param1:'user')**;
```

## 5.registerFactoryAsync(FactoryFuncAsync<t>func)</t>

有时我们需要异步实例化一个对象，我们可以用这种方法注册这些类型的对象。

考虑一个例子，

在这种情况下，最近的事件只能异步创建。我们可以使用

```
serviceLocator.**registerFactoryAsync**<Event>(
               () => **Event.*createRecentEvent*()**);
```

为了得到那个实例，我们必须使用 **getAsync < T >()，**它将返回 Future < T >

```
void main() async{
    Event event = await serviceLocator.**getAsync<Event>()**;
    //...
} 
```

我们对以上指定的所有方法都有异步支持，比如

*   **registerSingletonAsync()**
*   **registerLazySingletonAsync()**
*   **registerfactoryparamsync()**

更多信息，请查看 get_it 的[正式文档](https://pub.dev/packages/get_it)

## 使用名称注册

如果你想注册同类型的两个实例，我们可以这样做吗？

没有意义的代码😵

不，我们不能，它会抛出一个错误，因为默认情况下 get_it 只允许类型注册一次。您可以通过设置来更改它

```
void setUp(){
 **serviceLocator.allowReassignment=true;**     //registrations
}
```

但是当你调用**get<WebService>()**时，你会得到最后注册的 web service。那么我们如何拥有同一个类的两个实例并在我们的代码中使用它们呢？

## **实例名参数**

instanceName 是一个关键字参数，我们可以在注册实例时将它传递给任何注册方法。如果我们使用 instanceName，我们的实例用名称**和类型**注册。

我们可以通过，

```
var webserive = serviceLocator
                 .get<MyWebService>(**instanceName: 'v2'**); 
                 // get the second registred webservice
```

> 所有**相同类型的注册实例必须有唯一的实例名**。

示例:

为了获得实例，

```
var webserive = serviceLocator
                 .get<MyWebService>**(instanceName: 'v2')**; 
                 //will return the second registred MyWebService
var db = serviceLocator
                 .get<MyDatabase>**(instanceName: 'v2')**;
                 //will return the second registred MyDatabase
```

我们可以用 get_it 做的事情还有很多，官方文档什么都有涵盖，你可以在这里[看一下](https://pub.dev/packages/get_it)。

## 处置参数

有时我们需要做一些事情，比如关闭一个流，注销时写入日志，或者重置已注册的实例。可以在 dispose 参数中指定的。

```
serviceLocator.registerSingleton(
         MyWebService(),
         dispose: (webservice) => webservice.closeConnection());
```

每个注册方法都有 dispose 功能。

## **取消实例注册**

当我们不再需要某个实例或者想要重新初始化它时，我们可以注销该实例。

语法是

```
**void** unregister<T>({Object instance,String instanceName, **void** Function(T) disposingFunction})
```

> **实例:**要取消注册的实例。
> 
> **instanceName:** 要取消注册的已注册实例的名称。
> 
> **disposingFunction :** 注销时要调用的函数。

1.  要仅通过**键入**来取消注册，请使用

```
serviceLocator.unregister**<MyWebService>**();
```

2.若要按 instanceName 取消注册，请使用

```
serviceLocator.unregister<MyWebService>**(instanceName:'v1')**;
```

3.若要由实例本身注销，请使用

```
var myWebservice = serviceLocator
                     .get<MyWebService>(instanceName:'v1');
// ....
serviceLocator.unregister<MyWebService>**(instance:myWebservice)**;
```

> 当您指定了**实例**参数时，**实例名**参数不被考虑

*   如果您想在注销实例之前执行任何处置功能，您可以使用 **disposingFunction()**

```
serviceLocator.unregister<MyWebService>(
   **disposingFunction: (webservice)=> webservice.closeConnection()**);
```

> 当指定 disposingFunction() **，**时，不考虑注册实例时指定的 dispose 函数。

要取消注册 **lazySingletons** 使用，

```
**void** resetLazySingleton<T>({Object instance,
                            String instanceName,
                            **void** Function(T) disposingFunction})
```

## 完全重置 GetIt

您可以完全删除所有已注册的实例，并使用 **reset()** 功能重新开始。

语法是:

```
Future<**void**> reset({bool dispose = **true**});
```

当我们在进行单元测试时，当我们想要频繁地清除所有注册的实例(对于每个测试或每个组)时，这种方法非常方便。

```
await serviceLocator.**reset()**;
```

*   如果在注册时我们已经指定了一个类型，那么将调用每个注册类型的 dispose 函数。
*   If `**dispose=false,**` 不调用任何 dispose 函数，注销所有已注册的类型。

# ServiceLocator 设计模式？

如果你看看 get_it 是如何工作的，你可以清楚地看到它只是帮助我们找到(定位)我们已经创建的实例(服务)。这就是为什么它属于服务定位器设计模式，而不属于依赖注入。

**依赖注入**

依赖注入为您解决了依赖性。它将在后台为您创建实例的依赖关系。

如果你对上面的类进行依赖注入，当你试图创建一个 HybridLogger 时，consoleLogger 和 fileLogger **将被注入** **you** 。您不希望手动创建依赖项并将其传递给构造函数。

但是使用 service Locator 你必须手动注入依赖关系，据我所知，我们可以通过两种方式手动注入依赖关系，

1.  **在类内使用 serviceLocator(GetIt.instance)(不推荐)**

上面的代码不好。为什么？我们可能会忘记注册依赖项(ConsoleLogger 或 FileLogger)，**，这可能会引发运行时错误。**

示例:

**2。构造函数注入+ get_it(良好实践)**

这将帮助我们通过`get_it` 实现依赖注入(在某种程度上)，但是我们仍然必须创建所有的实例并手动传递它们。

这确保了我们注册了所有的依赖项。但是当我们的应用程序变得太大时，最好使用依赖注入包，比如[injectible](https://pub.dev/packages/injectable)。

> 主要区别在于依赖关系是如何定位的，在服务定位器中，客户端代码请求依赖关系，在 DI 容器中，我们使用容器来创建所有对象，并将依赖关系作为构造函数参数(或属性)注入。

希望您对 get_it 以及如何使用 get_it 包有效地管理我们的依赖关系有所了解，get_it 包中有太多值得探索的内容。请不要忘记查看[官方文档](https://pub.dev/packages/get_it)。

如果你在这篇文章中发现了任何错误的信息，请在评论中指出，这将会有很大的帮助。感谢你的阅读，鼓掌👏如果你喜欢这篇文章。