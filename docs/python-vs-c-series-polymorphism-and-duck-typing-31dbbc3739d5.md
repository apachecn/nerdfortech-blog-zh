# Python vs C++系列:多态性和 Duck 类型

> 原文：<https://medium.com/nerd-for-tech/python-vs-c-series-polymorphism-and-duck-typing-31dbbc3739d5?source=collection_archive---------7----------------------->

这是 [Python vs C++系列](/nerd-for-tech/python-vs-c-series-getter-setter-and-property-e92d7801c21a)的第二篇文章。在本文中，我们将讨论另一个基本的面向对象编程概念——多态性。

(注意，本系列中的 Python 代码假设使用 Python 3.7 或更高版本)

# 多态性的简要回顾

多态性是一个希腊词，意思是有多种形式。支持多态性的编程语言意味着变量、函数或对象可以采用多种形式，比如函数接受不同类型的参数。此外，使用多态性，我们可以用相同的名称(即相同的接口)定义函数，但是这些函数有多个实现。

# C++中的多态性

C++支持静态(编译时解析)和动态(运行时解析)多态性。

在 C++中，静态多态性也称为函数重载，允许程序声明多个同名但参数不同的函数。下面展示了一个 C++中函数重载的例子。

```
#include <string>

void myOverloadingFunction(int parameter)
{
    // Do something
}

void myOverloadingFunction(std::string parameter)
{
    // Do something
}

void myOverloadingFunction(int parameter1, std::string parameter2, float parameter3)
{
    // Do something
}
```

