# ES6，如果你是一个 JavaScript 开发者的话，这很棒！

> 原文：<https://medium.com/nerd-for-tech/es6-and-its-beauty-if-you-are-a-javascript-developer-29c925a30fb5?source=collection_archive---------11----------------------->

![](img/081badc0f60a722ef483e2d1f473bd56.png)

“欧洲计算机制造商协会脚本”——你熟悉这个名字吗？简而言之，它被称为 JavaScript，我们钟爱的 JavaScript 就是基于它。不过 ES6 并不是最新的，2016 年之后开始按年份命名(比如 ESMAScript 2017/2018 等)。

ES6 于 2015 年发布。在这个版本中，一系列新特性也开始出现。在这篇博客中，我们将尝试学习其中的一些。

# **1。新类型的声明！**

通常我们用 var 关键字来声明一个变量。但是它有一些问题，例如

*   var 可以用相同的名称声明多次。
*   默认情况下，var 是全局的。
*   循环中的变量使用相同的引用。
*   用 var 声明的变量甚至可以在声明之前使用。

为了解决这些问题，ES6 配备了**常量**和**常量**。两者都有其独特性。

const 不能在同一个变量名中使用两次，除非你在不同的不可访问的作用域中使用它。比如你可以在箭头函数中使用它。让我给你看一个例子——

```
const add = (x, y) => {
               const result = x+y;
               console.log(result);       // 10
               }const sub = (x, y) => {
               const result = x+y;
               console.log(result);       // 2
               }add(6, 4);
sub(6, 4);
```

如你所见，用 **const** 声明的*结果*变量可以在不同的范围内使用。但是如果你试图在第一次声明它的同一个函数中重新初始化它，你会得到一个错误，你也可以试试。 ***常量*** *可以重复使用，但不能重新初始化*！

如果您想重新初始化或在声明时不确定是否需要重新初始化， **let** 是您的解决方案。让给你一个机会来声明一个可以被重新初始化和重用的变量。

```
let num = 6;
const example = () => {
               num = 10;
               const result = num*5;
               console.log(result);         // 50example();
```

**2。箭头功能！**

虽然我们在上一个例子中展示了一个关于箭头函数的例子，但是现在让我们来讨论一下。通常在声明一个函数的时候，你必须先命名它，为了得到一个返回类型，你也必须指定它。但是箭头函数帮助你用更少的代码以更简单的方式工作。让我们来看一个代码示例，*常规函数* vs *箭头函数* —

```
//regular function exampleconst add = function (x, y) {
                 return x+y;
                 }
```

同样的带箭头功能的代码——

```
const add = (x, y) => x+y;
```

**3。新的循环，更有趣！**

随着 ES6 的出现，出现了新的回路类型，即的**和**的**。这有助于您遍历可迭代对象。现在哪一个用在哪里有很大的区别。**循环中的**for/用于迭代可迭代对象(数组、字符串)的*值*，而**循环中的**for/用于迭代对象的*键* s。**

让我们来看一个例子

```
// example of for/of loopconst students = ['John', 'Sara', 'Jack'];

for ( let element of students ) {
    console.log(element);
}// output
// John
// Sara
// Jack// example of for/inconst employee = {
    name: 'Richard',
    department: 'web',
    age: 23
}

for ( let key in student ) {    
    console.log(`${key} : ${employee[key]}`);
}// output
// name : Richard
// department : web
// age => 23
```

**4。破坏！**

ES6 引入了数据析构，有助于从数组和对象中解包值。

```
// object destructuringconst champion = { Male: 'Bran', Female: 'Emma' };
const {Male: x, Female: y} = obj;
    *// x = 'Bran'; y = 'Emma'*

const {Male, Female} = obj;
    *//* Male: 'Bran', Female: 'Emma' // array destructuringconst alphabet = ['a', 'b'];
const [x, y] = alphabet;
    *// x = 'a'; y = 'b'*
```

**5。默认参数值**

ES6 允许您提供一个默认参数值，如果没有给出的话。这很简单，让我们看一个例子—

```
const aFunc = (x, y=0) => x+y;console.log(aFunc(5));    //5
console.log(aFunc(5, 10));    //15
```

**6。展开元素**

假设你有一个包含 100 个元素的数组，你想把它复制到一个新的数组，你会怎么做呢？通常我们使用 loop。但是现在有一个新的 spread 运算符(…)可以帮助我们解决这个问题。例子—

```
const arr1 = [0, 1, 2, 3];//using spread to operator to copy it in new arrayconst arr2 = [...arr1];
console.log(arr2);      //[0, 1, 2, 3]
```

假设你想在一个新数组的末尾添加另一个元素

```
const arr2 = [...arr1, 9];
console.log(arr2);      //[0, 1, 2, 3, 9]
```

**7。承诺**

Promise 是一个对象，它帮助你处理来自请求的响应，有成功或拒绝。通常代码语法是这样的—

```
examplePromise.then (
function(params) {code for successful request}
function(params) {code for failed request}
)
```

**8。查找方法**

如果你是 JavaScript 初学者，find() 方法是非常必要的。它帮助你从数组中找到一个元素，并返回它得到的第一个值/元素。

```
const arr = ['java', 'python', 'c', 'c++'];const element = arr.find(language => language === 'java');console.log(element);          //java
```

find 方法还有一个比较特别的地方，你也可以通过 **findindex()** 方法来搜索想要的元素的索引。

```
const arr = [5, 9, 11, 17, 21];const larger = (element) => element > 9;console.log(arr.findIndex(larger));  // expected output: 2
```

**9。班级！**

在 ES6 之前，人们需要创建函数来像类一样工作。但是，使用 ES6，可以创建类。OOP 是一个值得探讨的巨大领域。我简单说一下。

类是一个模板，我们以后可以用它来创建对象。在类中，我们可以使用 constructor，这是一个默认的方法，无论何时调用该类都会调用它。让我们看一个例子——

```
class Member {
      constructor(name, phone, email) {
                  this.name = name;
                  this.phone = phone;
                  this.email = email;
       }
}const monty = new Member('Monty', 7881592856, 'monty121@me.com')console.log(monty.name);        // "Monty"
```

可以添加方法(类中的函数称为方法)并轻松调用。例子—

```
class Member {
      constructor(name, phone, email, age) {
                  this.name = name;
                  this.phone = phone;
                  this.email = email;
                  this.age = age;
       }
    addAge() {
          this.age +=1;
        }
}const monty = new Member('Monty', 7881592856, '[monty121@me.com](mailto:monty121@me.com)', 31)
console.log(monty.age);        // 31
monty.addAge();
console.log(monty.age);        // 32
```

**10。新的全局方法**

也许这并不重要，但有时它会派上用场。

**10.1 isFinite()**

它根据答案是无穷大/NaN 还是相反返回 true 或 false。

```
let a = 'a';
console.log(isFinite(a/1));           //false
console.log(isFinite(10/0));         //false
console.log(isFinite(10/1));         //true
```

**10.2 伊斯南()**

和前面的差不多，它告诉我们给定的值是否是一个数字。

```
console.log(isNaN("Hello"));       // returns true
console.log(isNaN("10"));          // returns false
console.log(isNaN(10));           // returns false
```

这个博客到此为止。如果我做错了什么，请在评论框中告诉我。