# 如何在 JavaScript 中复制对象？

> 原文：<https://medium.com/nerd-for-tech/how-to-copy-objects-in-javascript-4a44740b04a5?source=collection_archive---------8----------------------->

如果您正在使用 JavaScript，那么处理对象总是很困难。作为一个 JS 初学者，我在对象方面面临了很多挑战，但现在我对此有了更好的理解，我想和大家分享一下。

![](img/0fdde0c5543c9946ee7ddef5cbbc1389.png)

> 这里要理解的最基本的东西是基本类型和对象之间的区别。像字符串、数字、布尔值等基本类型总是作为一个值被复制，而对象则不同，因为它们是通过引用被复制和存储的。

这是什么意思？我们举个例子来了解一下基本的区别。

```
let foo = "Hello String";
let bar = foo;
bar = "Bye String";
console.log(foo);  // "Hello String"
```

你看到刚才发生了什么吗？让我为你打破它。

当您将 foo 赋值给 bar 时，它会为整个值创建一个单独的副本。所以当我们给 bar 赋值时，它不会影响 foo 值，因为原语不使用引用。

而与对象完全不同。

## 对象是引用类型

> 分配给对象存储的变量，不是对象本身，而是它的“内存地址”——换句话说，是对它的“引用”。

让我们通过例子来理解它。

```
let person = { name: 'Sarfraz' };
let person2 = person;
person.name = 'John';
console.log(person2.name); // "John" changes from person refernce
```

如您所见，这里只有一个对象，有两个引用指向它。因此，复制一个对象变量会创建一个对同一个对象的引用。

但是如果我们需要复制一个对象呢？这是可以做到的，但是 JavaScript 中没有内置的方法。让我们看看复制对象的三种方法。

## 使用 Spread (…)语法

Spread 操作符克隆对象及其属性。请记住，这是一个浅层复制，这意味着只有第一级原语字段通过值进行复制，其余的嵌套对象通过引用进行复制。

```
const dishes = { chicken: '🍗', pizza: '🍕' };
const cloneDish = { ...dishes };console.log(cloneDish); 
// { chicken: '🍗', pizza: '🍕' }
```

## 使用 Object.assign()方法

Object.assign()方法用于将所有可枚举的自身属性的值从一个或多个源对象复制到目标对象。

```
let obj = { a: 1, b: 2, };
let objCopy = Object.assign({}, obj);console.log(objCopy); 
// Result - { a: 1, b: 2 }
```

> Spread (…)语法和 Object.assign 有一个问题，它们只做浅层复制。这意味着嵌套属性仍将通过引用进行复制。使用时要小心。

# 深层复制对象

深层拷贝将复制它遇到的每个对象。副本和原始对象不会共享任何东西，所以它将是原始对象的副本。这解决了我们在上述两种方法中面临的问题。

## 使用 JSON . parse(JSON . stringify(object))

```
let obj = 
  {
    foo: 1,
    bar: {
         foobar: 2,
       }
   }let newObj = JSON.parse(JSON.stringify(obj));obj.bar.foobar = 20;
console.log(obj); 
// { foo: 1, bar: { foobar: 20 } }
console.log(newObj); 
//{ foo: 1, bar: { foobar: 2 }} (New Object with Original Values)
```

这种方法有一些缺陷。它不能用于复制用户定义的对象方法，并且您必须确保源对象是 JSON 安全的。

## 使用 _。cloneDeep()方法

_。cloneDeep()方法用于创建值的深层副本，即递归克隆值。

```
const _ = require('lodash');
var obj = { x: 23 };
var deepCopy = _.cloneDeep(obj);
obj.x = 10; // Changing original valueconsole.log(obj.x); //10
console.log(deepCopy); //23
```

因为这是一个深层拷贝，你不会有任何原始对象的痕迹。

# 结论

在 JS 中复制对象可能很棘手，尤其是如果您是 Javascript 新手的话。希望这篇文章能帮助你理解工作原理，避免以后可能会遇到的问题。如果您有任何建议或任何更好的方法来实现这一点，欢迎指出来，我会更新它。编码快乐！