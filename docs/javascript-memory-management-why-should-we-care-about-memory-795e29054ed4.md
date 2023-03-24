# Javascript 内存管理——我们为什么要关心内存？

> 原文：<https://medium.com/nerd-for-tech/javascript-memory-management-why-should-we-care-about-memory-795e29054ed4?source=collection_archive---------15----------------------->

![](img/3713e1263ebe7f981f54073e6a44f108.png)

照片由[乔丹·哈里森](https://unsplash.com/@jordanharrison?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

作为一名 Javascript 开发人员，我们通常专注于编写越来越多的代码，并尽快完成。但是在更快地写代码的过程中，有时我们会让程序比我们预期的要慢。其中一个原因是我们在写代码的时候并不关心内存。我们中的许多人在高端设备上运行程序，这些设备拥有最新的 GPU、CPU 和像超级计算机一样的计算能力。所以我们实际上并没有发现我们的程序变慢了。但最终用户的情况并非如此。我们的用户可能在一台 10 年前的设备上运行这个程序，它不具备开发人员的计算机所拥有的所有超级能力，所以他们在运行我们的网站并定期抱怨客户支持后会感到沮丧。并且在最坏的情况下，用户可能会完全停止使用该产品。

> “不是所有的用户都有顶级设备来运行我们编写的网站或应用程序。因此，我们应该同样关注让他们的应用程序更快，并为所有用户提供几乎相同的体验。”

# 概观

现在你可能会想，好吧，这里有很多 Jagran。现在告诉我我们怎样才能提高性能。你们中的一些人可能会想*“为什么我们需要考虑内存，我们在 Javascript 中有* ***垃圾收集器*******(GC)****。”但是这正是我们犯下大错的地方。**

*即使 Javascript 有一个垃圾收集器来处理不再需要的内存。不过，我们应该知道内存管理和垃圾收集器是如何工作的，这样我们就可以避免编写内存泄漏的程序，并且可以清理不再需要的变量。*

# *内存生命周期*

*在所有的编程语言中都有一个内存生命周期，从内存被分配到被释放。*

*   *内存分配*
*   *内存的使用*
*   *内存释放*

## *内存分配:*

*需要时，操作系统会为程序分配内存。在像 **C** 这样的低级语言中，程序员必须明确地告诉操作系统使用`malloc`或`calloc`来分配内存。但是在 Javascript 这样的高级语言中，分配是由语言本身负责的。*

## ** Javascript 中的内存分配*

```
*var n = 123; // allocates memory for a number
var s = 'azerty'; // allocates memory for a string

var o = {
  a: 1,
  b: null
}; // allocates memory for an object and contained values

// (like object) allocates memory for the array and
// contained values
var a = [1, null, 'abra'];

function f(a) {
  return a + 2;
} // allocates a function (which is a callable object)

// function expressions also allocate an object
someElement.addEventListener('click', function() {
  someElement.style.backgroundColor = 'blue';
}, false);*
```

*在 javascript 中，函数调用也可以像*

*`var d = new Date(); // allocates a Date object
var e = document.createElement('button'); // allocates a DOM element`*

*一些方法也可以分配新的值或对象，比如*

```
*var s = 'azerty';
var s2 = s.substr(0, 3); // s2 is a new string
// Since strings are immutable values,
// JavaScript may decide to not allocate memory,
// but just store the [0, 3] range.var a = ['ouais ouais', 'nan nan'];
var a2 = ['generation', 'nan nan'];
var a3 = a.concat(a2);
// new array with 4 elements being
// the concatenation of a and a2 elements.*
```

# *内存的使用:*

*基本上，当我们读取或写入内存中分配的值时，就会用到内存。*

# *内存释放:*

*大多数与内存管理相关的工作都发生在这个阶段。因为在这个阶段，我们需要决定是否需要清理一些内存。用低级语言来说，释放空间是开发人员的职责。但是在 JS 和 Java 这样的高级语言中，这是由垃圾收集器来完成的。垃圾收集器的主要任务是始终监控内存，并决定是否可以清理一些内存。*

*垃圾收集器保留每个变量的引用，并检查该变量是否可达。如果这个变量可以从根节点到达，那么它会留在内存中，否则 GC 会释放内存。*

## *引用计数垃圾收集*

*这是最简单的垃圾收集算法。在这个算法中，检查如果一个对象被任何其他对象引用，那么它将留在内存中。如果一个对象没有被任何其他对象引用，那么它就被认为是“垃圾”并从内存中删除。这种方法的问题是，如果有一个循环引用，那么如果对象不能从根访问，但仍然从子对象引用，那么它就不会被垃圾收集器收集。*循环引用是内存泄漏的常见原因。**

```
*var x = {
  a: {
    b: 2
  }
};
// 2 objects are created. One is referenced by the other as one of its properties.
// The other is referenced by virtue of being assigned to the 'x' variable.
// Obviously, none can be garbage-collected.

var y = x;      // The 'y' variable is the second thing that has a reference to the object.

x = 1;          // Now, the object that was originally in 'x' has a unique reference
                //   embodied by the 'y' variable.

var z = y.a;    // Reference to 'a' property of the object.
                //   This object now has 2 references: one as a property,
                //   the other as the 'z' variable.

y = 'mozilla';  // The object that was originally in 'x' has now zero
                //   references to it. It can be garbage-collected.
                //   However its 'a' property is still referenced by
                //   the 'z' variable, so it cannot be freed.

z = null;       // The 'a' property of the object originally in x
                //   has zero references to it. It can be garbage collected.*
```

## *标记和扫描算法*

*因为上述算法存在局限性。大多数现代浏览器不使用它。垃圾收集最常用的算法是“标记和清除算法”。*

*标记和清扫算法假设从根可到达的所有对象都是有效对象，而其他对象可以被销毁。在 javascript 中，全局对象被认为是根对象。因此，所有可以从全局对象访问的对象都留在内存中，而其他对象则被释放。*

***标记**:该算法的第一步是标记从根*可达的所有子节点，即*全局对象。*

***清除:**下一步是从内存中清除所有没有标记*的对象。**

*** * JavaScript 内存管理最常见的限制是，开发人员不能手动触发垃圾收集器，如果他们想要的话*。 *****

# *常见的内存泄漏*

*由于编码选择不当，会出现常见的内存泄漏，其中一些如下:*

***未声明的全局变量:**如果您是 javascript 新手，并且了解到即使您不声明变量，JS 也会为您完成工作，那么这种情况通常会发生。你陷入了一个陷阱，假设你在一个函数中使用了一些临时变量，但是你没有在函数内部声明它，以为 JS 会为你声明这个变量。当你调用函数时，当函数超出作用域时，与函数相关的所有东西都会被垃圾收集器收集，除了你没有在里面声明的变量。这个问题的原因是，当我们没有声明一个变量时，Javascript 会在全局范围内创建这个变量，并且这个变量总是可以被全局对象访问的。*

> **所以请总是在你想使用的范围内声明变量。尝试使用较少数量的全局变量，并在不再需要时手动将其赋值为 null】。**

***闭包:**如果在外层函数中声明了一个变量，这个变量会自动为嵌套的内层函数所用，并继续驻留在内存中，即使这个变量没有在嵌套函数中被使用/引用，那么在闭包中就会发生内存泄漏。*

***Detached DOM/Out of DOM reference:**假设你在一个对象中引用了一个 DOM 元素。在从浏览器中销毁 dom 元素之后。因为您已经访问并存储了 dom 元素，所以即使从浏览器中删除了 dom，它也会保留在内存中。*

***缓存:**重复使用的大型表格、数组、列表中的对象都存储在缓存中。规模无限增长的缓存会导致高内存消耗，因为它们不能被垃圾收集。为了避免这种情况，请确保为其大小指定一个上限。*

# ***结论***

*   *变量有三个生命周期。分配、使用、释放*
*   *垃圾收集是自动执行的。我们不能强迫或阻止它。*
*   *对象在可访问时保留在内存中。*
*   *如果我们适当注意内存管理，我们可以避免大部分的内存泄漏。*

*希望你会发现一些新的信息，并喜欢这篇文章。如果你喜欢，请鼓掌支持。*

***参考文献:***

*   *[https://developer . Mozilla . org/en-US/docs/Web/JavaScript/Memory _ Management](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Memory_Management)*