# C#类型[值，引用]解释

> 原文：<https://medium.com/nerd-for-tech/c-types-value-reference-explained-fc8b9014285d?source=collection_archive---------9----------------------->

在 C#中我们可以找到三种不同的类型，值类型、引用类型和指针类型。在本文中，我们将关注引用&值类型。

那么什么是引用和值类型呢？

![](img/152e0d2611fa7db530cfdfe92b97c196.png)

# **主要区别**

值类型和引用类型的主要区别在于它们的赋值。

```
int a = 5;
int b = a;
b++;
```

在上面的例子中，我们将变量“a”的值赋给变量“b”，之后，我们增加 b 的值。

b 的值是多少，a 的值是多少？
嗯，你可能知道 b 的值现在是 6，a 的值仍然是 5。

其原因是因为在赋值时，我们在通过值赋值的**调用中将值从 a 复制到 b。**

但是，是不是一直都是那样的？让我们看看下面的例子:

```
SampleClass instance = new SampleClass();
instance.age = 45;SampleClass second = instance;
second.age = 60;Console.WriteLine(second.age);
Console.WriteLine(instance.age);
```

在上面的例子中，两个输出都是 60，原因是类是引用类型，这意味着在我们赋值后它们指向内存中的同一个位置，当我们改变 second 时，实例也会改变。

# 为什么我们会有这个？

在我们看到了引用和值类型之后，问题来了。为什么我想要引用类型？为什么我要在内存中创建一个实例作为我的另一个实例？

假设我们有一个函数，它获取一个未知大小的字符串数组。(数组也是一种引用类型)。

在这种情况下，考虑一下如果数组有 100，000 个值，计算机复制数组的所有元素需要多少时间。

```
public static void Main(string[] args)
{ 
  int[] arr = new int[100000];
  CallMyFunc(arr);
}public static void CallMyFunc(int[] a)
{
  Console.WriteLine("Pow!");
}
```

在上面的例子中，如果数组是值类型，那么将所有值复制到“a”参数中会花费很多时间。

这就是为什么我们既有值类型又有引用类型，当涉及到像整数那样的小尺寸复制时，复制整数值不会花很多时间，因为它只有 4 个字节。

但是，在上面的例子中，它必须将 100000 * 4 = 400000 字节复制到一个新参数中(如果数组是值类型)。

# 如何通过引用调用生成值类型

让我们假设你有一个名为“Pow”的函数，你希望你的函数通过引用的方式在调用中得到一个整数。

C#提供了两种方法来做到这一点， **ref & out。**

```
public static void Main(string[] args)
{
  int a = 5;
  Pow(ref a); //now a value is 6
  Console.WriteLine(a); //prints 6
}public static void AddToA(ref int b)
{
  b++;
}
```

在上面的例子中，我们使用了 **ref** ，通过它，我们也引用了调用函数，这意味着我们获得了地址，通过这样做，我们可以改变参数和调用值。

```
public static void Main(string[] args)
{
  Pow(out int b);
  Console.WriteLine(b); //prints 5
}public static void AddToB(out int c)
{
  c = 5;
}
```

out 关键字强制在函数中初始化，这意味着在函数结束后，我们可以确定 b 存储了一个值。因为它强制在函数内部初始化，所以我们可以在主程序的调用中声明变量，正如你在上面看到的。

如果我们没有“c = 5”这一行，就会抛出一个错误，因为我们不能创建一个使用 out 关键字的函数，也不能初始化它得到的值。

```
public static void Main(string[] args)
{
  int b = 7;
  Pow(out b);
  Console.WriteLine(b); //prints 5
}public static void AddToB(out int c)
{
 c = 5;
}
```

我们也可以使用已经声明的变量和 out 关键字。使用这些关键字，我们可以引用调用者并改变他的值，我们也可以在同一个函数中“返回”多个值。

# ReferenceEquals 函数

如果要检查两种类型是否指向同一个地址。您可以使用 ReferecneEquals 函数

```
SampleClass a = new SampleClass();
SampleClass b = a;Console.WriteLine(ReferenceEquals(a, b)); //true
Console.WriteLine(ReferenceEquals(a, new SampleClass())); //false
```

# 为什么字符串是引用类型？

我们都知道字符串就像值类型一样工作。当我们把一个变量赋给另一个变量，然后我们改变其中一个，并不会导致另一个也改变。

喜欢这个例子:

```
string a = "Nice Article!";
string b = a;
b += " By Gilad Bar Ilan"; //a still holds "Nice Article!"
```

然而，如果我们在改变之前写 ReferenceEquals，我们就会得到 True

```
string a = "Nice Article!";
string b = a;
Console.WriteLine(ReferenceEquals(a,b)); //prints true
```

这样做的原因是因为每次我们链接一个新的值到一个字符串，这个字符串就会在内存中创建一个新的实例，在我们这样做之后，他就不再与之前的字符串相关了。

因为字符串是不可变的，我们看到字符串的行为就像值类型。

## **如果字符串是可变的**

```
string a = "hello world";
string b = a;
a[0] = 'b';
```

那么上面的代码也必须改变“b”变量。

## 那么，为什么字符串是引用类型呢？

在整个讨论之后出现了一个问题，为什么 string 是一个引用类型？

嗯，这和我们之前讲过的有关。让我们假设字符串是值类型，在这种情况下，如果我们有一个长度为 100，000 个字符的字符串。

这会使我们的程序运行得非常慢，通过使它成为引用类型，我们可以使我们的程序运行得更快。

通过将字符串作为引用类型而不是值类型，我们必须做出的唯一让步是字符串是不可变的。

# 结构和类

结构和类是值类型和引用类型的很好的例子。

结构是值类型，类是引用类型。

当我们谈论像 KeyValuePair 结构这样的小块数据时，我们宁愿使用 struct，因为复制这种 struct 并不慢。

每当我们谈到需要一段时间才能复制的大块数据时，我们宁愿使用一个类，并通过引用进行复制。

恭喜你，现在你知道引用和值类型的区别了！