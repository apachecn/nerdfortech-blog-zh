# PHP 备忘单

> 原文：<https://medium.com/nerd-for-tech/php-cheat-sheet-b91492d4d9a9?source=collection_archive---------3----------------------->

如果你已经熟悉了编程语言，并且想跳过一些基本的常见的东西，快速入门 PHP，这本书是为你准备的。

![](img/dfbe0640a85ad1f5b93b1f59c6fa4755.png)

## 什么是什么

PHP 是一种用于 web 开发的脚本语言。它很容易上手，有一个活跃和支持的社区，并且仍然需要相当多的技能。

与所有代码都在浏览器中运行的经典 html-css-js 项目不同，使用 PHP，您的代码将在服务器中运行。

开发一个 PHP 项目伴随着许多其他的依赖，比如 PHP 本身之上的数据库和服务器。所以，为了一次下载完，我们将在本教程中使用 **XAMPP** 。你可以从[这里](https://www.apachefriends.org/download.html)得到。

在你设置好 XAMPP 之后，你可以去 XAMPP 控制面板启动你需要的任何其他依赖项。对于 PHP 开发，我们将使用 **Apache** 服务器，在本教程中还会用到 **MySQL** 服务器。一旦启动 Apache 服务器，您就可以转到您喜欢的浏览器，键入 *"localhost"* 作为 URL，以访问本地 Apache 服务器。这是您本地托管项目并观察变化的地方。

我们想通过 XAMPP 运行的任何项目，我们必须把它们放在 *XAMPP/htdocs* 文件夹下。然后，我们可以简单地启动 Apache 服务器并转到 URL*localhost/project name*来查看它。这样做将会，如果存在的话，自动引导你到 index.php 文件。

## 入门指南

PHP 的最终目标是获取动态数据(可能来自数据库或用户输入)并将其嵌入到 html 中以对其进行操作。

php 代码写在:

为了声明一个变量，我们使用美元符号，并用大写/小写字母或下划线来命名它们。

> $ name = ' Gamze

稍后在您的 html 或 php 代码中，您可以在 php 标签中使用这个变量作为；

在您的 php 代码中，您可以覆盖之前定义的任何变量。但是，如果您想避免这种情况，可以使用 define 函数。这个函数有两个参数，一个变量名和一个值，然后将值永久地附加到变量上。

> define ('name '，' gamze ')；

或者，你可以用一个点把两个变量并排放在一起。

> $string= '你好，我的名字是:'；
> 
> $ name = ' Gamze
> 
> echo $string。$ name

或者，您可以通过单引号或双引号一起使用两个变量；

> 回显“$ string $ name”；
> 
> echo“你好我是$ name”；

PHP 将字符串视为字符数组。所以如果你想得到我们变量的第一个字母“g ”,用这个:

> $ name[0]；

您可以使用函数来计算字符串的长度:

> strlen($ name)；

您可以使用以下函数使变量全部大写或小写:

> strto upper($ name)；
> 
> strtolower($ name)；

你可以用符号+、-、*、/、**对 PHP 进行加、减、乘、除和乘方运算。或者，您可以执行一个数学方法，将结果与变量相等，只需:

> $ number = 40
> 
> $ number+= 10；//现在数字变量保存的值是 50

您可以采用一个浮点数，并通过使用函数将其更改为最接近的较低或较高的整数；

$ number = 2.17

> 地板($ number)；//给出的结果为 2
> 
> ceil($ number)；//结果为 3

一旦在 PHP 中创建了一个数组，就可以通过索引回显数组中的每一项。然而，如果你试图回显整个数组，你会得到一个错误。因此，您可以使用下面的函数回显数组；

> print _ r($ array name)；

向数组中添加新元素有两种选择:

> $ array name[1]= 60；//将数组的索引 1 中的所有内容替换为 60。如果那里什么都没有，将在提到的索引中添加一个新项目。
> 
> array_push($arrayName，60)；//将值 60 添加到数组的末尾

或者，您可以将一个数组添加到另一个数组的末尾，使用合并功能创建一个新数组:

> $ array tree = array _ merge($ array one，$ array two)；

您还可以创建作为键和值对工作的数组。

> $arrayName =['Gamze' = > '女性'，' John' = > '男性'，' Jane' = > '女性']
> 
> echo array name[' John ']//将' Male '打印到屏幕上。

您可以使用 pop 方法从数组中删除一些项；

> array _ pop($ array name)；//将从数组中删除最后一项

## 环

PHP 文件中的 for 循环可以定义为:

> for($ I = 0；$i <5; $i++) { };

If we’re running, let’s say, an array through a for loop we can count the items via the count function or simply use the foreach function.

> for ($i=0; $i
> 
> foreach($someArray as $i) {}; //note that you can access each item within the array with i if you use this notation.

## Embedding PHP into HTML

You can write PHP inside HTML using the php tags as shown above. You can also combine PHP with HTML as:

> 土耳其里拉
> 
> 

## 分配与比较

将一个变量赋给另一个变量可能会与我们在这种语言中使用的比较语法相混淆。请参见下面的示例:

> $ var 1 = 5；$ var2 = 10$ var3 = ' 5
> 
> echo $ var1 = $ var2//将值 10 赋给 var1 并打印出来。
> 
> echo $ var1 = = $ var3//将比较 5 和“5”是否相同，并返回 true，因为它们持有相同的值。这叫做“松散比较”。
> 
> echo $ var1 = = = $ var3//将返回 false，因为这被称为“严格比较”，虽然值相同，但两个变量并不完全相同，并且由于引号的原因，它们处于两种不同数据类型的 f ct 中。

**注意:**如果 PHP 在 echo 上返回 false，它将不会在屏幕上显示任何内容，但是如果返回 true，它将会显示“1”。

提示:如果你使用 echo 在双括号内运行一个变量，PHP 将会承认这一点。然而，如果在这种情况下你的变量是一个作为键-值对工作的数组，并且你试图使用键调用一个值，它不会工作。因此，在这种情况下，请确保添加大括号，如下所示:

> echo " { $ array name[' key ']} "；

## 声明全局和局部变量

PHP 使用范围。如果在函数内部声明变量，它只能在函数的作用域内使用。同样，如果在函数外部声明一个，它也不会读取函数内部的变量。如果您想使用在函数之外声明的变量，请使用下面的语法:

> $ name:" Gamze "；
> 
> 函数 sayHello () {
> 
> 全局$ name
> 
> 回显“你好$ name”；}

您也可以在调用函数时将全局变量声明为参数，从而在函数范围内获取全局变量，如下所示:

> $ name:" Gamze "；
> 
> 函数 say hello(){ echo " hello $ name "；}
> 
> say hello($ name)；

## 包含和要求

您可以将代码从另一个. php 文件中获取。php 文件，使用 include 或 require 函数:

两者的区别在于，两个标签都将首先运行包含/必需的文件。但是如果提到的文件有错误，如果它是用 include 标记声明的，页面的其余部分仍然会出现，但是如果它是用 required 标记声明的，主页的其余部分也将无法加载。

请记住，只要它在 php 标签中，您就可以在项目中的任何位置包含一个新文件，次数不限——甚至是在 html 中间！