(示例代码可在[CPP _ function _ overloading . CPP](https://github.com/shunsvineyard/shunsvineyard/blob/main/python_vs_cpp_series/polymorphism_and_duck_typing/cpp_function_overloading.cpp)获得)

# 虚函数和抽象类

通过使用具有虚函数的类层次结构，C++实现了运行时多态性。当派生类中的方法重写其基类时，要调用的方法由运行时对象的类型决定。下面的代码展示了它在 C++中是如何工作的。

```
#include <memory>

class BaseClass
{
    public:
        virtual void doWork()
        {
            // do some work
        }
};

class DerivedClassA: public BaseClass
{
    public:
        virtual void doWork() override
        {
            // do some work
        }
};

class DerivedClassB: public BaseClass
{
    public:
        virtual void doWork() override
        {
            // do some work
        }
};

void myFunction(std::shared_ptr<BaseClass> p)
{
    // The appropriate doWork() to be called will be determined by
    // the instance of p at the runtime.
    p->doWork();
}
```

(示例代码可在 [cpp_virtual_function.cpp](https://github.com/shunsvineyard/shunsvineyard/blob/main/python_vs_cpp_series/polymorphism_and_duck_typing/cpp_virtual_function.cpp) 获得)

**接口和纯虚函数**

当虚函数追加 *= 0* 后，就变成了纯虚函数，包含纯虚函数的类称为抽象类。如果支持实例化，任何从抽象类派生的类都必须定义它的纯虚函数。因此，我们通常使用抽象类来定义程序的接口。举个例子，

```
class MyInterface
{
    // Since this class contains a pure virtual class; it becomes an abstract
    // class, and cannot be instantiated.
    public:
        // Use a pure virtual function to define an interface.
        virtual int method(int parameter) = 0;
};

class DerivedClass: public MyInterface
{
    public:
        // If the derived class needs to be instantiated, the derived class
        // must implement its parent's pure virtual function.
        virtual int method(int parameter) override
        {
            // do something
        }
};
```

(示例代码可从 [cpp_interface.cpp](https://github.com/shunsvineyard/shunsvineyard/blob/main/python_vs_cpp_series/polymorphism_and_duck_typing/cpp_interface.cpp) 获得)

# Python 中的鸭子打字

鸭子分类是鸭子测试的一个应用，它说，“如果它看起来像鸭子，像鸭子一样游泳，像鸭子一样嘎嘎叫，那么它可能是一只鸭子。”使用 duck 类型，函数不检查参数的类型；相反，它会检查参数是否存在。Duck typing 是 Python 这样的动态语言的精髓。

我们在 C++中需要多态性的一个原因是 C++是一种静态语言。多态允许我们用相同的名字定义函数，但是使用不同的类型参数或者不同数量的参数。相反，Python 是一种动态语言。所以函数重载对于 Python 来说不是必须的，也不被支持(见 [PEP3142](https://www.python.org/dev/peps/pep-3124/) )。以下示例显示了 Python 函数如何处理不同类型的参数。

```
def my_function(parameter):

    if type(parameter) is str:
        print("Do something when the type is string")

    elif type(parameter) is int:
        print("Do something when the type is integer")

    else:
        raise TypeError("Invalid type")

if __name__ == "__main__":
    # Valid
    my_function(10)
    my_function("hello")

    # TypeError exception will be thrown
    my_function(2.3)
```

(示例代码可在 [python_overloading_1.py](https://github.com/shunsvineyard/shunsvineyard/blob/main/python_vs_cpp_series/polymorphism_and_duck_typing/python_overloading_1.py) 获得)

在这个例子中， *my_function* 由于 duck 类型的性质，可以接受任何参数。因此，如果我们希望函数根据参数的类型执行不同的操作，函数需要检查参数是否存在，以确定要做什么。该示例的输出如下所示。

```
Do something when the type is integer
Do something when the type is string
Traceback (most recent call last):
    ….
    raise TypeError("Invalid type")
TypeError: Invalid type
```

**如果我们用同一个名字定义多个函数会怎么样？**

如果我们定义了两个或更多同名的函数，Python 解释器将使用它扫描的最后一个函数。例如，下面的代码可以工作，但是只使用最后一个 *my_function* 。

```
def my_function(parameter):

    if type(parameter) is str:
        print("Do something when the type is string")

    elif type(parameter) is int:
        print("Do something when the type is integer")

    else:
        raise TypeError("Invalid type")

def my_function(parameter):
    print(parameter)

if __name__ == "__main__":
    my_function(10)
    my_function(2.3)
```

(示例代码可从[python _ overloading _ 2 . py](https://github.com/shunsvineyard/shunsvineyard/blob/main/python_vs_cpp_series/polymorphism_and_duck_typing/python_overloading_2.py)获得)

它的输出如下所示。

```
10 
2.3
```

最后一个 *my_function(parameter)* 才是真正被调用的，这也是为什么 *my_function(2.3)* 起作用的原因。

**带可选参数的函数**

除了定义同名但参数类型不同的函数之外，通过多态，我们还可以定义同名但参数数量不同的函数。Python 不支持函数重载，但是它的[关键字参数](https://docs.python.org/3/tutorial/controlflow.html#keyword-arguments)和[默认参数](https://docs.python.org/3/tutorial/controlflow.html#default-argument-values)能力提供了一种定义接受不同数量参数的函数的方法。下面的代码片段演示了关键字参数和默认参数的用法。

```
def my_function(parameter1, parameter2=None, parameter3="hello"):
    print(parameter1)
    if parameter2:
        print(parameter2)
    print(parameter3)

if __name__ == "__main__":
    # Use default parameter2 and parameter3; parameter 1 does not
    # have default value, so it cannot be omitted.
    my_function(10)

    # Use default parameter2
    my_function(parameter1=1, parameter3=5)

    # Use default parameter3; also notice that the order does not matter
    # when using keyword arguments.
    my_function(parameter2="world", parameter1=1)
```

(示例代码可在 [python_overloading_3.py](https://github.com/shunsvineyard/shunsvineyard/blob/main/python_vs_cpp_series/polymorphism_and_duck_typing/python_overloading_3.py) 获得)

*my_function(10)的输出*

```
10
hello
```

*my_function(参数 1=1，参数 3=5)* 的输出

```
1
5
```

*my _ function(parameter 2 = " world "，parameter1=1)* 的输出

```
1
world
hello
```

**使用鸭式打字时的缺点**

使用 duck typing，Python 允许我们用不同的类型和不同数量的参数编写一个函数。然而，当我们需要函数基于参数执行某些操作时，我们将需要针对每种类型的几个 if-else 语句，如果 if-else 语句越来越长，其可读性就会降低。当这种情况发生时，我们可能需要考虑重构我们的代码。Python 没有函数重载的好处，我们可以像在 C++中一样，利用函数重载使用几个小函数来执行不同的操作。

# 类型安全和类型检查

虽然 Python 是一种动态语言，但这并不意味着 Python 不关心类型安全。与 C++不同，编译器会捕捉大多数与类型相关的错误，Python 依靠林挺工具来完成这项工作。

从 Python 3.5 开始， [PEP484](https://www.python.org/dev/peps/pep-0484/) 引入了对类型提示的支持，类型检查工具如 [mypy](http://mypy-lang.org/) 可以利用这些提示来检查 Python 程序。下面的示例显示了类型提示的示例。

(Python 类型提示的更多细节可以在[对类型提示的支持](https://docs.python.org/3/library/typing.html)中找到)

```
from typing import Dict, Optional, Union

class MyClass:

    def my_method_1(self, parameter1: int, parameter2: str) -> None:
        pass

    def my_method_2(self, parameter: Union[int, str]) -> Dict:
        pass

def my_function(parameter: Optional[MyClass]):
    pass
```

(示例代码可从 [python_type_hints_1.py](https://github.com/shunsvineyard/shunsvineyard/blob/main/python_vs_cpp_series/polymorphism_and_duck_typing/python_type_hints_1.py) 获得)

尽管我们可以使用类型提示和林挺工具来确保类型安全，但是 Python 运行时并不强制函数或变量来满足其类型注释。如果我们忽略类型检查器生成的错误或警告，并且仍然传递无效的类型参数，Python 解释器仍然会执行它。

比如下面例子中的 *my_function* 期望*参数 1* 为 *int* 类型，*参数 2* 为*字符串*类型。但是，当函数被调用并且*参数 1* 的类型为*字符串*并且*参数 2* 为*浮点*时，Python 解释器会执行它。

```
def my_function(parameter1: int, parameter2: str) -> None:
    print(f"Parameter 1: {parameter1}")
    print(f"Parameter 2: {parameter2}")

if __name__ == "__main__":
    my_function(parameter1="Hello", parameter2=3.5)
```

(示例代码可从 [python_type_hints_2.py](https://github.com/shunsvineyard/shunsvineyard/blob/main/python_vs_cpp_series/polymorphism_and_duck_typing/python_type_hints_2.py) 获得)

如果我们运行类型检查器(在例子中使用 *mypy* ，它将显示不兼容的类型错误。(以 mypy 为例)

```
$ mypy python_type_hints_2.py 
python_type_hints_2.py:12: error: Argument "parameter1" to "my_function" has incompatible type "str"; expected "int"
python_type_hints_2.py:12: error: Argument "parameter2" to "my_function" has incompatible type "float"; expected "str"
Found 2 errors in 1 file (checked 1 source file)
```

然而，如果我们执行代码，它仍然会工作。

```
$ python python_type_hints_2.py 
Parameter 1: Hello
Parameter 2: 3.5
```

Python 类型提示仅用于林挺工具检查类型，但对运行时没有影响。

# 抽象基类和接口

Python 不像 C++那样有虚函数或者接口的概念。然而，Python 提供了一个基础设施，允许我们构建类似于接口功能的东西— [抽象基类](https://docs.python.org/3/library/abc.html) (ABC)。

ABC 是一个不需要提供具体实现的类，但用于两个目的:

1.  检查隐式接口兼容性。ABC 定义了一个类的蓝图，可以用来检查类型兼容性。这个概念类似于 c++中抽象类和虚函数的概念。
2.  检查实现的完整性。ABC 定义了一组派生类必须实现的方法和属性。

在下面的例子中， *BasicBinaryTree* 定义了二叉树的接口。任何派生的二叉树(如 AVL 树或二叉查找树)必须实现*基础二叉树*中定义的方法。

要使用 ABC 定义一个接口，接口类需要继承助手类— [abc。ABC](https://docs.python.org/3/library/abc.html#abc.ABC) 。派生类必须实现的方法使用[@ ABC . abstract method](https://docs.python.org/3/library/abc.html#abc.abstractmethod)decorator(相当于 C++中的纯虚函数)

```
class BasicBinaryTree(abc.ABC):

    @abc.abstractmethod
    def insert(self, key: int) -> None:
        raise NotImplementedError()

    @abc.abstractmethod
    def delete(self, key: int) -> None:
        raise NotImplementedError()

    @abc.abstractmethod
    def search(self, key: int) -> Node:
        raise NotImplementedError()
```

派生类(以 AVLTree 为例)继承了抽象基类(即本例中的 *BasicBinaryTree* )并实现了用*@ ABC . abstactmethod*decorator 定义的方法。

```
class AVLTree(BasicBinaryTree):
    """AVL Tree implementation."""

    def insert(self, key: int) -> None:
        # The AVL Tree implementation
        pass

    def delete(self, key: int) -> None:
        # The AVL Tree implementation
        pass

    def search(self, key: int) -> AVLNode:
        # The AVL Tree implementation
        pass
```

二叉树接口的完整例子如下(也可以在 [python_interface.py](https://github.com/shunsvineyard/shunsvineyard/blob/main/python_vs_cpp_series/polymorphism_and_duck_typing/python_interface.py) 获得)。

```
import abc

from typing import Optional
from dataclasses import dataclass

@dataclass
class Node:
    """Basic binary tree node definition."""

    key: int
    left: Optional["Node"] = None
    right: Optional["Node"] = None
    parent: Optional["Node"] = None

class BasicBinaryTree(abc.ABC):
    """An abstract base class defines the interface for any type of binary trees.

    The derived class should implement the abstract method defined in the abstract
    base class.
    """

    @abc.abstractmethod
    def insert(self, key: int) -> None:
        raise NotImplementedError()

    @abc.abstractmethod
    def delete(self, key: int) -> None:
        raise NotImplementedError()

    @abc.abstractmethod
    def search(self, key: int) -> Node:
        raise NotImplementedError()

class BinarySearchTree(BasicBinaryTree):
    """Binary Search Tree."""

    def insert(self, key: int) -> None:
        # The BST implementation
        pass

    def delete(self, key: int) -> None:
        # The BST implementation
        pass

    def search(self, key: int) -> Node:
        # The BST implementation
        pass

@dataclass
class AVLNode(Node):
    """AVL Tree node definition. Derived from Node."""

    left: Optional["AVLNode"] = None
    right: Optional["AVLNode"] = None
    parent: Optional["AVLNode"] = None
    height: int = 0

class AVLTree(BasicBinaryTree):
    """AVL Tree implementation."""

    def insert(self, key: int) -> None:
        # The AVL Tree implementation
        pass

    def delete(self, key: int) -> None:
        # The AVL Tree implementation
        pass

    def search(self, key: int) -> AVLNode:
        # The AVL Tree implementation
        pass
```

# 结论

Python 对多态性的支持可能与 C++不同，但多态性的概念仍然有效，并在 Python 程序中广泛使用，类型安全的重要性对于 Python 程序仍然至关重要。

*原载于 2021 年 10 月 10 日*[*https://shunsvineyard . info*](https://shunsvineyard.info/2021/10/10/python-vs-c-series-polymorphism-and-duck-typing/)*。*