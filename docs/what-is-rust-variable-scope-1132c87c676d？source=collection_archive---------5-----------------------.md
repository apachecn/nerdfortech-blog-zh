# 什么是 Rust 变量作用域？

> 原文：<https://medium.com/nerd-for-tech/what-is-rust-variable-scope-1132c87c676d?source=collection_archive---------5----------------------->

![](img/a243d9f74bbe88fb84154ad4eaeb98a9.png)

# 介绍

*   Rust 也可能是一种以性能和安全为目标的编程语言。
*   与其他编程语言不同，它不使用垃圾收集。
*   Rust 以非常低的开销提供了确定性的资源管理。
*   它准备成为高度共存和高度安全系统的语言。
*   每个变量在 [Rust 中都有一个作用域，从变量](https://www.technologiesinindustry4.com/)初始化的地方开始。
*   我们可以让较大作用域中的变量对该变量初始化的较大作用域中的作用域可见。
*   在一次作业中，我们会用讲义的结果来分配变量
*   因此，在 if 语句的范围内给出变量是完全有效的。

# 变量作用域

*   范围是块表达式存储其变量的地方。
*   作用域在 ASCII 文档中不直接表示，但是作用域在块表达式以`{ '符号开始时开始，在块表达式以` } '结束时结束(或者在块到达其结尾之前运行了` return '语句)。
*   [范围是存储块变量的内存块](https://www.technologiesinindustry4.com/)。
*   作用域是一个项目在程序中有效的范围。
*   当一个变量进入作用域时，它是有效的。它在超出范围之前一直有效。
*   fn main()let s = " hello "；//用 s 做东西}//
*   这个作用域现在结束了，s 不再有效

例子
println！(" {} "，s)；
// s 从现在开始有效
{// s 在这里无效，

还没有宣布

*   变量“s”指的是一个字符串文字，其中字符串的价格被硬编码到我们程序的文本中。变量从声明它的目的开始一直有效，直到这个范围的最高值。
*   当“s”进入范围时，它是有效的。
*   它一直有效，直到超出作用域。作用域之间的连接以及变量何时有效类似于其他编程语言。
*   现在我们将在这个理解的基础上通过引入字符串类型来休息。

# 字符串类型

*   Rust 具有第二种类型，string。这种排序是在堆上分配的，并且是在一个位置上存储一定量的在编译时我们不知道的文本。我们将使用 from 函数从字符串文字创建一个字符串，如下所示:

let s = String::from(" hello ")；

*   双冒号(::)是一个操作符，它允许我们在 String 类型下命名这个特定的函数，而不是使用 string_from 这样的安静名称。
*   这类[字符串通常会变异为](https://www.technologiesinindustry4.com/) :s.push_str("，world！");//向字符串追加文字

println！(" {} "，s)；//这可能会打印出“hello，world！`
let mut s = String::from(" hello ")；

*   字符串文字和字符串类型的区别在于，这两种类型是如何影响内存的。

# 字符串:内存和分配

*   运行时必须从操作系统请求内存。
*   当变量超出范围时，Rust 自动吸引 drop 函数返回内存。

从
调用 String::它请求它需要的内存
Drop 自动返回内存。

更多详情请访问:[https://www . technologiesinindustry 4 . com/2020/10/what-is-rust-variable-scope . html](https://www.technologiesinindustry4.com/2020/10/what-is-rust-variable-scope.html)