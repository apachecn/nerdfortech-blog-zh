# 什么是 Ruby Lambda 函数？

> 原文：<https://medium.com/nerd-for-tech/what-are-ruby-lambda-functions-6cb6bfe9b20c?source=collection_archive---------3----------------------->

## 介绍 Lambda 及其一流的地位

![](img/1c6026f972269d71bd9c22c4b7c138a8.png)

法比奥·桑塔尼耶洛·布鲁恩通过 unsplash.com[拍摄](https://unsplash.com/photos/Y6tGu-OH8lA)

如果你搜索的是 AWS Lambda 函数，那么你来错地方了。Ruby Lambda 函数是它们自己的东西，尽管它们不像 AWS Lambda 那样华丽或者广为人知，但我认为作为一名 Ruby 开发者理解它们同样重要。我的希望是在这篇博文中让你相信这一点。

# 兰姆达斯是什么？

如果你熟悉 JavaScript，你就会知道 JavaScript 把它的函数视为一等公民。如果你不是，这只是一种奇特的说法，函数可以作为变量存储，作为参数传递给其他函数，并作为函数的返回值。

与 JavaScript 不同，Ruby 不把它的函数视为一等公民，这在一定程度上限制了它的功能。在 Ruby 中，lambda 弥合了这一鸿沟，允许我们创建被视为一等公民的函数。这允许我们封装某个代码块和其中包含的任何内容，并在应用程序的其他地方使用它！

事实上，在写这篇文章的过程中，我发现 lambda 并不是 Ruby 独有的，在大多数编程语言中都存在。

# lambdas 怎么写？

现在我们已经讨论了什么是 lambdas，让我们来看看定义它们的几种方法。

```
# lambda
1\. my_lambda = lambda { puts "Hello from my lambda!" }# literal lambda
2\. my_literal_lambda = ->() { puts "Hello from my literal lambda!" }
```

调用你新的很酷的 lambda 函数可以通过以下任何一种方式来完成。

```
1\. my_lambda.call2\. my_lambda.()3\. my_lambda.[] # this can also be called as my_lambda[]4\. my_lambda.===
```

如果你想使用参数呢？

```
1\. lambda_with_arg = lambda { |name| puts "Hi, my name's #{name}!" }2\. literal_lambda_arg = ->(name) { puts "Hi my name's #{name}!" }
```

你可以用以下方式称呼这些兰姆达斯。

```
1\. lambda_with_arg.call("David")2\. lambda_with_arg.("David")3\. lambda_with_arg["David"]4\. lambda_with_arg.==="David"
```

如果您想使用多个参数，只需像往常一样用逗号分隔即可。我正在用 Ruby 3 . 0 . 1 版本测试这个，不管出于什么原因，方括号`[]`只能通过移除分隔 lambda 名称和方括号的`.`来调用。

Lambdas 也可以通过使用 do-end 关键字写成多行块。

```
1\. multi_line_lambda = lambda do |name|
2\.   puts "Hello!"
3\.   puts "What's your name?"
4\.   puts "My name is #{name}."
5\. end
```

可以用 lambda 语法写一个多行 lambda，但是约定是在那种情况下使用常规的 lambda 关键字来提高可读性。

这基本上涵盖了如何写信和打电话给 lambdas，但如果我不涵盖他们的独特之处，他们的一等公民身份，我会给你带来伤害。

# 一等兰达斯

我们已经看到了兰达斯成为一等公民的一面。上面的每个例子都使用一个变量来存储 lambda 函数。我不能夸大它的重要性，因为它允许 lambda 函数作为参数传递和作为值返回。

让我们来看看如何做到这两点。

```
1\. def say_hello(first_name, last_name, lambda_arg)2\.   name = lambda_arg.call(first_name, last_name)3\.   puts "Hello my name is #{name}."4\. end5\. forward_lambda = lambda do |first, last|6\.   return "#{first} #{last}"7\. end8\.  backwards_lambda = lambda do |first, last|9\.    return "#{last}, #{first}"10\. end11\. say_hello "david", "polcari", forward_lambda12\. say_hello "david", "polcari", backwards_lambda=> Hello my name is david polcari. => Hello my name is polcari, david.
```

正如你在上面看到的，我们能够定义一个函数，创建两个独立的 lambdas，并在将它们传递给同一个函数之前将它们存储在变量中。

这是一个返回 lambda 的例子。

```
1\. def calculate_tax *subtotal*2\.   tax =  (subtotal * 0.0825).round(2)3\.   return lambda { puts tax }4\. end5\. first_tax = calculate_tax(10)6\. second_tax = calculate_tax(100)7\. first_tax.call8\. second_tax.call=> 0.83
=> 8.25
```

如果你想抽象出一些功能并存储起来，以便在程序执行的后期使用，这真的很有帮助。

这基本上概括了 Ruby lambdas 以及如何使用它们。希望你能意识到这些是多么的强大，并且能够在你未来的项目中实现它们。

顺便说一句，对于那些好奇的人来说，lambda 函数实际上是 Ruby 中 Proc 类的一个实例。过几周再回来看我的下一篇文章，讨论 Ruby 中 proc 和 lambda 的区别。

一如既往地在下面留下评论，让我知道你是如何在你自己的项目中使用 lambda 的！