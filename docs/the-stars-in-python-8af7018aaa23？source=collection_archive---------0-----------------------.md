# * Python 中的星星**

> 原文：<https://medium.com/nerd-for-tech/the-stars-in-python-8af7018aaa23?source=collection_archive---------0----------------------->

*python 中的 args 和 kwargs*

![](img/f776bf5daf1fa7da5d50f064e3afec7c.png)

python 中的可变长度参数

## 用 python 打印列表

假设我们想要打印下面的列表。

```
list_of_numbers = [1, 2, 4, 5, 6, 7]
```

1.  第一种打印方式是遍历列表并打印每一个

```
list_of_numbers = [1, 2, 4, 5, 6, 7]for number in list_of_numbers:
    print(number, end=" ")----------------------------------------------OUTPUT:1 2 4 5 6 7
```

2.更好的方法是使用**列表理解**

```
list_of_numbers = [1, 2, 4, 5, 6, 7][print(number, end=" ") for number in list_of_numbers]----------------------------------------------OUTPUT:1 2 4 5 6 7
```

3.最好是使用**扩展操作符**

```
list_of_numbers = [1, 2, 4, 5, 6, 7]print(*list_of_numbers)----------------------------------------------OUTPUT:1 2 4 5 6 7
```

需要用逗号分隔元素吗？在打印功能中设置 sep 参数

```
list_of_numbers = [1, 2, 4, 5, 6, 7]print(*list_of_numbers, sep=",")----------------------------------------------OUTPUT:1,2,4,5,6,7
```

## 向函数传递可变长度的参数

在 python 中，向函数传递任意数量的参数的不同方法是:

1.  将列表传递给函数。

列表可以打包任意数量的值传递给函数。

```
def print_parameters(arg=[]):
    if(len(arg) == 0):
        print("No parameters have been passed")
        return
    print(*arg, sep=",")print("\nSingle parameter:")
print_parameters([10])print("\nMultiple Parameter:")
print_parameters([1, 2, 4, 5, 6, 7])print("\nEmpty parameter:")
print_parameters()------------------------------------------
OUTPUT:Single parameter:
10Multiple Parameter:
1,2,4,5,6,7Empty parameter:
No parameters have been passed
```

在上面的方法中，即使只传递一个参数，也需要将其封装在一个列表中。如果该函数接受*位置参数*，就可以避免这种情况。

2.位置参数也称为*星形参数*可以接受一个、多个甚至零个函数参数。甚至这些值也不需要属于同一类型。

```
def print_parameters(*arg):
    if(len(arg) == 0):
        print("No parameters have been passed")
        return
    print(*arg, sep=",")print("\nSingle parameter:")
print_parameters(10)print("\nMultiple Parameter:")
print_parameters(1, 2, 4, 5, 6, 7)print("\nEmpty parameter:")
print_parameters()------------------------------------------
OUTPUT:Single parameter:
10Multiple Parameter:
1,2,4,5,6,7Empty parameter:
No parameters have been passed
```

有了位置参数，就不再需要将参数打包到一个列表中。位置参数是一个 n 元组参数，其中 n 是传递给函数的参数个数。

```
def function_name(*arg):
    print("type of arg = ", type(arg))function_name()--------------------------------------------
OUTPUT:type of arg =  <class 'tuple'>Empty parameter:
No parameters have been passed
```

如果参数列表太长，python 提供了一种方法来命名传递给函数的参数，从而使值不依赖于位置。这使得函数更具可读性。

3.命名参数:与位置参数类似，python 也提供命名参数。命名的参数以**双星形开头。**

```
def function_name(**kwargs):
    print("A : ", kwargs['A'])
    print("B : ", kwargs['B'])
    print("C : ", kwargs['C'])function_name(A=65, B=66, C=67)--------------------------------------
OUTPUT:A :  65
B :  66
C :  67
```

要迭代命名参数，请使用 kwargs.items()

```
def function_name(**kwargs):
    for name, value in kwargs.items():
        print(name, ": ", value)function_name(A=65, B=66, C=67)--------------------------------------
OUTPUT:A :  65
B :  66
C :  67
```

args 和 kwargs 都可以和常规参数一起传递给函数。

```
def function_name(reg_para, *args, **kwargs):
    print("Regular parameter: ", reg_para)
    print("Positional parameters: ", *args)
    print("Named parameters: ",
    *[name + ": " + str(value) for name, value in kwargs.items()])function_name("A string", 1, 2, 3, A=65, B=66, C=67)--------------------------------------
OUTPUT:Regular parameter:  A string
Positional parameters:  1 2 3
Named parameters:  A: 65 B: 66 C: 67
```

PS:命名参数应该只在位置参数之后传递，位置参数应该只在常规参数之后传递。

这是对 python 中经常使用的 args 和 kwargs 的快速概述。

参考资料:

[](https://www.datacamp.com/community/tutorials/usage-asterisks-python) [## Python 中星号的用法

### 我们大多数人使用星号作为乘法和幂运算符，但它们也有广泛的运算被…

www.datacamp.com](https://www.datacamp.com/community/tutorials/usage-asterisks-python) [](https://www.informit.com/articles/article.aspx?p=2314818) [## 有效的 Python:函数参数的 4 个最佳实践

### Python 中的函数有各种各样的额外特性，使程序员的生活变得更容易。有些类似于…

www.informit.com](https://www.informit.com/articles/article.aspx?p=2314818) [](https://jbencook.com/object-spread-operator-python/) [## Python 的对象扩展运算符

### 假设您有一本既想复制又想更新的词典。在 JavaScript 中，这是一种常见的模式

jbencook.com](https://jbencook.com/object-spread-operator-python/) [](https://stackoverflow.com/questions/2921847/what-does-the-star-operator-mean-in-a-function-call) [## 在函数调用中，星号是什么意思？

### 星号*将序列/集合解包为位置参数，因此您可以这样做:def sum(a，b): return…

stackoverflow.com](https://stackoverflow.com/questions/2921847/what-does-the-star-operator-mean-in-a-function-call) [](https://stackoverflow.com/questions/36901/what-does-double-star-asterisk-and-star-asterisk-do-for-parameters) [## **(双星/星号)和*(星号/星号)对参数有什么作用？

### 它们允许将函数定义为接受，并允许用户传递任意数量的参数，位置(*)和…

stackoverflow.com](https://stackoverflow.com/questions/36901/what-does-double-star-asterisk-and-star-asterisk-do-for-parameters)