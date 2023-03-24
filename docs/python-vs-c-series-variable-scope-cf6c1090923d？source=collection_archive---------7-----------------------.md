# Python vs C++系列:可变范围

> 原文：<https://medium.com/nerd-for-tech/python-vs-c-series-variable-scope-cf6c1090923d?source=collection_archive---------7----------------------->

每种编程语言都有自己定义作用域的方式，大部分都是类似的工作方式，都有类似的作用域级别，比如块作用域和函数作用域。本文是 [Python 与 C++系列](/nerd-for-tech/python-vs-c-series-getter-setter-and-property-e92d7801c21a)的一部分，将重点关注对于 C++背景的人来说不直观的特定 Python 范围规则。

(注意，本系列中的 Python 代码假设使用 Python 3.7 或更高版本)

# C++中的变量范围

C++中的作用域有几个级别，但一般有局部、全局和块作用域。

```
#include <iostream>

int global_variable = 0; // global variable

int myFunction(int parameter=0)
{
    // local variable can only be accessed within block.
    int local_variable = 0;

    if (parameter > 0)
    {
        local_variable += parameter;
    }
    else
    {
        // global variable can be accessed everywhere
        local_variable += global_variable; 
    }

    // update the global variable within a function
    global_variable = local_variable; 

    return local_variable;
}

double myFunction2()
{
    // local variable but has the same name as the global_variable.
    // In this case, the local one takes higher priority.
    double global_variable = 1.23;
    return global_variable;
}

void main()
{
    std::cout << global_variable << std::endl;
    // 0     The global global_variable
    std::cout << myFunction(10) << std::endl;
    // 10    The local_variable
    std::cout << global_variable << std::endl;
    // 10    The global global_variable updated by myFunction()
    std::cout << myFunction2() << std::endl;
    // 1.23  The local global_variable inside myFunction2()
    std::cout << global_variable << std::endl;
    // 10    The global global_variable was not affected by myFunction2()
}
```

# Python 中的变量范围

Python 也有几个作用域级别，其中大部分的工作方式类似于 C++和许多其他语言。然而，正如前一篇文章中所讨论的那样([可变、不可变和复制赋值](/nerd-for-tech/python-vs-c-series-mutable-immutable-and-copy-assignment-d95c0ea73879))，复制赋值并不创建新对象；相反，它绑定到一个对象。因此，在 Python 中使用赋值操作符又引出了另一个问题:赋值是创建一个新的对象来绑定，还是只是更新绑定到另一个对象，又是哪个呢？

