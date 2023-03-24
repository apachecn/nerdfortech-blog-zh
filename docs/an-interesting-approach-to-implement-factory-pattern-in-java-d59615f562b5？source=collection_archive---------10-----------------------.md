# 一种用 Java 实现工厂模式的有趣方法

> 原文：<https://medium.com/nerd-for-tech/an-interesting-approach-to-implement-factory-pattern-in-java-d59615f562b5?source=collection_archive---------10----------------------->

## 使用 Spring 注释

每当添加新的接口实现时，Java 中接口的传统工厂类都需要更新工厂。通常有几种方法可以做到这一点，让我们首先通过一个例子来理解一种流行的方法。这里我创建了一个*车辆*接口，它是由一个[弹簧](https://spring.io/projects/spring-framework) [组件](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/stereotype/Component.html)类— *汽车*实现的，

如果您想知道 [*Setter*](https://projectlombok.org/features/GetterSetter) 注释，这是一个为类字段生成 Setter 的 [Lombok](https://projectlombok.org/) 注释。现在让我们为这个接口创建一个工厂，

这里，工厂是用一种更传统的方法实现的，它有一个 register 方法，每个实现类都将使用这个方法向工厂注册自己，如下所示:

务必注意*车辆工厂* bean 将通过[弹簧](https://spring.io/projects/spring-framework)自动连线[](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/annotation/Autowired.html)*。这种方法有一个问题——你总是需要向工厂注册一个实现，这是一个额外的手动步骤，并不真正有助于应用程序的业务逻辑，但在执行过程中是至关重要的。如果一个新的开发人员编写了一个接口的实现，并且不知道工厂，他/她可能会忘记向工厂注册 bean，这可能会破坏应用程序。这也意味着**工厂与接口实现**紧密耦合，因为添加新的工厂需要手工更新工厂。通过构造函数手动将每个 bean 注册到工厂也可以被称为样板文件，因为这是重复的，而且如上所述，不会增加业务逻辑。*

*这就是使用注释的地方。注释的核心目的是存储与类或其组件相关联的元数据。我们是否可以利用这些注释在不调用任何 register 方法的情况下自动向工厂注册新的 beans？我们当然可以。这里是如何—*

1.  ***既然我们在这些例子中使用的是 S**[**spring**](https://spring.io/projects/spring-framework)**框架，那就让我们重用一下** [***组件***](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/stereotype/Component.html) **注释来达到我们的目的。***
2.  ***我们将为每个带注释的类命名，我们的工厂将使用它来注册 bean。***
3.  ***我们将**[***Autowire***](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/annotation/Autowired.html)**的所有接口实现，我们将通过它们来浏览，然后读取注释，然后将它们注册到工厂。***

*让我们看看它的实际效果。我们现在将用在 [*组件*](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/stereotype/Component.html) 注释中配置的名称更新*汽车*类，*

*现在让我们更新工厂的逻辑来读取这些注释，并在实例化期间向工厂注册相应的 beans，*

*同样，请注意*列表<车辆>* 将在实例化期间与*车辆*接口实现列表[](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/annotation/Autowired.html)*自动关联。让我们来理解这种说法，**

```
**vehicles.forEach(vehicle ->                      vehicleMap.put(vehicle.getClass().
getAnnotationsByType(Component.class)[0].value(), vehicle));**
```

**在这里，我们遍历每个实现，并把它放入工厂映射，key 作为 [*组件*](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/stereotype/Component.html) 注释的值，value 作为实现的 bean 本身。该键在检索过程中用于返回接口的正确实现。**

**因此，我们在这里，不像实现工厂模式的更传统的方法需要更新工厂类本身或 bean 注册的附加语句，**这种基于注释的方法与** [**Spring**](https://spring.io/projects/spring-framework) **框架一起通过自动向工厂注册任何新的实现来处理它。这不仅导致了工厂与底层实现的解耦，还确保了应用程序中样板代码的减少。****