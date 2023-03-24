# JavaScript —变量、模板文字、For & While 循环

> 原文：<https://medium.com/nerd-for-tech/javascript-variables-template-literals-for-while-loops-236445cabf74?source=collection_archive---------13----------------------->

![](img/fa277b02e9665efea021f5c633ed191a.png)

你好！我很兴奋能写出我的第一个媒体故事！😃我将于三月份开始我的 Wecode 训练营计划(位于韩国首尔)。我们已经从上周开始了预备课程。这是分享我的学习历程，记录我所学到的东西。我写这个故事时使用了免费 Udemy 讲座和 Codecademy 中讨论的概念和定义。

在 Wecode 预备课程的第二周，我们有六个 JavaScript 任务要完成。在这个故事中，我写的是前三个任务，以及我在 Udemy 上的 JavaScript Essentials 讲座中完成的一个迷你项目。我真的很喜欢劳伦斯·图尔顿的这个讲座！他对每个概念都给出了非常透彻和详细的解释。讲座链接在这里:

[](https://www.udemy.com/course/javascript-essentials/) [## 免费 JavaScript 教程- Javascript 基础

### 讲座很容易理解。我现在完全明白我错过了什么，什么让我困惑。我一直…

www.udemy.com](https://www.udemy.com/course/javascript-essentials/) 

# 任务一。使用 var，let，const 声明变量并讨论区别。

**var** 用于声明一个变量。变量是一个包含值的盒子，您可以用它来获取内存中的数据。 **var** 可以改变数值。JavaScript 是一种松散类型的语言，这意味着类型化的数据可以重新分配和改变类型(字符串、数字、布尔值、对象、数组……)。

const 是另一种类型的盒子，它在你定义的程序执行的整个生命周期中保持不变。 **const** 不能在没有赋值的情况下创建——不能未定义，所以出于安全原因，它更好。

**让**在 ES6 中推出。 **let** 关键字也可以像 **var** 一样被重新赋值。也可以声明为未定义。

let 和 var 的**区别在于 **let、**以及 **const 在**条件执行上下文**中声明时，会考虑它们所在的**范围。 **var** 不尊重。**

示例:

```
*If (true){
var symbolName = “value reference in memory”;
}*
```

if 语句中的这个 **var** 可以从**全局作用域**中取出，尽管它是在执行上下文中声明的(不过在函数中， **var** 尊重作用域)。

## 那用什么呢？

最好尽可能使用 **let** ，因为它尊重条件执行上下文的范围，这样可以避免可能的冲突。

# 任务二。使用模板文本编写变量和字符串。

我们可以使用模板文字**将**变量插入到字符串**中。**

一个**模板文字**被反斜杠(`)包围。变量放在${}中，然后变量的值被插入到模板文本中。

示例:

```
*let myName = ‘Jessica’;
let myCity = ‘Seoul’;
console.log(`My name is ${myName}. My favorite city is ${myCity}.`)*
```

上面的日志‘我的名字是杰西卡。我最喜欢的城市是首尔。到控制台。

# 任务三。编写 for 和 while 循环语句。

循环是一种编程工具，它重复一组指令，直到达到指定的条件，称为停止条件。

## 使用什么，for 循环还是 while 循环？

当我们知道循环应该运行多少次时， **for** 循环是理想的。然而，我们并不总是事先知道这一点。例如，你饿了，开始吃东西，你不知道你需要多少才能吃饱。相反，你会在饿的时候吃。因此，在我们希望一个循环执行不确定次数的情况下， **while** 循环是最佳选择。换句话说，循环的**用于执行循环**特定次数。**一个 **while** 循环用于只要满足某个条件就执行循环**——没有具体次数。****

另外，请注意，我们必须在**中为**循环创建一个新变量，但是您不必为**和**循环创建新变量。如果您想使用现有变量创建一个循环语句，那么使用 **while** 循环可能更好。

**For 循环示例:**

```
for (let counter = 0; counter < 4; counter++) {
 console.log(counter);
}
```

**While 循环示例:**

```
*const cards = [‘diamond’, ‘spade’, ‘heart’, ‘club’];
let currentCard;**while (currentCard != ‘spade’) {
currentCard = cards[Math.floor(Math.random()*4)];
console.log(currentCard);
}*
```

这里使用了 while 循环，因为我们不知道它将迭代多少次，直到 currentCard 被设置为' spade '。

# 迷你项目

我完成了一个来自 Udemy 上的 JavaScript 入门讲座的迷你项目，上面提到过。这个项目是做一个盒子创作者。有三个带有下拉框的选择器，你可以选择背景、宽度和高度。单击设置按钮后，所选选项将被应用，并显示带有已更改属性的框。

我使用了 **let 声明、文档方法、事件、函数和 for 循环。**该项目的 github 链接如下:

[https://github.com/jessywlee/boxcreator-js](https://github.com/jessywlee/boxcreator-js)