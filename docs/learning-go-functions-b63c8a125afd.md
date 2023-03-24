# 学习围棋—功能

> 原文：<https://medium.com/nerd-for-tech/learning-go-functions-b63c8a125afd?source=collection_archive---------3----------------------->

![](img/f5bb59682f6053470d75c70cac45c605.png)

# 功能

函数是一段代码，它接受一些输入，对输入进行一些处理，并产生一些输出。Go 中的**函数是一等公民**类似于 javascript。

## 申报

Go 中的一个函数是用 **func** 关键字声明的。一个函数有一个**名**，一个逗号分隔的**输入参数列表**及其类型，**结果类型**，以及一个**主体。**

```
func <name>(arg <type>) <return type> {
     //body
     return <val>
}
```

***例题***

```
func firstFn(arg1 int, arg2 int) int {
     val := arg1 + arg2
     fmt.Println("I am a method and my value is : ", val)
     return val
}
```

## 调用函数

调用方法只是使用函数名并传递必要的参数。对于有任何编程经验的人来说非常容易理解。

```
firstFn(5)
```

我想上面的示例函数让事情变得清楚了。这是一个非常简单的函数。有一些重要的事情要记住。

*   参数是可选的。只有当你的函数需要调用函数的值时，它们才是必需的。
*   返回类型是可选的。只有当函数返回一个有意义的值时，才有必要使用返回类型和返回语句。

> 让我们围绕 Go 中函数的声明和初始化来讨论一些语法规则。

> **函数参数分组**

```
func secondFn(arg1, arg2, arg3 int, arg4, arg5 string, emp *Employee) int {
     fmt.Println("I am a method and my value is : ", arg1)
     return arg1
}
```

观察函数参数。您可以将具有相同类型的变量分组，用分隔符(，)分隔，并且为所有变量声明一次类型。arg1、arg2、arg3 是 int 类型，arg4、arg5 是 string 类型。

> **具有多个返回值的函数**

```
func thirdFn(arg1, arg2, arg3 int, arg4, arg5 string) (int, string) {
   fmt.Println("I am a method and my value is : ", arg1)
   return arg1, arg5
}
```

函数在 Go 中可以返回多个值。对于有 java 背景的人来说这是最酷的特性，因为 java 函数只能返回一个值。
观察返回类型和返回语句是如何从单值返回函数变化而来的

```
func thirdFn(arg1, arg2, arg3 int, arg4, arg5 string) **(int, string)**
```

返回类型是用逗号分隔的**和括在双引号内的**。****

```
return arg1, arg5
```

****return** 已经提供了相应的**返回值，以逗号分隔**和**的顺序，如函数签名**中的 return type 所声明。**

> **多值函数的用例将在与 Go 中的错误处理相关的文章中得到更多的强调，其中函数返回值以及错误(如果有的话)。调用者可以在使用函数的值之前检查错误值。**

> ****用作数值****

**函数可以是匿名的，即没有名字。它们是为小范围声明的。例子**

```
func main() {
    func() {
        fmt.Println("i am anonymous func")
    }()
}
```

**注意，该函数以关键字 **func** 开始，但没有名称**。函数体作为块位于花括号内。函数定义后有一个 Paran thesis**()**。这是函数执行所必需的。
函数的作用域只在 **main()** 函数内部。****

****这些匿名函数可以赋给一个变量。函数可以使用被赋予的变量来调用。****

```
**func main() {
   var fn func() = func() {
      fmt.Println("i am func")
   }
   fn()
}**
```

****该函数还可以添加参数，这些参数可以在调用该函数时被提供值。****

```
**func main() {
    var fn = func(arg1 int) {
       fmt.Println("i am func with : ", arg1)
    }
    fn(5)
}**
```

> ******功能关闭******

****当即使在父函数已经执行之后，子函数仍然保持父作用域的环境时，就创建了闭包。这有助于传播已经通过父函数内的封闭范围匿名函数执行的函数的字段。****

```
**function foo(arg1 int) {      function inner(arg2 int) int {          return arg1 + arg2     }      return inner }  var fooInner = foo(5) fmt.Println(fooInner(4))**
```

# ****方法****

****方法是 Go 中特殊类型的函数。除了正常的函数签名，它们还有一个额外的**接收器**。****

```
**func (<var> <type>) <name>() <return_type>{
   /* function body*/
}**
```

> ******带值接收器的方法******

****示例—从上面的员工结构中调用。****

```
**type Employee struct {
   id int
   name string
   dept []string
}func main() {
    emp := Employee{
        id: 123,
        name: "prateek",
        dept: []string{
             "Technology",
             "MFP",
             "Delivery",
        },
    }
    fmt.Println(emp)
    emp = emp.setEmpName()
    fmt.Println(emp)
}func (e Employee) setEmpName()  {
   e.name = "Hari"
}Result
{123 prateek [Technology MFP Delivery]}
{123 Hari [Technology MFP Delivery]}**
```

****使用方法观察名称的变化。****

> ******带指针接收器的方法******

```
**func (e *Employee) SetEmpName() {
   e.name = "sridhar"
}**
```

****如果使用上面的方法而不是值为 reciever 的方法，那么在原始结构实例 emp 中更新**值，而不从函数 SetEmpName()** 返回任何值。对于指针类型，这是意料之中的。****

# ****具有任意数量参数的函数****

****在 Go 中，我们可以在类型前使用 **…** 在函数中设置任意数量的参数。****

```
**func multiParams(values ...int) (*int) {
   fmt.Println(values)
   result := 0
   for _, val := range values {
      fmt.Println(val)
      result += val
   }
   return &result
}func main() {
   sum := multiParams(1, 2, 4, 5)
   fmt.Println(*sum)
}Result
12**
```

****这里我们到了帖子的结尾。**你可以在帖子上评论任何细节或不一致的问题。我将在以后的文章中讨论各种主题——常量、枚举器、延迟、错误处理和 Go 中的死机******