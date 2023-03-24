# 避免使用构建器模式伸缩构造器

> 原文：<https://medium.com/nerd-for-tech/avoid-telescoping-constructors-with-the-builder-pattern-2114b75360b7?source=collection_archive---------10----------------------->

![](img/73d4733fc687c1548c15b414918d7889.png)

图片来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=516559) 的 [Steffi Timm](https://pixabay.com/users/sttimm-265330/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=516559)

在有些情况下，一个应用需要多个 C *构造器*。这可能是由于用户的要求。但是多重*构造函数*的问题是，如果参数太多，你的代码可能会变得难看(太复杂)。看看下面的例子。

然而，一些程序员并没有像上面那样实现*构造函数*，而是使用了所谓的*伸缩*构造函数*。*

因此，这里有一个如何使用*伸缩构造函数*的例子。这只是一种方法(在这个例子中你可以看到*构造函数*引用前面的*构造函数*来赋值)。

按照这种方法，一个*伸缩构造器*中可以有多个*构造器*，每个*构造器*可以根据参数使用另一个*构造器*。(自己看下面的例子)。

这里是*伸缩* *构造器*的另一种方法，与上面不同。

但是这是一个糟糕的方法(使用*伸缩构造函数*)。因为这有点复杂，你可以看到我们到处传递空值(有太多的*空值*，我们很容易得到*空值指针异常*)。所以，这就是我们要用构建器模式来回答的问题。

> 你可能认为我们可以使用*设置器*，来分配变量值，而不是有多个*构造器*。但是 setter*的问题是它们不是不可变的。因此，一旦创建了对象，任何人都可以对其进行更改。因此*设置器*在某些情况下可能不是理想的选择。*

*所以让我们来看看如何一步一步地解决这个问题。我们正在进行“*一个员工的月薪是如何产生的*”的考试场景。在月薪中，我们有*员工姓名、员工邮箱、基本工资、车补*和*奖金*等变量。*

*首先，我们将所有这些变量放入`MonthlySalary` 类，并将另一个名为`builder`的*静态* *内部类*添加到`MonthlySalary` 类中。*

*因为我们有两个“必备”参数(`empId`、`basicSalary`)，我们将把它们作为参数传递给*构造函数*。所以，在对象创建时，它们总是被传递(如下)。*

```
 *// required parameter
  public MonthlySalaryBuilder(String empId, String basicSalary) {
     this.empId = empId;
     this.basicSalary = basicSalary;
  }*
```

*现在，让我们创建一些方法来为其他变量赋值。并且让我们把我们的`MonthlySalary`类的变量`final`(所以它们是不可变的)。*

*例如:*

```
 *//set value of employee email
  public MonthlySalaryBuilder addEmpEmail(String empEmail) {
      this.empEmail = empEmail;
      return this;
  }*
```

*好了，现在我们差不多完成了。为了完成一切，添加一个名为`build()`的方法来返回一个`MonthlySalary`和一个`toString()`的实例。*

*输出:*

```
*Employee Salary (telescoping-1) - Id: EMP-TC-1 Email: null Basic salary: null Motor car allowance: null Bonus: nullEmployee Salary (telescoping-2) - Id: EMP-TC-2 Email: null Basic salary: null Motor car allowance: null Bonus: nullEmployee Salary (Builder) - Id: EMP0001 Email: johnDoe@gmail.com Basic salary: 999.0 Motor car allowance: 250.0 Bonus: null*
```

*现在你可以看到，我们用构建器模式得到了相同的结果，但是我们没有使用多个*构造器*，任何*设置器*，而且我们的属性也是不可变的。此外，我们不需要传递任何空值。假设你有一个有 10 个参数的构造函数，你想在运动中传递一个参数。对于其余的参数，您必须传递 9 个空值。*

*然而，缺点之一是最初你必须做一些实现构建器模式的编码。*

*我希望您已经理解了构建器模式的概念。如果你正在寻找上述代码，我已经把它们保存在我的回购这里。*

*[](https://github.com/Hasitha-Su/Krish-LP-Training.git) [## hasit ha-Su/Krish-LP-培训

### 通过在 GitHub 上创建一个帐户，为 Hasitha-Su/Krish-LP-Training 开发做出贡献。

github.com](https://github.com/Hasitha-Su/Krish-LP-Training.git)* 

## *参考*

*[](https://www.journaldev.com/1425/builder-design-pattern-in-java) [## Java - JournalDev 中的生成器设计模式

### 今天我们将研究 java 中的构建器模式。生成器设计模式是一种创造性的设计模式，就像工厂…

www.journaldev.com](https://www.journaldev.com/1425/builder-design-pattern-in-java)*