根据官方文档— [Python 作用域和名称空间](https://docs.python.org/3/tutorial/classes.html#python-scopes-and-namespaces)，命名变量的搜索顺序如下(引用自[文档](https://docs.python.org/3/tutorial/classes.html#python-scopes-and-namespaces))

*   首先搜索的最内层范围包含本地名称
*   任何封闭函数的作用域(从最近的封闭作用域开始搜索)都包含非局部名称，但也包含非全局名称
*   倒数第二个范围包含当前模块的全局名称
*   最外层的范围(最后搜索的)是包含内置名称的名称空间

由于这个规则，一些非直觉的情况发生了，我们将在下面的小节中讨论这些情况。

# 控制语句

上面提到的搜索顺序不包括 *if 语句*等控制语句。因此，下面的代码是有效的和可行的。

```
# if-statement does not define a scope
condition = True
if condition:
    result = 1
else:
    result = 2

print(result)
# 1 
```

同样， *for-loop* (和 *while-loop* )， *with-statement，*和 *try-except* 也没有定义作用域。

```
# for-loop does not define a scope
for i in range(10):
    x = 1 + i

print(x)
# 10

# with-statement does not define a scope
with open("example.txt") as file:
    data = file.read()

print(data)
# Output from the example.txt

# try-except does not define a scope
try:
    raise ValueError("Test exception")

except ValueError:
    message = "Catch an exception"

print(message)
# Catch an exception
```

# 全局变量

第二种情况发生在使用全局变量时。正如我们所料，我们可以从任何地方访问一个全局变量。

```
global_variable = [1, 2, 3]   # global variable

def function1():
    print(global_variable)
    # [1, 2, 3]

function1()
```

然而，如果我们试图从函数或内部作用域更新全局变量，行为就会改变。举个例子，

```
global_variable = [1, 2, 3]   # global variable

def function2():
    global_variable = [2, 3, 4]   # local variable
    print(global_variable)
    # [2, 3, 4]
    print(hex(id(global_variable)))
    # 0x7f32763a4780

function2()
print(global_variable)
# [1, 2, 3]
print(hex(id(global_variable)))
# 0x7f32763f7880
```

在这个例子中，当我们将 *function2* 中的 *global_variable* 的值设置为*【2，3，4】*时，实际上是在 *function2* 的范围内创建了一个新的局部对象进行绑定，并不影响全局 *global_variable* 的任何内容。我们还可以使用内置函数 [id](https://docs.python.org/3/library/functions.html#id) 来验证两个 *global_variable* 变量是不同的对象(参见示例输出)。

**全局关键字**

如果在 Python 的函数中为变量赋值，默认情况下，它是一个局部变量。如果我们想在内部作用域(如 function)中访问一个全局变量，我们必须使用 [global](https://docs.python.org/3/reference/lexical_analysis.html#keywords) 关键字，并用它显式声明该变量。请参见下面的示例。

```
global_variable = [1, 2, 3]   # global variable

def function3():
    global global_variable
    global_variable = [3, 4, 5]
    print(global_variable)
    # [3, 4, 5]
    print(hex(id(global_variable)))
    # 0x7f32763a4780

function3()
print(global_variable)
# [3, 4, 5]
print(hex(id(global_variable)))
# 0x7f32763a4780
```

这次， *global* 关键字告诉我们 *function3* 中的 *global_variable* 与 global *global_variable* 绑定在一起，它们的地址显示它们是同一个对象。

此外，我们还可以使用 *global* 关键字从函数或内部作用域定义一个全局变量。

```
def function4():
    global new_global_variable
    new_global_variable = "A new global variable"
    print(new_global_variable)
    # A new global variable
    print(hex(id(new_global_variable)))
    # 0x7f32763a25d0

function4()
print(new_global_variable)
# A new global variable
print(hex(id(new_global_variable)))
# 0x7f32763a25d0
```

在 *function4* 中，我们用 *global* 关键字定义了 *new_global_variable* ，然后我们就可以从 *function4* 外部访问它了。

# 嵌套函数和非局部关键字

Python 提供了另一个关键字[非局部](https://docs.python.org/3/reference/lexical_analysis.html#keywords)，我们可以在嵌套函数中使用它。正如[命名变量](https://docs.python.org/3/tutorial/classes.html#python-scopes-and-namespaces)的搜索顺序规则所述，将首先搜索最里面的范围。因此，在嵌套函数的情况下，内部函数不能更新外部变量。

```
def outer_function1():
    variable = 1

    def inner_function1():
        variable = 2
        print(f"inner_function: {variable}")

    inner_function1()
    print(f"outer_function: {variable}")

outer_function1()
# The output of the variable:
# inner_function: 2
# outer_function: 1
```

正如我们所料， *inner_function1* 中的*变量*与 *outer_function1* 中的*变量*是不同的对象。

现在，让我们使用*非本地*关键字。关键字使变量引用最近范围内以前绑定的变量，并防止变量进行本地绑定。

```
def outer_function2():
    variable = 1

    def inner_function2():
        nonlocal variable
        variable = 2
        print(f"inner_function: {variable}")

    inner_function2()
    print(f"outer_function: {variable}")

outer_function2()
# The output of the variable:
# inner_function: 2
# outer_function: 2
```

*inner_function2* 中的*变量*与 *outer_function2* 中的*变量*绑定。

**全局与非局部**

*全局*和*非本地*的主要区别在于，*非本地*关键字只允许访问本地范围之外的下一个最近的范围，而*全局*关键字允许访问全局范围。

下面的例子有三层嵌套函数，我们在最内层使用了*非局部*关键字。最内层*函数*中变量 *x* 的变化只影响下一个最接近的作用域*内层*函数中的变量 x。

```
x = "hello world"

def outer_nonlocal():

    x = 0

    def inner():

        x = 1

        def innermost():
            nonlocal x
            x = 2
            print(f"innermost: {x}")

        innermost()
        print(f"inner: {x}")

    inner()
    print(f"outer_nonlocal: {x}")

outer_nonlocal()
print(f"global: {x}")
# The output of x:
# innermost: 2
# inner: 2
# outer_nonlocal: 0
# global: hello world
```

关于*全局*关键字，在*最里面的*函数中使用*全局*关键字的例子允许访问全局变量*y*；中间的变量 *y* (即 *outer_global* 函数和 *inner* 函数)不受影响。

```
y = "hello world"

def outer_global():

    y = 0

    def inner():

        y = 1

        def innermost():
            global y
            y = 2
            print(f"innermost: {y}")

        innermost()
        print(f"inner: {y}")

    inner()
    print(f"outer_global: {y}")

outer_global()
print(f"global: {y}")
# The output of y:
# innermost: 2
# inner: 1
# outer_global: 0
# global: 2
```

# 结论

范围是编程语言的一个基本概念，大多数编程语言的工作方式都类似。然而，由于 Python 中赋值操作符的工作方式以及命名变量规则的搜索顺序，在某些情况下 Python 作用域的工作方式与 C++非常不同。了解这个缺陷对于避免编写错误代码至关重要。

(所有示例代码也可在[变量范围](https://github.com/shunsvineyard/shunsvineyard/tree/main/python_vs_cpp_series/variable_scope)获得)

*原载于 2021 年 10 月 25 日*[*https://shunsvineyard . info*](https://shunsvineyard.info/2021/10/24/python-vs-c-series-variable-scope/)*。*