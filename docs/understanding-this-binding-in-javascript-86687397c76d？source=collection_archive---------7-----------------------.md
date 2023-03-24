# 理解 Javascript 中的“this”绑定

> 原文：<https://medium.com/nerd-for-tech/understanding-this-binding-in-javascript-86687397c76d?source=collection_archive---------7----------------------->

为了清楚地理解“this”关键字，我们首先需要了解执行上下文是如何工作的。每次运行 Javascript 代码时，引擎都会创建一个全局执行上下文。每次调用一个函数时，都会为该函数创建一个全新的本地执行上下文。每个函数都有自己的执行上下文，但是它是在调用函数时创建的。一个程序中只能有一个全局执行上下文，可以有任意数量的局部执行上下文。

**执行上下文是什么样子的？**

执行上下文在创建阶段创建。在创建阶段会发生以下事情:

1.  创建词汇环境组件。
2.  VariableEnvironment 组件已创建。

**词法环境是什么样子的？**

让我们看下面的例子来理解词汇环境:

```
const person = {
  name: 'yasemin',
  birthYear: 1991,
  calcAge: function() {
    console.log(2018 - this.birthYear);
  }
}
person.calcAge();
const calculateAge = person.calcAge;
calculateAge(); 
```

下面的代码片段展示了词法环境在概念上的样子。

```
GlobalExecutionContext = {
  LexicalEnvironment: {
    EnvironmentRecord: {
      Type: "Object",
      // Identifier bindings go here
    }
    outer: < null >,
    this: < global object >
  }
}
FunctionExecutionContext = {
   LexicalEnvironment: {
      EnvironmentRecord: {
        Type: "Declarative",
        // Identifier bindings go here
      }
      outer: < Global or outer function environment reference>,
      this: depends on how function is called
    }
}
```

正如您在上面的例子中看到的，每个词汇环境都有三个组成部分:

1.  环境记录
2.  参考外部环境，
3.  **本绑定**

在这篇文章中，我们将解决“这个”绑定。

**什么是“这个”绑定？**

正如您在上面的示例中看到的，“this”绑定是执行上下文(全局、函数或 eval)的一个属性。它的行为取决于执行上下文。

**全局执行上下文:**

*   在全局执行上下文中，“this”的值指的是全局对象。(在浏览器中，这是指窗口对象)。

**功能执行上下文:**

*   在函数执行上下文中，“this”的值取决于如何调用函数。每个函数调用都定义了自己的上下文，因此,“this”的行为可能与您预期的不同。

在 javascript 中，有很多方法可以调用函数。让我们看看它们是什么。

**默认绑定:**

如果不使用任何其他类型的绑定，就会发生默认绑定。如果我们调用任何只有括号的函数，我们将得到默认绑定。它以不同的方式表现“严格”模式。

在非严格模式下，如下调用函数时，`this`引用全局对象:

```
function show() {   
 console.log(this === window); *// true* }  
show();
```

在严格模式下，Javascript 将`this`设置为`undefined`。请注意，从 ECMAScript 5.1 开始，严格模式就可用了。`strict`模式适用于函数和函数内的内部函数。

```
function show() {  
   "use strict";  
   console.log(this === undefined); *// true*    
   function display() {    
      console.log(this === undefined); *// true*   
   }   
   bar();
 } 
 show();
```

**隐式绑定:**

隐式绑定是在调用带有“.”的函数时发生的情况在它之前。换句话说，它在调用一个对象的方法。

```
const obj = {
  foo: function() {
    console.log(this); // {foo: f} which is basically obj.
  },
};

obj.foo();
```

你也可以把它存储在一个变量中，通过变量调用这个方法。

```
const obj = {
  name: 'Obj',  
  foo: function() {
    console.log(this.name); // undefined
  },
};

const newObj = obj.foo();
newObj.foo()
```

它记录了`undefined`,因为当你调用一个方法而没有指定它的对象时，JavaScript 在非严格模式下将`this`设置为全局对象，在严格模式下将`undefined`设置为全局对象。

注意:要解决这个问题，您可以使用`bind`方法。我们稍后会看一看它。

**新绑定:**

