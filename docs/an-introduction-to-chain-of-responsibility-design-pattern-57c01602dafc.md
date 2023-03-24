# 责任链设计模式介绍

> 原文：<https://medium.com/nerd-for-tech/an-introduction-to-chain-of-responsibility-design-pattern-57c01602dafc?source=collection_archive---------24----------------------->

![](img/59c474c7d85b1c3346d5c8b5328044a8.png)

责任链设计模式介绍

当我们需要在工作中应用松散耦合的概念时，可以应用责任链模式。这被归类在行为模式下。

让我们通过一个真实的例子来理解应用这种模式的方法。

*假设，某公司计算每个员工在特定月份的工资，如果某员工的工作时间介于 180 小时和 200 小时之间，则在基本工资上增加 30%的基数。如果员工的工作时间介于 200 小时和 220 小时之间，项目中的额外金额将被添加到之前的金额中，并且工作时间超过 220 小时，则再次将另一个金额添加到之前的金额中。*

为了实现这一点，首先，我通过扩展 Employee 创建了 EmployeeSalary 类，并向它添加了一些参数。请参考以下代码。

接下来，我通过实现 SalaryAdd 接口创建了一个名为 EmployeeSalaryHandler 的类，并声明了一个名为 *successor* 的变量来保存这个类的下一个节点的实例。除此之外，我还通过扩展 EmployeeSalaryHandler 类和实现从 EmployeeSalaryHandler 类继承的不完整方法 addSalary()创建了另外 3 个类，分别称为 Level1、Level2 和 Level3。请参考以下代码。

工资的计算是通过调用 Level1、Level2 和 Level3 类中的 addSalary(employeeSalary)来完成的。这里，计算从级别 1 开始，如果 EmployeeSalary (hours)的实例不满足条件，接下来，它将指向级别 1(级别 2)的继任者，同时将基本工资增加 30%。级别 2 继续保持不变，直到实例满足(小时)条件。但是在做所有这些事情之前，我们必须在开始时设置链条。因此，我创建了一个名为 BindChain 的类来创建这个链。参考下面的代码。

在这里，我已经创建了 3 个级别之间的链，并将级别 1 设置为初始级别。

最后，我在 MainApplication 类中实现了 main 方法。参考下面的代码。

在这里，我实现了 EmployeeSalary 的 4 个实例，并通过使用 level1 作为初始值来调用 addSalary(employeeSalary)方法。但是，您可以通过简单地更改 BindChain 类中的级别来更改继任者的首字母和顺序。

下面定义了输出。

```
Employee Salary Sheet: 0001
Employee Name: Hasini
Employee Position: ASE
Total Hours Worked: 200
Base Salary: 5000.0
Addition: 1500.0
_____________________________
Total Salary: 6500.0
_____________________________
_____________________________Employee Salary Sheet: 0002
Employee Name: Hasini
Employee Position: SE
Total Hours Worked: 250
Base Salary: 6000.0
Addition: 4800.0
_____________________________
Total Salary: 10800.0
_____________________________
_____________________________Employee Salary Sheet: 0003
Employee Name: Hasini
Employee Position: AQA
Total Hours Worked: 150
Base Salary: 5000.0
Addition: 0.0
_____________________________
Total Salary: 5000.0
_____________________________
_____________________________Employee Salary Sheet: 0004
Employee Name: Hasini
Employee Position: QA
Total Hours Worked: 210
Base Salary: 6000.0
Addition: 2800.0
_____________________________
Total Salary: 8800.0
_____________________________
_____________________________
```

我再次用 JavaScript 实现了相同的场景。首先，我通过扩展 Employee 类创建了一个名为 EmployeeSalary 的类，并像前面一样继续其余的部分。参考下面的代码。

```
EmployeeSalary {   
  id: '0001',      
  name: 'Hasini',  
  position: 'ASE', 
  hours: 200,      
  baseSalary: 5000,
  addition: 0,     
  salary: 0        
}
```

这里我也使用了相同的逻辑来再次实现这个模式。参考下面的代码。

```
EmployeeSalary {   
  id: '0001',      
  name: 'Hasini',  
  position: 'ASE', 
  hours: 200,      
  baseSalary: 5000,
  addition: 1500,  
  salary: 6500     
}
EmployeeSalary {
  id: '0002',
  name: 'Hasini',
  position: 'SE',
  hours: 250,
  baseSalary: 6000,
  addition: 4800,
  salary: 10800
}
EmployeeSalary {
  id: '0003',
  name: 'Hasini',
  position: 'AQA',
  hours: 150,
  baseSalary: 5000,
  addition: 0,
  salary: 5000
}
EmployeeSalary {
  id: '0004',
  name: 'Hasini',
  position: 'QA',
  hours: 210,
  baseSalary: 6000,
  addition: 2800,
  salary: 8800
}
```

这是文章的结尾。我想你已经对责任链设计模式的基础有了一些了解。我希望在我的下一篇文章中讨论另一种设计模式。

[](https://github.com/HasiniSandunika/Design-Patterns-Java.git) [## GitHub-HasiniSandunika/Design-Patterns-Java:用 Java 实现设计模式。

### 用 Java 实现设计模式。为 HasiniSandunika/Design-Patterns-Java 开发做出贡献，创建…

github.com](https://github.com/HasiniSandunika/Design-Patterns-Java.git) 

**参考文献**

*   https://www.youtube.com/watch?v=iOOnctEDp84&list = PLD-myte BG 3 x 86 i3 uyaxwzkfvtuy 2g MDO&index = 5
*   【https://www.javatpoint.com/chain-of-responsibility-pattern 
*   [https://www . tutorialspoint . com/design _ pattern/chain _ of _ respons ibility _ pattern . htm](https://www.tutorialspoint.com/design_pattern/chain_of_responsibility_pattern.htm)