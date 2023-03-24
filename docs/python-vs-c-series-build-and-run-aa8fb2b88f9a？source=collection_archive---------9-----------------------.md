# Python vs C++系列:构建和运行

> 原文：<https://medium.com/nerd-for-tech/python-vs-c-series-build-and-run-aa8fb2b88f9a?source=collection_archive---------9----------------------->

继上一篇文章[开发环境设置和虚拟环境](https://shunsvineyard.medium.com/python-vs-c-series-development-environment-setup-and-virtual-environment-8df69b78bb06)之后，本文不讨论语言差异，而是关注项目构建及其使用。写代码的目的是为了使用它。在 C++中，当我们开始编写代码时(甚至在此之前)，我们需要决定如何使用代码(例如，作为可执行的二进制文件或库)，然后根据我们的需要构建代码。Python 里是这样吗？怎么做呢？当我开始学习 Python 时，我认为 Python 是一种脚本语言，只能作为脚本运行。这不是真的。Python 代码可以像 C++和许多其他语言一样作为库或可执行文件来构建和运行。然而，用 Python 来做这件事的方法是完全不同的，术语也是如此。我希望有人能告诉我如何使用我在开始学习 Python 时已经熟悉的概念或语言来做到这一点。这将使学习过程更加简单。这就是本文的目的。

(注意，本系列中的 Python 代码假设使用 Python 3.7 或更高版本)

# 构建 C++程序

在我们开始编写 C++程序之前，我们需要决定我们将构建什么，通常，我们有两个选择——可执行文件或库。可执行文件的典型项目布局可能如下所示，其中包括 C++源代码和一个构建文件，如果使用 [CMake](https://cmake.org/) 的话， *CMakeLists.txt* 。

```
MyExecutableProject
├── CMakeLists.txt
├── include
│   └── MyLib.hpp
└── src
    ├── Main.cpp
    └── MyLib.cpp
```

假设文件的内容如下。

*MyLib.hpp*

```
class MyLib
{
public:
    static int add(int, int);
};
```

*MyLib.cpp*

```
#include "MyLib.hpp"

int MyLib::add(int a, int b)
{
    return (a + b);
}
```

*CMakeLists.txt*

```
cmake_minimum_required (VERSION 3.10)
project(MyExecutable)

set(SOURCES
    ${PROJECT_SOURCE_DIR}/src/MyLib.cpp
    ${PROJECT_SOURCE_DIR}/src/Main.cpp
)

include_directories(${PROJECT_SOURCE_DIR}/include)

add_executable(myexe ${SOURCES})
```

对于这个 C++程序，我们需要编译和构建它的代码。一旦编译完成，就会生成一个可执行的二进制文件 *myexe* 。然后，我们可以通过发出*来运行它。/myexe* 如果我们在 Linux 中运行代码。

```
$ ./myexe 
10 + 20 = 30
```

**C++库**

构建 C++库类似于 C++可执行项目，同样需要 C++代码和构建文件。

```
MyLibraryProject
├── CMakeLists.txt
├── include
│   └── MyLib.hpp
└── src
    └── MyLib.cpp
```

主要区别是我们需要通过构建工具将代码构建为库，比如 [CMake](https://cmake.org/) ，所以我们在 *CMakeLists.txt* 中定义它，告诉编译器如何将项目构建为库(下面的例子假设我们构建了一个静态库)。

```
cmake_minimum_required (VERSION 3.10)
project(MyLib)

set(SOURCES ${PROJECT_SOURCE_DIR}/src/MyLib.cpp)

include_directories(${PROJECT_SOURCE_DIR}/include)

add_library(mylib STATIC ${SOURCES})
```

在我们构建库项目之后，将会生成 *libmylib.a* ，我们的代码可以通过包含它的头文件并链接到库来使用它。

# 构建并运行 Python 程序

上一节展示了我们在 C++中通常做的事情。我们能用 Python 做同样的事情吗？我们需要这样做吗？如何去做？答案是肯定的，只是方式不同。

Python 是一种解释型语言，所以用 Python 编写的程序可以直接由 Python 解释器执行，无需编译。因此，将 Python 代码编写为脚本并作为脚本运行是最明显的选择，尤其是当 Python 程序像单个文件一样简单时。

# 作为脚本运行

假设我们有一个名为 *script.py* 的 Python 脚本，其内容如下。

```
def add(a: int, b: int) -> int:
    return a + b

number_1 = 10
number_2 = 20

result = add(a=number_1, b=number_2)

print(f"{number_1} + {number_2} = {result}")
```

将它作为脚本运行可以像下面这样简单。

```
$ python script.py 
10 + 20 = 30
```

Python 解释器将从头到尾执行脚本。

除此之外，通过添加 shebang， *#！/usr/bin/env python* ，在我们脚本的顶部，python 脚本可以作为一个 shell 脚本在 Linux 系统中执行。

```
#!/usr/bin/env python

def add(a: int, b: int) -> int:
    return a + b

number_1 = 10
number_2 = 20

result = add(a=number_1, b=number_2)

print(f"{number_1} + {number_2} = {result}")
```

我们也可以去掉它的*。py* 扩展，使它看起来像一个 shell 脚本，这仍然有效。

```
$ chmod +x script.py
$ mv script.py script
$ ./script
10 + 20 = 30
```

注意， *#！/usr/bin/env python* 使用 *env* 命令指定 python 解释器的路径，该命令将通过查看 *PATH* 环境变量来执行 Python 解释器。除了使用 *env* 命令，我们还可以在 shebang 中使用绝对路径来指定 Python 解释器，例如 *#！/usr/local/bin/python* 。

# Python 模块

在 C++中，我们有头文件和 CPP 文件，但是在 Python 中，只有一种类型。py 文件。此外，包含 Python 定义和语句的文件被称为[模块](https://docs.python.org/3/tutorial/modules.html)。因此，每一个*。py* 文件是一个模块；Python 脚本也是一个模块。

但是，模块和脚本的思想是不一样的。模块和脚本的主要区别在于，脚本被设计为直接执行，而模块被设计为[导入](https://docs.python.org/3/reference/import.html)。但这是概念上的区别；我们仍然可以导入 Python 脚本。因此，我们不需要指定我们的 Python 代码是库还是可执行文件——每个 Python 模块都是可导入和可执行的。此外，运行 Python 模块的方法不止一种。以下示例将展示运行 Python 模块的不同方式。

假设我们有一个名为 *mylib.py* 的 Python 模块，其内容如下。

```
def add(a: int, b: int) -> int:
    return a + b

if __name__ == "__main__":
    number_1 = 5
    number_2 = 7
    result = add(a=number_1, b=number_2)
    print("Running from the module...")
    print(f"{number_1} + {number_2} = {result}")
```

第一种方式是使用带有模块文件名的 Python 解释器，就像运行脚本一样(即 *python mylib.py* )。另一种方法是使用 [-m](https://docs.python.org/3/using/cmdline.html#cmdoption-m) 选项。当使用这个选项时，Python 解释器将在 [sys.path](https://docs.python.org/3/library/sys.html#sys.path) 中搜索指定的模块并执行它。通常，模块名是不带*的 Python 文件。py* 扩展。

```
$ python -m mylib 
Running from the module... 5 + 7 = 12
```

同样的 *mylib.py* 模块也可以作为库导入。假设我们要导入 *mylib.py* 的代码名为 *main.py* 并且两个模块在同一个文件夹中，那么 *main.py* 可以导入 *mylib.py* 模块如下。

```
import mylib

if __name__ == "__main__":
    number_1 = 10
    number_2 = 20
    result = mylib.add(a=number_1, b=number_2)
    print("Running from main...")
    print(f"{number_1} + {number_2} = {result}")
```

然后，我们可以运行 *main.py* 模块，如前面的示例所示。

```
$ ls
main.py mylib.py
$ python main.py 
Running from main...
10 + 20 = 30
```

# 为什么要使用 if __name__ == "__main__ "语句？

在前面的示例中需要注意的一件重要事情是，我们在 *mylib.py* 模块中添加了 *if __name__ == "__main__"* 语句。为什么？

使用此 if 语句是为了防止在将代码作为模块导入时执行代码。要理解这一点，我们需要知道什么是*_ _ 名 __* 。

*__name__* 是每个 Python 模块都有的属性，是模块用来标识自己的字符串。换句话说，它是模块执行时的名字。然而，该值并不总是相同的，而是取决于模块是如何执行的。模块导入时，其 *__name__* 为模块名称(即*)。没有*的 py* 文件的文件名。py* )。但是，当模块作为可执行文件执行时，它的 *__name__* 变成了 *__main__* 。

[__main__](https://docs.python.org/3/library/__main__.html) 是顶层代码运行的环境名称。顶层代码是开始运行的第一个用户指定的 Python 模块。就像 C++程序中的入口点 *main()* 。所以当我们把一个模块作为可执行文件运行时，它就变成了顶层环境，所以它的 *__name__* 就变成了 *__main__* 。

另外，_ *_name__* 是模块内部的全局变量，是模块外部的属性。换句话说，可以在模块内部或外部访问它。因此，我们可以通过打印该值来使用它，以演示一个模块的 *__main__* 在导入和执行时是如何分配的。

假设我们有一个名为 *mymodule.py* 的模块，内容如下。

```
def greeting(name: str) -> None:
    print(f"Hello, {name}")

print(__name__)
```

当我们使用 Python 解释器导入 *mymodule.py* 模块时， *print* 函数会显示 *__name__* 的值为 *mymodule* 。

```
>>> import mymodule 
mymodule
```

现在，如果我们将模块作为脚本运行，它的 *__name__* 值就变成了 *__main__* 。

```
$ python mymodule.py 
__main__
```

有了对 *__name__* 和 *__main__* 的理解，我们就可以解释为什么在 *mylib.py* 模块中使用 *if __name__ == "__main__"* 语句，以及它是如何阻止导入时执行代码的:if-statement 块下的代码只有在模块作为顶层代码执行时才会运行。我们在本节开始时已经看到了它是如何工作的——当 *mylib.py* 示例被导入时，从模块……运行的*没有被执行。我们还可以检查如果没有 if 语句会发生什么。假设我们有 *mylib2.py* 模块，它与 *mylib.py 几乎相同，*除了 *mylib2.py* 没有 *if __name__ == "__main__"* 语句。*

```
def add(a: int, b: int) -> int:
    return a + b

number_1 = 11
number_2 = 17
result = add(a=number_1, b=number_2)
print("Running from the mylib2...")
print(f"{number_1} + {number_2} = {result}")
```

以及导入 *mylib2.py* 的模块 *main2.py* 。

```
import mylib2

number_1 = 23
number_2 = 29
result = mylib2.add(a=number_1, b=number_2)
print("Running from main2...")
print(f"{number_1} + {number_2} = {result}")
```

当我们运行 *main2.py* 时，即使 *mylib2.py* 刚刚被导入，两个“Running from…”语句都会被执行。

```
$ ls 
main2.py  mylib2.py
$ python main2.py 
Running from the mylib2...
11 + 17 = 28
Running from main2...
23 + 29 = 52
```

因此，如果我们的模块包含只有当模块在顶级环境中运行时才应该运行的代码，我们应该始终将代码放在 *if __name__ == "__main__"* 语句下。

# Python 包

到目前为止，我们只讨论了简单的案例，但是我们不可能总是把所有的事情都放在一两个模块中。当我们的项目变得更加复杂时，我们必须正确地组织代码。Python 构造模块的方式叫做[包](https://docs.python.org/3/tutorial/modules.html#packages)。Python 包只是一个包含 Python 模块的文件夹。包方法通过避免模块之间的命名冲突，帮助我们很好地组织 Python 代码和很好地构造 Python 模块的名称空间。一个简单的包示例可能如下所示。

```
mypackage
├── __init__.py
└── mylibrary
    ├── __init__.py
    └── module.py
```

一个包可以包含另一个包；一个包也可以由多个包和模块组成。上面的例子显示我们有一个名为 *mypackage* 的包，在 *mypackage* 中，我们有另一个包 *mylibrary* ，它包含了提供实际功能的模块。另外，每个文件夹都有一个 __init__。py 文件。 *__init__。py* 文件是 Python 将包含该文件的目录视为包所必需的(详见[常规包](https://docs.python.org/3/reference/import.html#regular-packages))。很多时候， *__init__。py* 可以只是一个空文件。

像 Python 模块一样，Python 包可以作为库导入，也可以作为可执行文件调用。

# 将包作为库导入

将包作为库导入与导入模块是一样的。唯一的区别是我们需要将包包含在导入路径中。

假设 *mypackage* 中 *module.py* 的内容有以下内容(使用上面的例子)。

```
def add(a: int, b: int) -> int:
    return a + b
```

在 *mypackage* 包之外，我们有一个 *main.py* 包，我们希望导入该模块并像库一样使用它。由于*模块*模块是 *mylibrary* 包的一部分，而 *mylibrary* 包是 *mypackage* 包的一部分，所以导入路径需要包含这些包。因为我们假设我们的 *main.py* 与 *mypackage* 在同一个位置，所以导入路径变成了 *mypackage.mylibrary* ，如下面的代码片段所示。

```
from mypackage.mylibrary import module

if __name__ == "__main__":
    number_1 = 10
    number_2 = 20

    result = module.add(a=number_1, b=number_2)
    print(f"{number_1} + {number_2} = {result}")
```

然后我们就可以照常运行 *main.py* 了。

```
$ tree -L 3
.
├── main.py
└── mypackage
    ├── __init__.py
    └── mylibrary
        ├── __init__.py
        └── module.py
$ python main.py 
10 + 20 = 30
```

(完整示例见[python _ package _ library _ example](https://github.com/shunsvineyard/shunsvineyard/tree/main/python_vs_cpp_series/project_build_and_run/python_package_library_example)

# 将包作为可执行文件运行

要使 Python 包可运行，我们需要添加一个 *__main__。py* 文件放到包的顶层，然后我们的 *mypackage* 就变成了下面这个。

```
mypackage
├── __init__.py
├── __main__.py
└── mylibrary
    ├── __init__.py
    └── module.py
```

[__ 主 _ _。py](https://docs.python.org/3/library/__main__.html#main-py-in-python-packages) 用于为一个包提供命令行界面。当使用 *-m* 选项从命令行直接调用包时，Python 解释器将寻找 *__main__。py* 运行。因此，我们可以将我们的入口点代码放入 *__main__。py* 文件。

```
from mypackage.mylibrary import module

if __name__ == "__main__":
    number_1 = 10
    number_2 = 20

    result = module.add(a=number_1, b=number_2)
    print("Running from mypackage...")
    print(f"{number_1} + {number_2} = {result}")
```

如上所述，我们使用带有包名的-m 选项来直接执行包。

```
$ python -m mypackage
Running from mypackage...
10 + 20 = 30
```

# 构建并安装软件包

在 C++中，我们使用 *CMakeLists.txt* 、 *Makefile* ，或者类似的东西来定义构建和安装过程。Python 也有类似的方法，很多工具都是为此服务的，比如 [setuptools](https://setuptools.readthedocs.io/en/latest/) 和 [wheel](https://wheel.readthedocs.io/) (详见[分发 Python 模块](https://docs.python.org/3/distributing/index.html#distributing-index))。用于定义过程的文件称为 [setup.py](https://docs.python.org/3/distutils/setupscript.html) 。 *setup.py* 文件和构建工具的关系类似于 *CMakeLists.txt* 到 CMake。虽然 *setup.py* 文件主要用于定义我们希望项目构建、分发和安装的内容，但是安装文件只是一个常规的 Python 文件，因此我们可以在安装文件中使用任何 Python 功能。下面的示例演示了它如何处理包。假设我们有一个如下所示布局的项目和一个 setup.py 文件，它应该在包之外(在这个例子中是 *mypackage* )。

```
project
├── mypackage
│   ├── __init__.py
│   └── mylibrary
│       ├── __init__.py
│       └── module.py
└── setup.py
```

这个包的基本 *setup.py* 定义如下(下面的例子使用 [setuptools](https://setuptools.readthedocs.io/en/latest/) 作为构建工具)。

```
"""Setup file for mypackage."""

import pathlib
import setuptools

# The directory containing this file
HERE = pathlib.Path(__file__).parent

# This call to setup() does all the work
setuptools.setup(
    name="my-package",
    version="0.0.1",
    description="A Python package example",
    packages=setuptools.find_packages(),
    python_requires=">=3.7",
)
```

*setuptools.setup()* 函数是我们为包构建输入定义的函数，调用该函数实际上是我们在安装文件中需要做的全部工作。

(编写 *setup.py* 的详细内容可以在[编写设置脚本](https://docs.python.org/3/distutils/setupscript.html#writing-the-setup-script)中找到， *setuptools.setup()* 支持的关键字的完整列表可以在 [setuptools —关键字](https://setuptools.pypa.io/en/latest/references/keywords.html)中找到)

一旦我们的项目有了 *setup.py* 文件，我们的包就可以分发和安装了。既然是 Python，就没必要编译代码。下面的例子展示了如何使用 [pip](https://pip.pypa.io/en/stable/) 将包安装到我们的 Python 环境中。

```
$ tree -L 3
.
├── mypackage
│   ├── __init__.py
│   └── mylibrary
│       ├── __init__.py
│       └── module.py
└── setup.py
$ python -m pip install.
Processing /<path to the myproject>/project
  Preparing metadata (setup.py) ... done
Using legacy 'setup.py install' for my-package, since package 'wheel' is not installed.
Installing collected packages: my-package
  Attempting uninstall: my-package
    Found existing installation: my-package 0.0.1
    Uninstalling my-package-0.0.1:
      Successfully uninstalled my-package-0.0.1
  Running setup.py install for my-package ... done
Successfully installed my-package-0.0.1
```

该命令将查找本地 setup.py 并将包(在本例中为 *mypackage* )安装到我们的 Python 环境的 *site-packages* 文件夹中。包名将是我们在 *setuptools.setup()* 函数中指定的名称(例如*)。、<python env>/lib/python 3.7/site-packages/my-package*)。

# 使用可执行条目构建并安装软件包

前面几节展示了 Python 包可以像库或可执行文件一样运行。对于后者，如果包装中包含一个 *__main__。py* 文件，我们可以在安装后用 *-m* 选项执行它。但是还有更多。安装工具提供了一个方便的选项，允许我们定义将在安装过程中生成的可执行条目(例如，命令行界面或 GUI)。以下示例将描述如何将 CLI 条目添加到我们的包中。

该项目类似于上一节中的示例，但这次我们添加了一个带有 *cli.py* 模块的 *bin* 文件夹作为我们的命令行接口。

```
project
├── mypackage
│   ├── __init__.py
│   ├── bin
│   │   ├── __init__.py
│   │   └── cli.py
│   └── mylibrary
│       ├── __init__.py
│       └── module.py
└── setup.py
```

并且假设 *cli.py* 的内容如下。

```
from mypackage.mylibrary import module

def main():
    number_1 = 10
    number_2 = 20
    result = module.add(a=number_1, b=number_2)
    print("Running from entry...")
    print(f"{number_1} + {number_2} = {result}")
```

我们使用 setup.py 文件中的 entry_points 选项来定义条目。

```
"""Setup file for mypackage."""

import pathlib
import setuptools

# The directory containing this file
HERE = pathlib.Path(__file__).parent

# This call to setup() does all the work
setuptools.setup(
    name="my-package",
    version="0.0.1",
    description="A Python package example",
    packages=setuptools.find_packages(),
    entry_points={"console_scripts": ["my-cli=mypackage.bin.cli:main"]},
    python_requires=">=3.7",
)
```

在上面的代码片段中，关键字 *console_scripts* 表示该条目是控制台类型，而 *console_scripts* 需要一个控制台脚本列表。因为它是一个列表，所以它可以是多个控制台条目。对于每个控制台条目，格式为 *<控制台 _ 名称> = <路径 _ 模块:条目 _ 功能>* 。在这个例子中， *main()* 函数是入口点，它的控制台脚本名是 *my-cli* 。因此，当这个包被安装时，一个 *my-cli* 脚本将被创建并安装到我们的 Python 环境中。

```
$ tree -L 3
.
├── mypackage
│   ├── __init__.py
│   ├── bin
│   │   ├── __init__.py
│   │   └── cli.py
│   └── mylibrary
│       ├── __init__.py
│       └── module.py
└── setup.py
$ python -m pip install .
$ my-cli
Running from entry...
10 + 20 = 30
```

我们可以使用 *which my-cli* 命令来验证 *my-cli* 脚本已经安装到我们的 Python 环境中。

```
$ which my-cli
/<python environment>/bin/my-cli
```

生成的 *my-cli* 文件可能是这样的。

```
#!/<python environment>/bin/python
# EASY-INSTALL-ENTRY-SCRIPT: 'my-package==0.0.1','console_scripts','my-cli'
__requires__ = 'my-package==0.0.1'
import re
import sys
from pkg_resources import load_entry_point

if __name__ == '__main__':
    sys.argv[0] = re.sub(r'(-script\.pyw?|\.exe)?$', '', sys.argv[0])
    sys.exit(
        load_entry_point('my-package==0.0.1', 'console_scripts', 'my-cli')()
    )
```

这种能力增加了 Python 包的灵活性。当我们想为我们的包提供一些 CLI 工具或测试特性时，这变得非常容易。

在 C++中，我们需要决定是要构建可执行文件还是库，但是 Python 中可执行文件和库的界限是模糊的。Python 模块和包可以作为库导入，并作为可执行文件执行。这种灵活性允许 Python 包既包含要导入的模块，又包含要在一个包中执行的模块。

(本文中的所有示例代码都可以在 [project_build_and_run](https://github.com/shunsvineyard/shunsvineyard/tree/main/python_vs_cpp_series/project_build_and_run/python_package_library_example) 获得)

*原载于 2022 年 10 月 9 日*[*https://shunsvineyard . info*](https://shunsvineyard.info/2022/10/08/python-vs-c-series-build-and-run/)*。*