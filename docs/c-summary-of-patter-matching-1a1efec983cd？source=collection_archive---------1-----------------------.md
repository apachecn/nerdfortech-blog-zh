# C# —模式匹配概述

> 原文：<https://medium.com/nerd-for-tech/c-summary-of-patter-matching-1a1efec983cd?source=collection_archive---------1----------------------->

随着 C#新版本的出现，我们找到了匹配模式的新方法，这开始让我感到困惑，所以我决定给自己写一个总结。

模式匹配是在 C#7 中引入的。(本文中不包括正则表达式)

# 在 if 中检查对象的类型

当我们有一个 if 语句时，我们现在能够根据一个对象是否是我们感兴趣的类型来得到一个 if。参见下面的代码。

```
object i = 12; //object assigned an int
object s = "hello"; //object assigned a stringif (i is int x)//if the object i is int, it will be assigned to x
{
   Console.WriteLine(x);//will happen
}if (s is int xx)
{
   Console.WriteLine(xx);//will not happen since s is not an int
}if (s is string  xxx)
{
   Console.WriteLine("string " + xxx);//will happen
}
```

# 切换没有文字值的大小写

我们不需要在开关的 case 语句中有文字值。看看这段代码:

```
//We have 2 different classes
public class **AClass**
{
   public string? s { get; set; }
}
public class **AClassToo**
{
   public string? s { get; set; }
}//we are going to assign instances of the classes to tupe objects
Object obj = **new AClass**();
((AClass)obj).s = "Hello Class";
Object obj2 = **new AClassToo**();
((AClassToo)obj2).s = "Hello Class Too";//now assign one of those class objects to another object that we will use in the switch
**Object theObj** = obj;//now in the switch (ac ir whatever name you want)
switch(**theObj**)
{
   **case AClass ac when ac.s** == "Hello":
      Console.WriteLine("class hello");
      break;  
  ** case AClass ac when ac.s == "Hello Class":**
      Console.WriteLine("class hello class");
      break;
   case AClassToo ac2 when ac2.s == "Hello":
      Console.WriteLine("class 2 hello");
      break;
   **case AClassToo ac2 when ac2.s == "Hello Class Too":**
      Console.WriteLine("class hello Too");
      break;
   default:
      break;
}//Will print class hello class because theObj is a AClass with s as "Hello Class"//if we use the same code but have the assigment 
//Object theObj = obj2; instead (AClassToo type).//it will print class hello too.
```

# 带有开关表达式的开关语句

从 C# 8 开始。

开关表达式使用= >来指示返回值。这使得代码更短。

与前面的开关类似，但不相同，因为这种类型的开关针对每种情况返回一个值:

```
//assign the switch to a variable
string message = theObj switch
{
   AClass **=>** "class hello", //if AClass return this string
   AClassToo => "Hello Class Too",
   **null =>** "null",
   **_ =>** "some other thing", //any other cases, return this};
Console.WriteLine(message);
```

没有案件或突破；这里的关键词。

为了使它与原始版本更相似:

```
string message = theObj switch
{
   AClass **ac when ac.s** == "Hello" **=>** "class hello",
   AClass ac when ac.s == "Hello Class" **=>** "class hello class",
   AClassToo **act when act.s** == "Hello"**=>** "classToo hello",
   AClassToo act when act.s == "Hello Class Too" **=>** "classToo hello too",
   null => "null",
   _ => "some other thing",
};
Console.WriteLine(message);
```

如果您只想匹配类，而不检查任何属性，我们可以这样做:

```
string message = theObj switch
{
   AClass **_** => "class hello",
   AClassToo **_** => "classToo hello",
   null => "null",
   _ => "some other thing",
};
Console.WriteLine(message);
```

# Switch 语句 C#9

C#9 通过嵌套语句和对>比较的支持，使得 switch 语句更加有趣。

我们之前的例子，检查 s 的值，会是类似这样的嵌套表达式。

注意，在检查 s 值时，我们没有==

```
**string message = theObj switch**
{
   **AClass ac  => ac.s switch //if this class, switch for s checking**
   {
      **"Hello"** **=> "class hello",**
      "Hello Class" => "class hello class",
      _ => "some other thing AClass",
   },
   **AClassToo act => act.s switch**
   {
      "Hello" => "classToo hello",
      "Hello Class Too" => "classToo hello too",
      _ => "some other thing AClassToo",
   },
   null => "null",
   _ => "some other thing",
};
```

