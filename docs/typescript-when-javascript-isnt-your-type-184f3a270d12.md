# 类型脚本——当 JavaScript 不是你喜欢的类型时

> 原文：<https://medium.com/nerd-for-tech/typescript-when-javascript-isnt-your-type-184f3a270d12?source=collection_archive---------11----------------------->

![](img/45ac40a2c224ffb57000a0c99a2fcf84.png)

马库斯·温克勒在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

## 什么是 TypeScript？

简而言之，TypeScript 就是带有类型的 JavaScript。JavaScript 是一种动态类型语言，正因为如此，运行时的行为无法很好地预测，可能会导致错误。TypeScript 通过其严格的类型检查和赋值来解决这个问题。

希望这篇文章能让你掌握 TypeScript 的基础知识。

## 为变量分配类型

让我们从最简单的 TypeScript 代码示例开始。

```
const message:string = "hello world!";
console.log(message);
```

在上面的代码中，我们给*常量消息*分配了*字符串*类型。

与上面的代码类似，其他类型如*数字、布尔、符号、对象、未定义、never、any、*和 *unknown* 可以赋给 TypeScript 中的变量。

将类型赋给变量的一般语法是

```
const/let/var varName:type;
```

其工作原理是，如果你试图给*消息分配一个字符串以外的任何东西，* TS 将抛出一个错误。

## 推断类型

你并不总是需要在 TS 中显式地分配类型。有时候还是让 TS 来推断类型比较好。当您完全确定要传递的文字的类型时，这很有帮助。

```
// TS infers num as a number
const num = 1000;// TS infers toggle as a boolean
const toggle = true;// TS infers str as a string
const str = "I am a string";
```

## 任何的

TS 中有一个类型叫做 any，它的行为类似于普通的 JS 变量。它可以被赋予任何东西而不用担心类型，尽管这违背了 ts 的全部目的。只有当你完全不知道要分配什么类型的时候，才使用这个。

```
let anyThing:any = 1000
anyThing = "Hello"
anyThing = true
console.log(anyThing) // true
```

上述代码不会抛出任何错误，并且*any*的最终值将为 *true。*

## 类型联合

当你想让一个变量在两个或多个类型之间灵活转换时，union 就派上了用场。

```
let strOrNum:string | number;strOrNum = "I am a string"; // correct
console.log(strOrNum) // I am a stringstrOrNum = 111; // correct
console.log(strOrNum) // 111strOrNum = true; // error
console.log(strOrNum) // :(
```

在上面的例子中，我们使用一个|(管道操作符)创建了一个联合。通过这种方式，可以将多个类型链接在一起以创建类型的联合。

注意，在这个例子中，当你在联合类型之外赋值时，TS 将抛出一个错误。因此，只能将一个*字符串*或一个*数字*分配给*stronum。*

## 文字类型

甚至像字符串、数字等文字类型也可以作为类型赋给变量。

```
let lit:"abc" | "def" | 100;lit = "abc" // correctlit = "def" // correctlit = 100 // correctlit = 1000 /* Type '1000' is not assignable to type '"abc" | "def" | 100'. */lit = "random" /* Type 'random' is not assignable to type '"abc" | "def" | 100'. */
```

在上面的例子中，我们定义了一个只能用给定的文字赋值的变量。试图分配任何其他文字将抛出一个错误。当您希望将变量限制在几个值内时，TS 的这一特性非常有用。

## 键入别名

可以使用关键字 *type 创建类型的类型别名。*

```
type CustomUnion = string | number | boolean | undefined;let something:CustomUnion;something = "I am a string"; // correctsomething = 99; // correctsomething = false; // correctsomething = undefined; // correctsomething = [1, 2, 3]; // in-correct
```

另一个对象类型的例子。

```
type Student = {"firstname":string, "lastname":string}// correct
let student1:Student = {
    "firstname": "Johan",
    "lastname": "Liebert",
}// incorrect, "prefix" does not exist in type Student
let student2:Student = {
    "prefix": "Mrs."
    "firstname": "Anna",
    "lastname": "Liebert",
}// incorrect, "lastname" property is required
let student3:Student = {
    "firstname": "Kenzou",
}
```

## 用接口定义类型

也可以使用接口创建类型。下面的代码与上面的对象类型代码非常相似，并产生相同的输出

```
interface Student {
    "firstname":string, 
    "lastname":string
}// correct
let student1:Student = {
    "firstname": "Johan",
    "lastname": "Liebert",
}// incorrect, "prefix" does not exist in type Student
let student2:Student = {
    "prefix": "Mrs."
    "firstname": "Anna",
    "lastname": "Liebert",
}// incorrect, "lastname" property is required
let student3:Student = {
    "firstname": "Kenzou",
}
```

## 函数中的类型

函数中的参数也可以使用前面看到的语法在 TS 中赋值。

```
// expects two numbers
function addNums(num1:number, num2:number) {
    console.log(num1 + num2);
}// correct
addNums(10, 20)// in-correct
addNums(10, "20")// in-correct
addNums("10")// in-correct
addNums("10", 20n)
```

类似地，返回类型也可以分配给函数。

```
// can only return a string
function concatAndUpper(s1:string, s2:string):string {
    s1 += s2;
    return s1.toUpperCase();
}// error, expected to return string but returned number
function concatAndUpper(s1:string, s2:string):string {
    s1 += s2;
    return 100;
}
```

然而回调有一点不同的语法，回调的类型是用箭头语法指定的。

```
function funInFun(
  // defining types and return type for callback
  callback:(n1: number, n2: number) => number, 
  other: any
):number {
    return callback(10, 20);
}function callback(n1: number, n2:number) {
    return n1 + n2
}console.log(funInFun(callback, "dummy"))
```

## 通用函数

有时，如果你知道函数中变量之间的关系，你可以给它们分配泛型类型。

```
/* "Type" is just a placeholder, any name can be used instead of Type */
function fun<Type>(arrOrString: Type[]|Type, index: number): Type {
    return arrOrString[index]
}/* we pass the actual value of "Type" with the function call */ console.log(fun<number>([2, 4, 6, 8], 2))/* same as above but passing *string* as a type this time */
console.log(fun<string>("StrString", 4))
```

这种语法在创建数组、地图等对象时会经常出现

```
// only takes numeric values
const arr = new Array<number>();
```

## 缩小

有时，根据变量的类型对变量执行一些操作变得很重要。幸运的是，TS/JS 有 *typeof* 操作符来检查变量的类型。

现在，缩小基本上是使用的*类型通过排除过滤掉类型*

```
/* takes a string, number, number array or undefined */
function fun(x: string | number | number[] | undefined) {
    if(typeof x === 'string') {
        x.toUpperCase();
    }
    else if(typeof x === 'number') {
        x = 10 * x
    }
    else if(typeof x === 'number[]') {
        x[0] = x[0] + x[1]
    }
    else {
        console.log("undefined :(")
    }
}
```

这是所有的乡亲。以上是让您开始使用 TypeScript 的介绍。如果您想阅读更多关于 TS 的深入信息，请阅读 TS 文档，下一期再见。