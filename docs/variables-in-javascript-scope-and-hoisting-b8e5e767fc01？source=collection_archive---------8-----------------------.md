# JavaScript 中的变量、范围和提升

> 原文：<https://medium.com/nerd-for-tech/variables-in-javascript-scope-and-hoisting-b8e5e767fc01?source=collection_archive---------8----------------------->

![](img/fbf4e98c522f4ad7e488531b02852123.png)

图片来源:Unsplash

变量是任何编程语言的基础和最重要的部分。它们用于存储在程序的进一步执行中使用的值。

你可以把变量想象成一个盒子，我们可以在里面储存一些东西，比如值。

在 JavaScript 中，变量可以存储任何类型的值。它可以是数字、字符串、布尔值、数组、对象等等。我不想在本文中讨论数据类型，我们将在另一篇文章中讨论它们。让我们只关注一个变量。

> **让我们看看如何在 JavaScript 中使用变量:**

1.  声明一个变量
2.  在其中赋值
3.  使用它

就这么简单。

> **来点实际的:**

```
**var x;**            */Declare a variable***x=10;  **           */Assign a value in it***console.log(x);**   */Use it*
```

在 ES6 之前使用 ***var*** 作为变量关键字，但是在 ES6 之后有两个新的关键字用于分配变量 ***let*** 和 ***const。***

但是为什么 ***让*** 和 ***保持不变呢？***

要理解 ***让*** 和 ***const*** 的重要性，首先我们需要了解两个 JavaScript 特性:

## 范围和提升

# **我们先来讨论一下作用域:**

## 作用域仅仅意味着变量在什么范围内可以被访问。

在 JavaScript 中，有两种类型作用域:

1.  全球范围
2.  局部范围

> 没明白吗？

好的，别担心。让我们实际地做它。考虑以下代码:

```
var global = 'i am a global variable';
function doSomething() {                
  var local = 'i am a local variable';  
  console.log(local);                   
}                                       
console.log(global);
console.log(local);
```

**输出:**

```
somethingReferenceError: local is not defined
    at Object.<anonymous> (C:\Users\user\Desktop\demo\app.js:7:13)
```

> *什么是*参考错误*？*

如果你在上面的程序中看到，我声明了两个变量 ***【全局】*** 和 ***局部。***

**局部**变量在 **doSomething** 函数中，所以你不能在函数外访问它。这意味着变量 local 的范围在函数内，即**局部范围。**

但是变量 **global** 是在函数外部声明的，所以你可以从任何地方访问它。因此变量 global 在**全局范围内。**

在 ES6 之后，本地范围被进一步分成两部分:

1.  **var** 的功能范围(功能)
2.  **的阻塞范围(条件或循环)让**和**保持不变**

> **看看下面的代码:**

```
function doSomething() {
  if (1<2) {
    var cow = 'cow';
    let dog = 'dog';
    const cat = 'cat'; console.log(cow);   //cow
    console.log(dog);   //dog
    console.log(cat);   //cat
  }
  console.log(cow);     //cow
  console.log(dog);     //ReferenceError: dog is not defined
  console.log(cat);     //ReferenceError: cat is not defined
}
doSomething();
```

如你所见，如果我们试图在 **if** (块范围)之外访问 **let** 和 **const** 变量，它会给出 **ReferenceError** 。然而 **var** 变量在**函数范围内完美地完成了它的工作。**

也就是说， **var** 的范围是**函数范围**其中 **let** 和 **const** 的范围是**块范围。**

# **我们先来讨论一下吊装:**

***定义*** :当一个变量或函数被声明时，它的声明被移动到它们作用域的顶部。

> 见鬼了。

> **我们来简化一下:**

看看下面的情况；

1.  **试图在变量被声明和初始化之前访问该变量**

```
console.log(name);  //access name before it defined or initialized
var name='person';  //define and initialize after it been accessed/* Output */
undefined
```

2.**试图在变量初始化之前访问变量，但没有声明它**

```
console.log(name);  //access name before it defined or initialized
name='person';      //initialize name without it defined/* Output */
ReferenceError: name is not defined
```

正如我们所看到的，如果我们在变量被声明和初始化之前访问它，它会返回未定义。然而，如果我们*在变量初始化之前访问它而没有声明它*，它会返回一个 ReferenceError。

在**第二个条件**中，我们在访问变量 ***名称*** 之前似乎没有声明它，所以它给出了一个 ReferenceError，但是在**第一个条件**中发生的事情是，JavaScript 在访问变量**名称**之前自动声明了变量**名称**，因为我们在变量前面放了一个 ***var*** 关键字。

```
//How we write it       |     //How JavaScirpt Manipulate it
console.log(name);      |     var name;
var name='person';      |     console.log(name);
                        |     name='person';/* Output */
undefined
```

> 让我们来看看关于吊装的大图:

```
var statement = true;
function checkHoisting() {
  //var statement;  /* Javascript automatically declared it here */
  if(1>2){
    var statement = false;
  }
  console.log(statement);
}
checkHoisting();//Output: undefined
```

看到这个例子，人们可以很容易地预测输出应该是*真*。但是由于**提升**属性 JavaScript 声明了一个*新的*语句变量 top 在**检查提升**函数之上，该函数未初始化，因此输出为**未定义**。

这种类型的输出可能会导致奇怪的错误。

## 但是在 let 或 const 的情况下，这种情况根本不会发生。让我们看看

```
let statement = true;
function checkHoisting() {
  if(1>2){
    let statement = false;
  }
  console.log(statement);   //the global statement variable
}
checkHoisting();
//Output: true
```

Let 和 Const 不参与提升行为，因为它们是块范围变量。

> 让我们看看另一个场景:

```
var statement = true;
var statement = false;
console.log(statement); // Output:falselet done = true;
let done = false;
console.log(done);      //Output:SyntaxError: Identifier 'done' has     already been declared
```

这里发生了什么？你能猜到原因吗？

我来简化一下。

当我们用不同的值用 var 再次声明一个变量时，那么由于提升行为，这个变量的值用最新的值更新了，因而输出是 false。

但是在 **let** 和 **const** 的情况下，由于它们没有遵循提升属性，它抛出一个 **SyntaxError** *表示标识符‘done’已经被声明。*

这种变量重复也会导致错误。

# 结论:

由于范围和提升的原因， **var** 关键字可能会导致 w 不希望出现的不可预测的结果。所以根据 **ES6** 的特性，最好使用 **let** 和 **const** 来代替 var，以减少我们代码的混乱和错误。

也就是说，这就是这篇文章的全部内容。我希望这篇文章可以帮助你理解 JavaScript 中的变量的作用域和提升属性。

随意评论。

到时候见。