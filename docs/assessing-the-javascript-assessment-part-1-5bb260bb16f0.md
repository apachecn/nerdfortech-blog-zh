# 评估 JavaScript 评估，第 1 部分

> 原文：<https://medium.com/nerd-for-tech/assessing-the-javascript-assessment-part-1-5bb260bb16f0?source=collection_archive---------15----------------------->

我以前的大多数文章都集中在 LinkedIn 的 HTML 和 CSS 评估中发现的问题/主题。现在，我将继续讨论 JavaScript 评估。和以前一样，我将只涉及您可能在评估中看到的一些内容，但是如果您正在考虑将 JavaScript 技能徽章添加到您的 LinkedIn 个人资料中，接下来的几篇文章应该会非常有帮助。

**代码的对象**

JavaScript 中有大量的对象。您可能熟悉创建对象的老式方法:对象文字，如下例所示。

```
var myDog = { name: “Rudy”, breed: “Parson Russell Terrier”, sex: “male”, age: 10, favoriteTreat: “peanut butter”};
```

最新版本的 JavaScript (E6)允许我们通过[类](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes)更有效地创建对象。我们可以使用一个类作为模板，轻松地生成多个相似的对象，而不是每次需要一个对象时都必须键入一个对象文字。例如，让我们创建一个狗类。

```
class Dog { constructor(name, breed, sex, age, favoriteTreat) { this.name = name; this.breed = breed; this.sex = sex; this.age = age; this.favoriteTreat = favoriteTreat; }}
```

有了这个类声明，我们可以很容易地创建其他的狗对象。

```
let myDog = new Dog(“Rudy”, “Parson Russell Terrier”, “male”, 10, “peanut butter”)
```

JavaScript 还有许多内置对象，比如 Date 对象。创造一个这样的东西很容易:

```
let date = new Date()
```

您可能会在 LinkedIn 评估中收到一个问题，要求您选择正确的语句来创建一个名为“student”的新人员对象。

```
var student = new Person();var student = construct Person();var student = Person();var student = construct Person;
```

如上面的例子所示，我们不使用单词*构造*来创建一个新对象，仅仅使用类名(或内置对象名)也是不够的。所以找出正确答案应该是小菜一碟—首选， *var student = new Person()* 。

**条款和条件**

"什么时候你会使用条件句？"

这个问题不应该让你太紧张。我们来看几个[条件语句](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Building_blocks/conditionals)的例子。

一、曾经流行的 [*if-else*](https://www.w3schools.com/js/js_if_else.asp) :

```
if(multipleOptions) {return “You need a conditional statement!”} else {return “You don’t need a conditional statement.”}
```

接下来， [*开关*](https://www.w3schools.com/js/js_switch.asp) :

```
switch (options) { case multipleOptions: answer = “You need a conditional statement!” break; case onlyOne: answer = “You don’t need a conditional statement.” break;}
```

然后， [*三进制运算符*](https://www.w3schools.com/js/js_comparisons.asp) :

```
let answer = (multipleOptions) ? “You need a conditional statement!” : “You don’t need a conditional statement.”
```

好的，我相信你已经猜到了这个问题的答案取决于你的代码中是否包含了*多个选项*。它与将数据分组在一起、循环一组语句或者将一行代码重复五次(没错，那其实是选择之一！).因此，如果您在评估中遇到这个问题，您应该选择“当您希望您的代码在多个选项之间进行选择时”

**三元故障**

说到三元运算符…

一个问题提出了下面的代码，并询问它有什么问题:

```
var a;var b = (a = 3) ? true : false
```

让我们仔细检查一下这段麻烦的代码。在第一行中，变量 *a* 被声明但没有赋值。在第二行中，我们看到一个三进制语句分配给变量 *b* 。这一行的语法是正确的，除了一个小的但是非常关键的错误。*？*和 *:* 使用正确，那么问题出在哪里？看一下条件，就是括号里的部分。这里我们看到变量 *a* 被赋值为 3。等等…为什么我们要给*条件*中的变量赋值？我们的三元运算符如何测试呢？嗯，不能。至少，它不能通过使用赋值运算符来测试 *a* 的值是否为 3。相反，它需要一个[比较操作符](https://www.w3schools.com/js/js_comparisons.asp)，比如 *==* 或者 *===* 。

这里有一种方法可以纠正我们代码中的错误。让我们给变量 *a* 赋值，然后在三元运算符的条件中使用比较运算符。

```
var a = 3;var b = (a === 3) ? true : false
```

您将选择的比较运算符当然取决于您试图确定的内容，例如 *a* 是否等于 3、大于 3、小于 3、小于或等于 3，等等。无论如何，评估题的正确答案是“三元语句中的条件是使用赋值运算符。”

更多即将推出！