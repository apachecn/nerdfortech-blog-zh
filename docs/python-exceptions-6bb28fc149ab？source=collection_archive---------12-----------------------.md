# Python 中的异常处理

> 原文：<https://medium.com/nerd-for-tech/python-exceptions-6bb28fc149ab?source=collection_archive---------12----------------------->

异常是在 python 代码中发现错误时发生的事件。在本文中，您将了解如何引发异常，以及如何处理 python 代码中的错误。引发异常

我们可能希望在满足特定条件时抛出一个错误。要引发一个异常，我们使用 python 的 raise 关键字，后跟您想要引发的异常。例如

```
a = '54'
if type(a) != 'int':
    raise Exception('value of a must be integer')
```

*输出:*

```
Traceback (most recent call last):
  File "main.py", line 3, in <module>
    raise Exception('value of a must be integer')
Exception: value of a must be integer
```

现在，您不必一直创建自定义异常。Python 有一些内置的异常。这里是所有 python 异常的[完整列表](https://docs.python.org/3/library/exceptions.html)。其中之一，断言错误将在下面讨论。

## **断言错误异常**

我们不用等待程序失败，而是用 python 做一个断言。这将确保在执行任何进一步的代码之前满足特定的条件。比如有人用一些第三方库写了一些代码。目前，这些代码可能工作正常，但在将来，由于第三方库的变化，这种情况可能会改变。为了解决这个问题，我们通常断言所安装的库的版本(仅是我们的代码使用的那些，而不是全部)与代码产生时的版本相同。下面是示例代码

```
import numpy
assert (numpy.__version__ == '1.19.5')
```

如果 numpy 不是指定的版本，上面的代码将抛出一个错误。

## **处理异常**

为了处理异常，我们可以使用 try，except，else，finally 子句。Try 块包含您怀疑有错误的代码。Except 块包含在 try 块中发现任何异常时要执行的代码。Else 块包含在 try 块中没有发现异常时执行的代码。Finally 块包含不管 try 块中的异常如何都会执行的代码。下面是一个例子

```
try: 
    a, b = 10, 0
    print(a/b)except:
    print('Except block: this code is executed when there is an exception in try block')

else:
    print('Else block: this code is executed when there is no exception in try block')

finally:
    print('Finally block: this code is executed regardless of the exception is found or not')
```

*输出:*

```
Except block: this code is executed when there is an exception in try block
Finally block: this code is executed regardless of the exception is found or not
```

> 注意:-不需要使用所有这些方法，但是必须使用 try 和 except 块来处理异常。

在上面的示例中，无论执行哪个异常，except 块中的代码都会被执行。有时代码中可能有不同类型的异常，您可能希望以不同的方式处理它们:

```
import numpy
try: 
    assert (numpy.__version__ == '1.19.5')
    a, b = 10, 0
    print(a/b)

except AssertionError as error: 
    print(error)
    print('numpy is not of correct version')

except ZeroDivisionError as error:
    print(error)
    print('division by zero is not allowed')
```

如果 numpy 不是正确的版本，那么将执行第一个 except 块。输出将是:

```
 numpy is not of correct version
```

> 注意:first except 块的第一个 print 语句不打印任何内容

如果您的 numpy 是上面指定的版本，那么变量 a 除以变量 b 将产生一个错误，该错误将由 2nd 处理，但块和输出将是:

```
division by zero
division by zero is not allowed
```

希望这篇文章能帮助你理解 python 中的异常。