# JavaScript 类与原型——组织 JavaScript 代码

> 原文：<https://medium.com/nerd-for-tech/javascript-class-vs-prototype-organizing-javascript-code-d909ce1c5f6a?source=collection_archive---------14----------------------->

![](img/db590bb27b56bd4f2bc40603144afeba.png)

关于 [JavaScript](https://en.wikipedia.org/wiki/JavaScript) 语言的故事相当有趣。对于那些不知道的人，下面是流行的多范例语言的一些亮点:

*   布伦丹·艾奇([@布伦丹·艾奇](http://twitter.com/BrendanEich))网景通信公司的一名程序员在 1995 年只用了 10 天就创造了摩卡。
*   摩卡将很快被命名为 JavaScript——它与 [Java](https://en.wikipedia.org/wiki/Java_(programming_language)) 毫无关系——很可能是一种利用 Java 流行度的营销手段。
*   在网站大多是静态的时候，JavaScript 作为一种选项被引入，以提供动态编程体验。
*   自 1996 年以来， [ECMAScript](https://en.wikipedia.org/wiki/ECMAScript) 语言规范(ECMA-262)已经定义并标准化了 JavaScript 语言，现在是第 11 版。
*   JavaScript 运行在全球大约 97%的浏览器上。
*   JavaScript 已经成为客户端框架的首选，如 [Angular](https://angular.io/) 、 [React](https://reactjs.org/) 和 [Vue.js](https://vuejs.org/) ，以及 [Node.js](https://nodejs.org/en/) 运行时。

自从 JavaScript 成为主流，我就参与了以这样或那样的方式使用 JavaScript 的项目。在早期，HTML 文件引用 JavaScript 在向后端服务发送请求之前执行简单的验证。现在，我在过去 7 年中参与的每个基于网络的项目都使用完全由 JavaScript 构建的客户端框架。

然而，JavaScript 并非没有设计挑战，我在我的“[JavaScript 会通过时间的考验吗？](https://dzone.com/articles/will-javascript-pass-the-test-of-time)“出版时间回溯到 2017 年 6 月。

当时没有提到的一个项目是关于在 JavaScript 中何时使用类以及何时使用原型的讨论。我这篇文章的目标是关注这些概念——即使是利用现有的框架，比如[sales force Lightning Web Components](https://developer.salesforce.com/docs/component-library/documentation/en/lwc)(LWC)。

# JavaScript 中的原型概念

出于本文的目的，最好先谈谈 JavaScript 中的原型概念。

在 JavaScript 中，所有对象都从原型继承属性和方法。让我们考虑下面的原型例子:

```
function Vehicle(vinNumber, manufacturer, productionDate, fuelType) {
  this.manufacturer = manufacturer;
  this.vinNumber = vinNumber;
  this.productionDate = productionDate;
  this.fuelType = fuelType;
}Vehicle.prototype.vehicleInformation = function() {
  var productionDate = new Date(this.productionDate * 1000);
  var months = ['Jan','Feb','Mar','Apr','May','Jun','Jul','Aug','Sep','Oct','Nov','Dec'];
  var year = productionDate.getFullYear();
  var month = months[productionDate.getMonth()];
  var day = productionDate.getDate();var friendlyDate = month + ' '  + day + ', ' + year;
  return this.manufacturer + ' vehicle with VIN Number = ' + this.vinNumber + ' was produced on ' + friendlyDate + ' using a fuel type of ' + this.fuelType;
}
```

该代码的结果是有了一个可用的`Vehicle`对象，并且可以使用以下代码创建一个新实例:

```
let rogue = new Vehicle('5N1FD4YXN11111111', 'Nissan', 1389675600, 'gasoline');
```

有了这些信息，可以使用下面的方法调用`vehicleInformation()`函数:

```
alert(rogue.vehicleInformation());
```

这将产生一个包含以下消息的警报对话框:

> “VIN 号= 5N1FD4YXN11111111 的日产汽车于 2014 年 1 月 14 日生产，使用的燃料类型为汽油”

正如人们所料，可以引入第二个原型`SportUtilityVehicle`来进一步定义给定的车辆类型:

```
function SportUtilityVehicle(vinNumber, manufacturer, productionDate, fuelType, drivetrain) {
  Vehicle.call(this, vinNumber, manufacturer, productionDate, fuelType);
  this.drivetrain = drivetrain;
}
```

现在，我们可以新造一个`SportUtilityVehicle`来代替简单的`Vehicle`。

```
let rogue = new SportUtilityVehicle('5N1FD4YXN11111111', 'Nissan', 1389675600, 'gasoline', 'AWD');
```

我们还可以用`SportUtilityVehicle`原型定义一个新版本:

```
SportUtilityVehicle.prototype.vehicleInformation = function() {
  return this.manufacturer + ' vehicle with VIN Number = ' + this.vinNumber + ' utilizes drivetrain = ' + this.drivetrain + ' and runs on ' + this.fuelType;
}
```

现在，当使用以下方法调用`vehicleInformation()`函数时:

```
alert(rogue.vehicleInformation());
```

将出现一个警报对话框，其中包含以下消息:

> “VIN 号= 5N1FD4YXN11111111 的日产汽车采用动力传动系统= AWS，使用汽油行驶”

# JavaScript 类

从 ECMAScript 2015(2015 年 6 月发布的第 6 版)开始，JavaScript 引入了类的概念。虽然这可能会激起使用 Java、C#和 C++等语言的开发人员的兴趣，但引入 class 选项的目的是允许使用更简单、更干净的语法创建类。事实上，文档继续说明类仅仅是“语法糖”,让开发人员更容易理解。

将前面的示例从原型转换为类将如下所示:

```
class Vehicle {
  constructor(vinNumber, manufacturer, productionDate, fuelType) {
    this.manufacturer = manufacturer;
    this.vinNumber = vinNumber;
    this.productionDate = productionDate;
    this.fuelType = fuelType;
  }vehicleInformation() {
    var productionDate = new Date(this.productionDate * 1000);
    var months = ['Jan','Feb','Mar','Apr','May','Jun','Jul','Aug','Sep','Oct','Nov','Dec'];
    var year = productionDate.getFullYear();
    var month = months[productionDate.getMonth()];
    var day = productionDate.getDate();var friendlyDate = month + ' '  + day + ', ' + year;
    return this.manufacturer + ' vehicle with VIN Number = ' + this.vinNumber + ' was produced on ' + friendlyDate + ' using a fuel type of ' + this.fuelType;
  }
}class SportUtilityVehicle extends Vehicle {
  constructor(vinNumber, manufacturer, productionDate, fuelType, drivetrain) {
    super(vinNumber, manufacturer, productionDate, fuelType);
    this.drivetrain = drivetrain;
  }vehicleInformation() {
    return this.manufacturer + ' vehicle with VIN Number = ' + this.vinNumber + ' utilizes drivetrain = ' + this.drivetrain + ' and runs on ' + this.fuelType;
  }
}
```

如果我们需要向`SportUtilityVehicle`类添加 getters 和 setters，可以如下所示更新该类:

```
class SportUtilityVehicle extends Vehicle {
  constructor(vinNumber, manufacturer, productionDate, fuelType, drivetrain) {
    super(vinNumber, manufacturer, productionDate, fuelType);
    this.drivetrain = drivetrain;
  }vehicleInformation() {
    return this.manufacturer + ' vehicle with VIN Number = ' + this.vinNumber + ' utilizes drivetrain = ' + this.drivetrain + ' and runs on ' + this.fuelType;
  }get drivetrain() {
    return this._drivetrain;
  }set drivetrain(newDrivetrain) {
    this._drivetrain = newDrivetrain;
  }
}
```

如您所见，语法类似于 Java 或 C#之类的语言。类方法还允许属于原型链的函数和属性不引用 Object.prototype 语法。一个要求是构造函数总是被称为“构造函数”。

# JavaScript 类原型

如上所述，JavaScript 中的类只是一种语法糖，让使用 JavaScript 的特性开发人员更容易使用。虽然这种方法为来自 Java、C#或 C++等语言的人提供了更通用的设计，但许多 Javascript 纯粹主义者建议根本不要使用类。

事实上，Michael Krasnov 在“[请停止在 JavaScript](https://everyday.codes/javascript/please-stop-using-classes-in-javascript/) 中使用类”一文中提到了一个相关问题:

> 绑定问题。由于类构造函数与这个关键字密切相关，它可能会引入潜在的绑定问题，尤其是当您试图将类方法作为回调传递给外部例程时。

Michael 继续给出了避免使用 Javascript 类的其他四个原因，但是 class 选项的支持者很快减轻了他的思想的重量。

从 2021 年开始，我一直坚持以下对任何 IT 专业人士的使命宣言:

> “将您的时间集中在提供扩展您知识产权价值的特性/功能上。将框架、产品和服务用于其他一切。”

当谈到在 JavaScript 中使用类或原型时，我觉得这应该由支持和维护代码库的团队来做决定。如果他们的舒适度没有问题，那么他们应该相应地设计他们的组件。然而，如果偏好利用类的概念，团队中的开发人员应该理解上面提到的绑定挑战，但是应该继续前进，并保持在他们的舒适区内。

# 对 Lightning Web 组件的影响

几年前，Salesforce 推出了 Lightning Web Components (LWC)，我在“ [Salesforce 提供 JavaScript 编程模型](https://dzone.com/articles/salesforce-offering-javascript-programming-model)”一文中谈到过。近三年后，我发现自己在谈论使用类和原型方法对 Salesforce 开发人员的影响。

简单的回答是……没关系。Salesforce 允许 Lightning Web 组件利用原型或类。JavaScript 的典型继承模型是通过原型。但是为了吸引习惯于经典继承的开发人员，有一种语法上的糖可以帮助开发人员通过使用一种看起来非常像经典继承的方法来实现原型继承。

因此，当谈到 LWC 时——这完全是关于继承的，因为 LWC 已经为你构建了一个很棒的基类组件来扩展——你也可以利用这个语法上的优势。

你不需要担心原型继承，即使这一切都发生在幕后。只要做古典继承的事情，你就是黄金。

下面是这种情况的一个例子:

```
import { LightningElement } from 'lwc';export default class VehicleComponent extends LightningElement {
  // properties go herevehicleInformation() {
    return this.manufacturer + ' vehicle with VIN Number = ' + this.vinNumber + ' utilizes drivetrain = ' + this.drivetrain + ' and runs on ' + this.fuelType;
  }
}
```

看到了吗？LWC——知道 JavaScript 给了你这种语法上的甜头——让它变得如此简单。

# 结论

我承认 JavaScript 不是我花了大部分时间开发特性的语言。除了使用 Node.js 进行客户端开发之外，作为一名服务开发人员，我的主要工作通常是关注其他语言选项。

同时，在原型方法上使用类方法为 Java、C#和 C++开发人员提供了一个类似的桥梁。虽然这里没有正确的答案，但是理解 JavaScript 中类和原型的工作原理是很重要的。

最后，我们的角色就是能够支持您的代码库，解决缺陷，并快速改变特性和功能。实现的方法应该总是完全由特性团队的理解和维护所选标准的能力来驱动。

祝你今天过得愉快！