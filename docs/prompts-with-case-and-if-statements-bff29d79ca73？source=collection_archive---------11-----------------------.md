# 带有 Case 和 If 语句的提示

> 原文：<https://medium.com/nerd-for-tech/prompts-with-case-and-if-statements-bff29d79ca73?source=collection_archive---------11----------------------->

![](img/d15ffcec2872a3bbb8be82fff2b3b20d.png)

乔恩·泰森在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

因此，在周末，当我在车里等我哥哥的时候，我突然想到了一个主意。我想输入响应，然后根据输入给出响应。这听起来很简单，而且它会给我一个机会在周末创造一些有趣的东西。让我们来分析一下。我将在提供例子的同时讨论`gets`方法和`if` / `case`语句。下面是每个选项的例子，但请随时在您的`irb`上测试它们，因为，就像我的一位导师常说的，“测试是免费的。”

## `gets` **方法**

所以首先要做的是。让我们提示我们的程序获取用户输入。`gets`方法有三种选择，包括`gets`、`gets.chomp`和`gets.strip`。简单地使用`gets`将返回输入；然而，正如你在下面看到的，它会把字符串插值后的所有内容移到新的一行，这会影响我的条件语句。发生这种情况是因为当你按回车键时，它会创建一个新行字符，称为`/n`。

```
print "Hi, what's your name? "name = getsprint "Hi! Welcome #{name.capitalize}!"// Hi, what's your name? joy
// Hi! Welcome Joy
!
```

对你的意图要求很高，这可能没什么大不了的，但如果是的话，那么切换到`gets.chomp`或`gets.strip`会更有用。使用`gets.chomp`时，不用担心换行符，而是空格。`gets.strip`但是，它消除了空白和换行符。

```
//gets.chomp methodprint "Hi, what's your name? "name = gets.chompprint "Hi! Welcome #{name.capitalize}!"// Hi, what's your name? joy 
// Hi! Welcome Joy ! //gets.strip methodprint "Hi, what's your name? "name = gets.stripprint "Hi! Welcome #{name.capitalize}!"// Hi, what's your name? joy 
// Hi! Welcome Joy!
```

## `if` / `case`报表

为了增加情况的复杂性，我希望 ruby 打印出对给定输入的特定响应。这是 where`if`语句进入的画面。当使用像`==, !=, <=, >=, <, >, ||, &&`这样的操作符时，你可以设置条件。这些结构也使用下列保留字:`if`、`else`、`elsif`和`end`。

```
print "Hi, what's your name? "name = gets.stripif name.downcase == "joy"print "Welcome back #{name.capitalize}! How's it going?"elseprint "Hi! Welcome #{name.capitalize}!"end
```

有时候，对于你的情况来说，陈述并不总是最好的选择。幸运的是`case`语句是另一个可以依靠的选项。一个`case`语句有三个组成部分:`case`、`when`和`else`。就个人而言，我更喜欢使用`case`语句，因为它有助于你的代码变得更易读、更整洁。

```
print "Hi, what's your name? "name = gets.stripcase namewhen "joy"print "Welcome Back! How's it going?"elseprint "Hi! Welcome #{name.capitalize}!"end
```

请随意评论、分享和查看下面发布的一些资源。感谢阅读！

[](https://www.rubyguides.com/2019/10/ruby-chomp-gets/) [## 如何使用 Ruby Gets & Ruby Chomp 方法

### 您正在编写一个 Ruby 程序&您想问用户一个问题...你怎么能这样做？嗯，你可以用红宝石…

www.rubyguides.com](https://www.rubyguides.com/2019/10/ruby-chomp-gets/) [](https://www.rubyguides.com/2015/10/ruby-case/) [## Ruby Case 语句(包含示例的完整教程)

### 每当需要使用 if / elsif 语句时，可以考虑使用 Ruby case 语句。在这个…

www.rubyguides.com](https://www.rubyguides.com/2015/10/ruby-case/) [](https://www.w3resource.com/ruby/ruby-case-statement.php) [## Ruby case 语句:- w3resource

### case 表达式是 if-elsif-else 表达式的替代形式。语法:case 表达式[when 表达式[，表达式…

www.w3resource.com](https://www.w3resource.com/ruby/ruby-case-statement.php)  [## 目标

### case 语句可以以 else 子句结束。每个 when 语句可以有多个候选值，用…

ruby-doc.org](https://ruby-doc.org/docs/keywords/1.9/Object.html#method-i-case)  [## 目标

### case 语句可以以 else 子句结束。每个 when 语句可以有多个候选值，用…

ruby-doc.org](https://ruby-doc.org/docs/keywords/1.9/Object.html#method-i-if)