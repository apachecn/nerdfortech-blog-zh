# Python 中令人困惑的局部、非局部和全局变量

> 原文：<https://medium.com/nerd-for-tech/the-ever-confusing-local-nonlocal-and-global-variables-in-python-949b106c0243?source=collection_archive---------1----------------------->

![](img/12e3c9d1ff6c8500b173e0e19541dcf4.png)

如果你用 python 编程，我敢肯定你遇到过局部变量、非局部变量和全局变量。在这篇博客中，我用 python 中的例子解释了变量的作用域。

# 当地的

定义为函数参数的变量是函数的局部变量。换句话说，它们有一个局部范围，只能在声明它们的函数中使用。

抽样输出

```
Hi!, I am Batman. Nice to meet you!
Traceback (most recent call last):
  File "local_variable.py", line 6, in <module>
    print(f"Outside variable_scope function, superhero = {superhero}")
NameError: name 'superhero' is not defined
```

在函数 *variable_scope* 中，定义了变量*超级英雄*。因为它是在函数内部定义的，所以变量的范围是局部的，并且变量只在函数 *variable_scope* 内部可用。正如我们在示例输出中看到的，当函数 *variable_scope* 被调用时，函数 *variable_scope* 内部的 print 语句被执行，它打印变量 *superhero* 的值。然而，当我们试图在函数之外做同样的事情时，它失败了。

# 全球的

让我们稍微修改一下代码，在函数范围之外定义变量。

抽样输出

```
Outside variable_scope call, I am Batman!
Outside variable_scope call, I am Superman!
Hi!, I am Batman!. Nice to meet you!
Traceback (most recent call last):
  File "global_variable_error.py", line 12, in <module>
    variable_scope()
  File "global_variable_error", line 7, in variable_scope
    superhero2 += " And I am better than Batman"
UnboundLocalError: local variable 'superhero2' referenced before assignment
```

在上面的代码示例中，我们在程序主体中声明了两个变量*超级 1* 和*超级 2* 。因为变量是在主体中声明的，所以它们具有全局范围，并且在整个程序中都是可用的。在*变量作用域*函数中，我们尝试打印变量*超级 1* 的值，这是可行的。但是当我们试图修改*超级英雄 2* 的值时，却没有。这是因为当 python 编译 *variable_scope* 时， *superhero2* 变量的值被修改，因此被视为局部变量。

要修复这个错误，我们必须通知解释器这两个变量都是[全局](https://en.wikipedia.org/wiki/Global_variable)，尽管在函数内部有声明。我们通过使用关键字“*全局*”来做到这一点

抽样输出

```
Outside variable_scope call, I am Batman!
Outside variable_scope call, I am Superman!
Hi!, I am Batman!. Nice to meet you!
Variable_scope says, I am Superman! And I am better than Batman!
```

在上面的代码中*超级 1* 和*超级 2* 变量在 *variable_scope* 函数中被声明为全局变量。这样，当试图引用函数 *variable_scope* 中的变量时，python 解释器将这两个变量视为全局变量。

# 非局部的

让我们考虑嵌套函数，看看变量的范围。

抽样输出

```
Outer function says, I am Batman!
Traceback (most recent call last):
  File "nonlocal_error.py", line 9, in <module>
    outer_function()
  File "nonlocal_error.py", line 7, in outer_function
    inner_function()
  File "nonlocal_error.py", line 5, in inner_function
    superhero += " I am kidding!!"
UnboundLocalError: local variable 'superhero' referenced before assignment
```

在上面的代码示例中，我们有*外部函数*和*内部函数*。变量*超级英雄*在*外层函数*中声明。当调用 *inner_function* 时， *outer_function* 的作用域消失。这使得变量超级英雄的生活变得非常有趣！由于它没有在 *inner_function* 中声明，所以它不能是局部变量。而且它也不可能是全局的，因为它没有被定义为全局的(咄！).在这种情况下，*超级英雄*是一个[自由变量](https://en.wikipedia.org/wiki/Free_variables_and_bound_variables)，因为它没有绑定到局部范围。当我们试图修改变量*超级英雄*的值时，我们隐式地创建了一个局部变量*超级英雄*。

为了解决上面代码中的问题，让我们在 *inner_function* 中声明*超级英雄*变量[非本地变量](https://en.wikipedia.org/wiki/Non-local_variable)。

抽样输出

```
Outer function says, I am Batman!
Inner function says, I am Batman! I am kidding!!
```

声明一个非局部变量，将它标记为自由变量，即使我们试图在函数内部赋值。Python 将自由变量的名称保存在 __code__ 属性中。为了列出我们可以检查返回 *inner_function* 对象的 *fn_obj* 。

```
>>> fn_obj.__code__.co_freevars
('superhero',)
```

谢谢你的时间。这是 python 中有趣主题的基础，比如闭包和装饰器，我们将在下一篇博客中看到。

还有，蝙蝠侠比超人强！