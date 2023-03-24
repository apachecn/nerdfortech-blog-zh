# 元组，C#中的值元组—概述

> 原文：<https://medium.com/nerd-for-tech/tuple-valuetuple-in-c-a-summary-6ddedb99c72e?source=collection_archive---------2----------------------->

这只是对 tuple 的一个总结，因为我的笔记似乎到处都是。

# ValueTuple 和 Tuple 的区别

C#中有两种类型的元组类型，System。元组和 System.ValueTuple 元组。

**系统。元组** = >引用，成员是属性，只读。

**系统。ValueTuple** = >值，成员是字段，不只读。

两种类型的非常简单的元组:

```
System.Tuple<Int32> intTup = new Tuple<Int32>(100);
Console.WriteLine (intTup); //display (100)
Console.WriteLine (intTup.Item1); // displays 100

System.ValueTuple<Int32> intTupVal = new ValueTuple<Int32>(100);
Console.WriteLine (intTupVal); //(100)
Console.WriteLine (intTupVal.Item1); //100
```

那么有什么区别呢？

**对于初学者来说，尝试** `**intTup.Item1 = 200;**` **会给你一个编译错误。但是** `**intTupVal.Item1 = 200;**` **你可以做，而且会起作用。**

元组类型作为引用，在内存堆和内存分配方面更加困难，因此 ValueTuple 将为您提供更好的性能。

元组类型和值元组类型可以以相同的方式创建，**但是，值元组类型有一种额外的创建方式。**

创建元组和值元组的另一种方法是使用 create 关键字:

```
var intTup = System.Tuple.Create<Int32>(100);
var intTupVal = System.ValueTuple.Create<Int32>(200);
```

对于 **ValueTuple** ，有另外一种创建新值的方法:

```
(int, int, int) tup1 = (100,200, 300);
var tup2 = (400,500,600);

//to see the type:
Console.WriteLine(tup2.GetType()); 
//System.ValueTuple`3[System.Int32,System.Int32,System.Int32]
```

此外，要访问元组或值元组的元素，我们必须访问 Item1、Item2 等，这令人困惑。但是对于 ValueTuple 类型，我们可以命名元素:

```
(int first, int second , int third) tup1 = (100,200, 300);
 Console.WriteLine(tup1.first);//100

var tup2 = (first:100, second:200, third:300);
Console.WriteLine(tup2.second); //200
```

如果 ValueTuple 只有 1 个值，那么试图以这种方式创建它是无法编译的。它与创建它的其他方法一起工作(已经提到)。

ValueTuple 类型可以保存比 Tuple 类型更多的元素。如果你有一个元组，可以用`ToValueTuple.`把它转换成 ValueTuple

# 嵌套元组

因为元组类型受元素数量的限制，我们可能需要一个元组而不是另一个元组，这样我们就可以传递我们需要的所有东西。

```
var numbers = Tuple.Create(1, 2, 3, 4, 5, 6, 7, Tuple.Create(8, 9, 10, 11, 12));
Console.WriteLine(numbers.Item1); //1
Console.WriteLine(numbers.Rest.Item1); //(8, 9, 10, 11, 12)
Console.WriteLine(numbers.Rest.Item1.Item1);   //8
```

# 比较值元组，元组类型

对于 ValueTuple 类型，即使我们给元素取了不同的名称，比较的也是值。

```
(int x, int y) valtup1 = (3,4);
(int i, int j) valtup2 = (3,4);

Console.Write(valtup1 == valtup2);//True
```

但是，对于引用类型元组:

```
Tuple<int,int> tup1 = new Tuple<int, int>(3,4);
Tuple<int,int> tup2 = new Tuple<int, int>(3,4);

Console.Write(tup1 == tup2);// False
```

# 在方法中返回 ValueTuple

除了返回类型 ValueTuple，我们还可以指出元素应该有什么名称。

```
 public static (int, int) GetAValueTuple(){
   return (first:20, second:30);
 }

 var v = GetAValueTuple();
 Console.WriteLine(v);
 //Console.WriteLine(v.first); will not find first

 (int x, int y) w = GetAValueTuple();
 Console.WriteLine(w);
 Console.WriteLine(w.x); //will not find first but x is 20

 //BUT, if we name the return and the return type!
public static (int first, int second) GetAValueTuple(){
  return (first:20, second:30);
}
var v = GetAValueTuple();
Console.WriteLine(v.first);//20

//We can also do 
(int x, int y) = GetAValueTuple();
Console.WriteLine(x);//20
Console.WriteLine(y);//30
```

这些是主要的事情。我希望它能澄清一些事情。