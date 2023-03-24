# Lambda 表达式简介

> 原文：<https://medium.com/nerd-for-tech/introduction-to-lambda-expressions-577c3cce7f84?source=collection_archive---------8----------------------->

![](img/98515cb334238027d8ec09d38b5b146f.png)

在我们早期的一篇博客中，我们已经探索了坚实的原则。据此，创建执行单个活动的专用类和方法是要遵循的最佳实践之一。想象一下，创建了一个方法，但是在代码中只使用了一次。根据项目的可见性，它也不会在未来的任何时候被用完。在这种情况下，定义方法、输入类型和输出的工作量相当大，因为它在整个代码中被使用的次数很多。

在这种情况下，一次性可用的方法不太关注实现、参数规范、声明、访问说明符、返回值和名称，它允许我们在需要的地方编写方法。这对于完成任务来说将是非常方便的。

幸运的是，C#的 3.0 版本(.Net framework 3.5)，提供 lambda 表达式来创建可用于编写表达式的匿名方法。这就是只在一个实例中使用的操作序列，它还允许根据需要编写单行或多行匿名方法。我们来详细看看这个博客里有哪些 lambda 表达式。

**目录**

1.  什么是 Lambda 表达式？
2.  C#中如何使用 Lambda 表达式？
3.  结论

# 什么是 Lambda 表达式？

Lambda 表达式提供创建匿名函数的能力。每个 lambda 表达式都由 lambda 运算符“= >”组成，该运算符拆分匿名方法的输入参数和基于该参数执行的操作。

(输入参数)= >表达式

(输入参数)=> {要执行的语句序列}

Lambda 表达式可以转换为委托类型。有输入参数但没有返回类型的 Lambda 表达式可以转换为操作委托类型。

示例-

```
Action<string> welcomeUser = inputName => { ​ Console.WriteLine(string.Format("Welcome {0}.", inputName)); } welcomeUser("PARTECH");
```

类似地，具有参数和返回类型的 lambda 表达式可以转换为 Func 委托类型。

示例-

```
Func<int, int> radiusOfCircle = inputRadius => (3.14 * inputRadius * inputRadius); Console.WriteLine(radiusOfCircle(5));
```

这里，Func <int int="">分别表示输入和结果的数据类型。</int>

# C#中如何使用 Lambda 表达式？

在这一节中，让我们看一些在 C#中可以使用 lambda 表达式的实际例子。

## 场景 1

想象一个场景，其中字符串列表包含需要以逗号分隔格式显示的特定数据。用 Lambda 表达式很容易实现。

```
var data = new List<string> { "One", "Two", "Three", "Four", "Five" }; var result = String.Join(", ", data.Select(x => x));
```

结果将是——“一、二、三、四、五”

## 场景 2

想象一个场景，其中有一个字符串列表，我们需要在它前面加上一些数据来一起处理它。让我们看看如何使用 lambda 表达式来实现这一点。

```
var data = new List<PartechNonGeneric> { new PartechNonGeneric { Property = false, Data ="One" } }; data.ForEach(x => x.Data = "PARTECH_" + x.Data);
```

在这种情况下，前缀 data 'PARTECH_ '被附加到集合中存在的所有对象上。

## 场景 3

要检查特定数据是否出现在集合中，可以使用 lambda 表达式。考虑一个场景，用户想要检查名字是否存在于集合中。在这种情况下，可以使用 lambda 表达式。

```
var data = new List<PartechPersonalData> { new PartechPersonalData{ FirstName= "Test", LastName="User" } }; var exists = data.Exists(x => x.FirstName = "PARTECH");
```

如果所选过滤器的数据存在，上述代码返回 true，否则返回 false。

## 场景 4

考虑一个定制类列表，有必要过滤掉名字为 xx 的对象。发布这个，对特定对象而不是整个列表本身进行一些操作(比如我们之前看到的 foreach 操作)。

```
var data = new List<PartechPersonalData> { new PartechPersonalData{ FirstName= "Test", LastName="User" }, new PartechPersonalData{ FirstName= "Test2", LastName="User" } }; var partechData = data.Find(x => x.FirstName = "Test");
```

上面的代码将集合中第一个与提供的条件匹配的对象作为 lambda 表达式返回。

## 场景 5

要返回匹配相似条件的多个对象，可以使用下面的代码。设想一个场景，其中需要过滤 18 岁及以上的学生。对于这种情况，可能会出现多个学生记录。使用 lambda 表达式，可以捕获通过该条件的所有对象。

```
var data = new List<PartechPersonalData> { new PartechPersonalData{ FirstName= "Test", LastName="User", DateOfBirth = "2002-02-21" }, new PartechPersonalData{ FirstName= "Test2", LastName="User", DateOfBirth = "2002-02-28" } }; var partechData = data.FindAll(x => (x.DateOfBirth.Year - DateTime.Now.Year) >= 18);
```

上面的代码检查自定义列表中的所有记录，并检查正在定义的条件，然后返回与所提供的条件匹配的所有对象。

## 场景 6

要返回集合中所有对象的单个属性，可以使用 lambda 表达式。假设有一个保存名字、姓氏和出生日期的自定义类。考虑一个场景，其中需要只获取集合中出现的名字。使用 lambda 表达式，可以从自定义类集合中仅检索选定的属性。

```
var data = new List<PartechPersonalData> { new PartechPersonalData{ FirstName= "Test", LastName="User", DateOfBirth = "2002-02-21" }, new PartechPersonalData{ FirstName= "Test2", LastName="User", DateOfBirth = "2002-02-28" } }; var firstNameData = data.Select(x => x.FirstName);
```

在上面的代码中，带有属性的 Select 语句只提供自定义集合中的名字。

与上面的场景类似，可以用 lambda 表达式执行许多其他操作，比如修改和格式化字符串、日期格式、执行计算等。

# 结论

Lambda 表达式是一个强大的工具。它使得实现数据操作、自定义选择和许多其他流行的操作成为可能。最好的部分是，它仅仅使用简单的匿名方法、lambda 操作符和输入数据就完成了。

*原载于*[*https://www . partech . nl*](https://www.partech.nl/nl/publicaties/2021/03/introduction-to-lambda-expressions)*。*