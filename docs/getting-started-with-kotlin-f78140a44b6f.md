# Kotlin 入门

> 原文：<https://medium.com/nerd-for-tech/getting-started-with-kotlin-f78140a44b6f?source=collection_archive---------13----------------------->

![](img/c9cf2a3b29a4cd54d660f7aff42974c5.png)

由 [Mikesh Kaos](https://unsplash.com/@mikeshkaos?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 Unsplash 上拍摄的照片

这个博客主要是为打算开始学习科特林的初学者(当然像我一样)准备的。这不是一个项目或一个教程，而是我觉得我每天都需要的东西。所以不要再浪费时间了，让我们开始吧。

## 声明一个变量

变量可以分为两类

1.  val:只读(不能修改)
2.  var:声明为 var 的变量可以被重新分配和修改。

这里最好的部分是变量可以用任何类型赋值，可以是字符串也可以是整数。

所以在 kotlin 中声明一个变量很简单:

```
var thisIsModifiable = 1
val youCannotChangeMe = "abc"
```

你可以在声明的时候定义变量的类型。在下面的例子中，我们明确提到变量的类型是整数，为此我们使用“:”

```
val thisIsInt: Int
```

## 条件语句

否则:

```
if (condition) { 
    println("Inside if") 
} else if (condition) { 
    println("Inside else if") 
} else {
    println("Inside else")
}
```

when:这类似于 c 中的`switch`语句。

```
when(x) {
    1 -> println("x is 1")
    2 -> println("x is 2")
 else -> println("Neither 1 or 2")
}
```

## 数组、列表、集合和映射

*   Kotlin 中的*数组*由`Array`类表示。它们是*不变量*。这意味着 Kotlin 不允许我们将一个`Array<String>`赋值给一个`Array<Any>`，这可以防止可能的运行时故障

```
val hellos = arrayOf("hi", "hello", "hola", "hello")
// [hi, hello, hola, hello]
```

Kotlin 也有表示原始类型数组的类，没有装箱开销:`ByteArray`、`ShortArray`、`IntArray`。示例:

```
**val** x: IntArray = intArrayOf(1, 2, 3)
```

*   *List* 是一个有序集合，可以通过索引访问元素。元素可以在一个列表中出现多次。

```
val hellos = listOf("hi", "hello", "hola", "hello")
// [hi, hello, hola, hello]
```

*   *集合*是唯一元素的集合。

```
val hellos = setOf("hi", "hello", "hola", "hello")
// [hi, hello, hola]
```

*   *Map* (或 *dictionary* )是一组键值对。键是唯一的，每个键映射到一个值。这些值可以重复。

```
val numbersMap = mapOf("key1" to 1, "key2" to 2, "key3" to 3, "key4" to 1)println("All keys: ${numbersMap.keys}")  
println("All values: ${numbersMap.values}") All keys: [key1, key2, key3, key4] 
All values: [1, 2, 3, 1]
```

Kotlin 为每种集合类型提供了两个接口——只读的和可变的。注意，改变一个可变集合并不要求它是一个`[var](https://kotlinlang.org/docs/basic-syntax.html#variables)`:写操作修改同一个可变集合对象，所以引用不会改变。虽然，重新分配一个`val`集合会给出一个编译错误。

## 环

这可能是最重要的部分之一，因为我大部分时间都被困在这里😛

For 循环:`for`循环遍历任何提供迭代器的东西。

```
val marks = listOf(1,2,3)
for(mark in marks) {
   println(mark)
}
```

要使用列表的索引进行迭代，我们可以做如下工作:

```
val marks = listOf(1,2,3)
for(i in marks.indices) {
   println(marks[i])
}
```

还有一种东西叫做`forEach`。使用`forEach`，我们可以简化循环逻辑。

```
val marks = listOf(1,2,3)
marks.forEach { mark ->
     println(mark)
}
```

## 定义函数

```
fun myFirstFunction(x : Int, y: String) : Boolean {
    var result = false
    .... 
    return result 
}
```

这里我们定义了一个名为`myFirstFunction`的函数，有两个参数，一个整数和一个字符串。这有一个布尔返回类型。

就是这样！这些只是你们所有人的几个起点。这肯定是不够的，但它会增强你的信心，帮助你学习科特林。快乐学习！:)