新绑定是当你使用`new`关键字创建一个函数对象的实例时发生的事情，你使用函数作为构造函数。调用函数时可以使用`new`，比如:`new foo()`

`new`做 4 件事:

1.  它创建一个新的空对象。
2.  它使`this`成为新的对象。
3.  它使`foo.prototype`成为对象的原型。
4.  如果函数没有返回任何东西，它将隐式返回`this`。

```
function foo() {
  console.log(this); // outputs an empty object
}

new foo();
```

**显式绑定:**

在 JavaScript 中，函数是一等公民。换句话说，函数是对象，是函数类型的实例，它有两个方法:`[call()](https://www.javascripttutorial.net/javascript-call/)`和`[apply()](https://www.javascripttutorial.net/javascript-apply-method/)`。如果您使用函数对象中的三个函数`call`、`apply`或`bind`之一调用一个函数，就会发生显式绑定。

*   **调用**:它接受逗号分隔的附加参数。它们将被传递给函数调用。`foo.call(obj, argument1, argument2)`
*   **apply** :它与`call`非常相似，唯一的区别是它接受数组中的参数。`foo.apply(obj, [argument1, argument2])`

```
function Product(name, price) {
  this.name = name;
  this.price = price;
}

function Food(name, price) {
  Product.call(this, name, price);
  this.category = 'food';
}

function Toy(name, price) {
  Product.call(this, name, price);
  this.category = 'toy';
}

const cheese = new Food('feta', 5);
const fun = new Toy('robot', 40);
console.log(`${cheese.name} is ${cheese.category}`);//feta is a food
console.log(`${fun.name} is ${fun.category}`); //robot is a fun
```

*   **bind** :返回一个新的 [f](https://www.javascripttutorial.net/javascript-function/) 函数，当被调用时，其`[this](https://www.javascripttutorial.net/javascript-this/)`设置为一个特定值。`foo.bind(thisArg[,arg1[,arg2[,..]]])`与`[call()](https://www.javascripttutorial.net/javascript-call/)`和`[apply()](https://www.javascripttutorial.net/javascript-apply-method/)`方法不同，`bind()`方法不会立即执行函数。它只是返回一个新版本的函数，其`this`设置为`thisArg`参数。当你把一个方法对象作为回调传递给另一个函数时，`this`就丢失了。例如:

```
let person = { 
  name: ‘John Doe’,
  getName: function() {
   console.log(this.name); // undefined
  }
 };
setTimeout(person.getName, 1000);
```

它可以改写如下:

```
let f = person.getName; 
setTimeout(f, 1000); *// lost person context*
```

将方法用作回调意味着用默认绑定调用它们。(我们已经在默认绑定范围中讨论过了)。因此，当回调`person.getName`被调用时，全局对象中不存在`name`，它被设置为`undefined`。有很多方法可以解决这个问题:

1.用另一个函数
2 把它包起来。使用`bind`方法

```
let f = person.getName.bind(person); 
setTimeout(f, 1000);
```

除了`bind`方法的这个好处之外。我们也可以利用`bind`方法从不同的对象借用方法，而不像`call`和`apply`那样复制那个方法。

```
const person1 = {
name: "John",
age: 15,
displayAge: function(){
  console.log("He is " + this.age + " years old");
 }
};
person1.displayAge(); /*Output: He is 15 years old*/
const person2 = {
name: "Mike",
age: 20
};person1.displayAge.call(person2); //Output: He is 20 years oldconst displayAge = person1.displayAge.bind(person2)
displayAge(); //Output: He is 20 years oldperson1.displayAge.apply(person2); //Output: He is 20 years old
```

**箭头功能:**

arrow 函数不创建自己的执行上下文，而是从定义 arrow 函数的外部函数继承`this`。请参见以下示例:

```
const obj = {
  foo:() => console.log(this) //window object depends on strict mode  
};

obj.foo();
```

这是一个使用箭头功能和`new`绑定的例子。

```
function Person() {
  const foo = () => console.log(this.name); // Yasemin.
  this.name = "yasemin";
}

const person = new Person();
person.foo();
```

我们叫`new Person()`。这将创建一个新的空对象，并将其绑定为`this`的值。当我们调用 foo 的方法时，`this`当前指向的是`new`创建的对象。