如果我们要比较数量，我们可以使用类似于:

```
Object obj3 = new AClass3();
((AClass3)obj3).i = 15;string message = theObj switch
{
   AClass3 ac3  => ac3.i switch
   {
      **> 5  => "> 5",** _ => " <= 5",
   },
   null => "null",
   _ => "some other thing",
};
Console.WriteLine(message);
```

# 用解构匹配类内容

假设我们有一个类，它有这样一些数据:

```
public class AClass4
{
   public int? I { get; set; }
   public string? S { get; set; }
   public AClass? ACLASS { get; set; } public void Deconstruct(out int? i, out string? s)
   {
      i = I;
      s = S;      
   }
}
```

我们可以有这样一个开关:

```
var result = four switch
{
   AClass4(30, "how are you?") => "how are you 30",
   AClass4(25, "hey" ) => "hey 25",
   _ => "who?"
};
Console.WriteLine(result);
```

我们可以有上面的开关，因为我们在类中有解构方法。我们使用该方法与我们在交换机中使用的类的“four”实例进行比较。

如果我们有这样的类:

```
AClass4 four = new AClass4();
four.I = 30;
four.S = "how are you?";
four.ACLASS = new AClass();
four.ACLASS.s = "I am here";
```

我们会得到“你好吗 30”。如果任何值与传递的值不同，我们将得到“谁？”或者"嘿 25 "如果匹配。注意，我们没有检查 4 的类，因为它不是解构方法的一部分。我们可以包括它。

与上面的实例相同，但解构如下:

```
public void Deconstruct(out int? i, out string? s , out AClass? f)
{
   i = I;
   s = S;
   f= ACLASS;
}
```

当类为空或不为空时，我们可以使用选项进行开关检查:

```
var result = four switch
{
   //if the AClass in the AClass4 is null
   AClass4(30, "how are you?", **null** ) => "how are you 30",
   //if the AClass in the AClass4 is set 
    AClass4(30, "how are you?", **_**  ) => "hey 30",
   _ => "who?"
};
Console.WriteLine(result);
//the above prints hey 30
```

如果我们还想检查内部类的一些属性，我们也可以像这样:

```
public void Deconstruct(out int? i, out string? s , **out string? fs**)
{
   i = I;
   s = S;
  ** fs= ACLASS.s;**
}var result = four switch
{
   AClass4(30, "how are you?", null ) => "how are you 30",
   **AClass4(30, "how are you?", "I am here") => "hey 30 with class I am here",**
   AClass4(30, "how are you?", _  ) => "hey 30",
   _ => "who?"
};
Console.WriteLine(result);
```

解构方法通常像这样使用(对于我们一直在使用的同一个类)

```
var (ii, ss, cc) = four; //use the var to get the parts from the class
Console.WriteLine(ii);//ii will be the I from "four"
Console.WriteLine(ss); // S
Console.WriteLine(cc); //ACLASS.s
```

# 一些额外的开关可能性

我们也可以这样做来检查我们正在检查的类中的某个部分；

```
var result = four switch
{
   **{ ACLASS: { s:"I am here" } } ** =>  "I am here",
   _ => "who?"
};
```

向内部类添加另一个字符串。

```
AClass4 four = new AClass4();
four.I = 30;
four.S = "how are you?";
four.ACLASS = new AClass();
four.ACLASS.s = "I am here";
**four.ACLASS.s2 = "not me";**//this switch now will display who?
var result = four switch
{
   **{ ACLASS: { s:"I am here", s2:"me too" } }** =>  "I am here",
   _ => "who?"
};//changing the class to have four.ACLASS.s2 = "me too" will display "I am here"
```

如果我们不仅要检查内部类的属性，还要检查主类的属性(四个):

```
var result = four switch
{
 **{S:"how are you?" ,ACLASS: { s:"I am here", s2:"me too" } }    =>  "I am here",**   _ => "who?"
};
```

检查该类是否为`{ACLASS: null}`

我们也可以使用元组。这个例子来自微软的文档(不是全部):

```
var newState = **(state, operation, key.IsValid) switch**
{
   (State.Opened, Operation.Close, _)      => State.Closed,
   (State.Opened, Operation.Lock, true)    => State.Locked,
   (State.Locked, Operation.Open, true)    => State.Opened,
   ...                        
   _ => state
};
```

我目前就知道这么多。