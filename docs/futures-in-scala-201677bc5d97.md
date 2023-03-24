# Scala 中的期货

> 原文：<https://medium.com/nerd-for-tech/futures-in-scala-201677bc5d97?source=collection_archive---------9----------------------->

**异步执行**

考虑下面 scala 中给出的代码，它休眠 10 秒钟并返回结果。也就是 21+21。

```
**Thread**.sleep(**10000**); **21** + **21**
```

如果代码需要 10 分钟睡眠，而不是 10 秒。你能想象一个 10 分钟后返回结果的 API 吗？！。当然不是，我们期望 API 在几秒钟内响应，这是异步执行发挥作用的场景。Java 为在后台执行线程提供了多线程支持，但是这种机制是由监视器管理的，监视器提供锁，并且一次只允许一个线程进入。Python 提供了 celery，这是一个完全不同的进程，在后台执行任务。这些流程可用于在后台异步执行不同的任务。

这些在后台执行的任务是死锁和竞争条件的雷区。一千件事情都可能出错，现在让我们看看如何使用 scala futures 来避免这些死锁和竞争情况。

![](img/c8158858eb1db88dccc6560c5ee94eb3.png)

**Scala 期货**

当您调用一个函数时，线程会等待函数执行完毕并返回结果，如果结果是未来的，那么执行将由一个完全不同的线程异步执行。

```
scala> **import** **scala.concurrent.ExecutionContext.Implicits.global** import scala.concurrent.ExecutionContext.Implicits.globalscala> **val** fut **=** **Future** { **Thread**.sleep(**10000**); **21** + **21** }
fut: scala.concurrent.Future[Int] = Future(<not completed>)
```

上述函数将继续执行该方法，通常是通过一个完全不同的线程。无论执行是否完成，无论状态是成功还是失败，我们都可以继续轮询以获得未来的状态。

```
scala> fut.isCompleted
res0: Boolean = truescala> fut.value
res3: Option[scala.util.Try[Int]] = Some(Success(42))
```

10 秒钟后，未来的结果将如上图所示。在完成之前, *isCompleted 结果将为假，值将为无。*

**用地图改变未来**

映射函数可用于将一个未来的结果映射到另一个未来。我们可以明确指定如何处理未来的结果。

```
scala> **val** fut **=** **Future** { **Thread**.sleep(**10000**); **21** + **21** }
fut: scala.concurrent.Future[Int] = ...scala> **val** result **=** fut.map(x **=>** x + **1**)
result: scala.concurrent.Future[Int] = ...//After 10 secondsscala> result.value
res6: Option[scala.util.Try[Int]] = Some(Success(43))
```

**用 for 表达式转换未来**

我们可以并行运行两个未来，并结合它们的结果，创造一个新的未来。

```
scala> **val** fut1 **=** **Future** { **Thread**.sleep(**10000**); **21** + **21** }
fut1: scala.concurrent.Future[Int] = Future(<not completed>)scala> **val** fut2 **=** **Future** { **Thread**.sleep(**10000**); **23** + **23** }
fut2: scala.concurrent.Future[Int] = Future(<not completed>)scala> **val** rs2**=for** {
     | x **<-** fut1
     | y **<-** fut2
     | } **yield** x + y
rs2: scala.concurrent.Future[Int] = Future(<not completed>)scala> rs2.value
res20: Option[scala.util.Try[Int]] = Some(Success(88))
```

如果您不想并行运行期货，请在 for 循环中指定它。例如

```
scala> **for** {
x **<-** **Future** { **Thread**.sleep(**10000**); **21** + **21** }
y **<-** **Future** { **Thread**.sleep(**10000**); **23** + **23** }
} **yield** x + y
res9: scala.concurrent.Future[Int] = ...
```

成功和失败的方法可以用来创造一个已经失败或者已经成功的未来。

```
scala> **Future**.successful { **21** + **21** }
res21: scala.concurrent.Future[Int] = Future(Success(42))scala> **Future**.failed(**new** **Exception**("hackkkkkkkkkkkkk!"))
res23: scala.concurrent.Future[Nothing] = Future(Failure(java.lang.Exception: hackkkkkkkkkkkkk!))
```

未来可以用承诺来创造和控制。承诺是创造和控制未来最普遍的方式。当你完成了承诺，未来也就完成了。承诺可以以成功、失败或完成的形式完成。

```
scala> **import** **scala.concurrent._**
import scala.concurrent._scala> **val** pro**=Promise**[**Int**]
pro: scala.concurrent.Promise[Int] = Future(<not completed>)scala> **val** fut **=** pro.future
fut: scala.concurrent.Future[Int] = Future(<not completed>)scala> fut.value
res24: Option[scala.util.Try[Int]] = Nonescala> pro.success(**42**)
res25: pro.type = Future(Success(42))
```

**过滤、收集和转换期货**

filter 和 collect 是用于从 future 中过滤出来并对其执行一些操作的两种方法。

以下示例显示了对未来值的筛选操作，筛选大于零的值。如果我们应用过滤操作来过滤小于零的值，结果将是零。

```
scala> **val** fut **=** **Future** { **42** }
fut: scala.concurrent.Future[Int] = Future(<not completed>)scala> **val** valid **=** fut.filter(res **=>** res > **0**)
valid: scala.concurrent.Future[Int] = Future(<not completed>)scala> valid.value
res26: Option[scala.util.Try[Int]] = Some(Success(42))
```

collect 方法用于验证未来的输出，并对其执行一些转换操作。

```
scala> **val** fut **=** **Future** { **42** }
fut: scala.concurrent.Future[Int] = Future(Success(42))scala> **val** valid**=**fut collect { **case** res **if** res > **0** **=>** res + **46** }
valid: scala.concurrent.Future[Int] = Future(<not completed>)scala> valid.value
res31: Option[scala.util.Try[Int]] = Some(Success(88))
```

转换操作可以应用于条件执行的未来，以指定如果未来成功该做什么以及如果失败该做什么。在下面的例子中，如果未来是成功的，它将用于第一个条件，如果未来是失败的，它将用于第二个条件。

```
scala>  **val** success **=** **Future** { **42** }
success: scala.concurrent.Future[Int] = Future(<not completed>)scala> **val** first **=** success.transform(
| res **=>** res * -**1**,
| ex **=>** **new** **Exception**("see cause", ex)
| )
first: scala.concurrent.Future[Int] = Future(<not completed>)scala> first.value
res34: Option[scala.util.Try[Int]] = Some(Success(-42))
```

**编写期货测试用例**

我们已经看到了如何创造未来。现在让我们看看如何编写测试用例并成功测试我们的未来。Scala 提供了一个名为 await 的方法来阻塞我们的线程一段时间。在那之后，我们将得到结果，并可以测试它。在下面的例子中，await 接受一个时间延迟参数，等待特定的时间量并返回结果。获得结果后，我们可以断言结果并测试函数。

```
scala> **import** **scala.concurrent.Await**
import scala.concurrent.Awaitscala> **import** **scala.concurrent.duration._**
import scala.concurrent.duration._scala> **val** fut **=** **Future** { **Thread**.sleep(**10000**); **21** + **21** }
fut: scala.concurrent.Future[Int] = Future(<not completed>)scala> **val** x **=** **Await**.result(fut, **15.**seconds)
x: Int = 42
```