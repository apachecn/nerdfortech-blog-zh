# 如何使用 Python 构建和分发 CLI 工具

> 原文：<https://medium.com/nerd-for-tech/how-to-build-and-distribute-a-cli-tool-with-python-537ae41d9d78?source=collection_archive---------0----------------------->

在我的[上一篇文章](https://anishkrishnaswamy.medium.com/ranpass-a-random-password-generator-cli-tool-46f9b3f9b4e)中，我写了为什么我构建了一个随机密码生成器 CLI 工具。在本文中，我将展示如何使用 Python 从头开始构建 CLI 工具的细节。

# 要求

在你的系统上安装 Python(最好是 Python3，因为 Python2 是 20 世纪 20 年代以前的版本)，一个你自己选择的文本编辑器和一些热情。

在我们开始之前，让我们也设置一些我们将需要的其他东西。打开终端并安装:

1.  virtualenv 基本上是一个隔离的 python env，我们可以在其中测试我们的工具/脚本等，而不用在我们的系统上全局安装它们。
2.  wheel - `pip install wheel` wheel 是 python 工具/包的打包机制。
3.  setuptools - `pip install setuptools` setuptools 是一个库，我们将使用它来打包我们的工具。
4.  twine - `pip install twine` twine 用于将包分发到 pypi/test.pypi。

注意:如果您认为您可能已经安装了这些，那么通过提供`--upgrade`标志将它们更新到最新版本。总是最好保持你的工具更新。

# 目录结构和初始设置

为工具创建一个目录。然后创建以下文件夹结构和其中的文件

```
MyTool
    - app
        - __init__.py
        - __main__.py
        - application.py
    - my_tool.py
    - setup.py
    - requirements.txt
    - README.md
    - LICENSE
    - MANIFEST.in
```

选择分发软件包的许可证。我用[https://choosealicense.com/](https://choosealicense.com/)来挑选许可证。

将下面一行添加到 MANIFEST.in 中，告诉 setuptools 将许可证文件与要分发的包一起添加
`include LICENSE`

为项目写一份好的自述。如果您不确定要添加什么，您可以在完成工具构建后，当您对工具的功能有了更清晰的认识时，再编写它。

我们将使用 [Click](https://click.palletsprojects.com/en/7.x/) 来构建这个 CLI 工具。虽然我们可以通过使用`sys.argv`读取参数来构建一个简单的 CLI 工具，但使用 click 这样的库将使我们能够轻松地添加更多功能并扩展我们的工具。

我们的工具所需的依赖关系在`requirements.txt` 中指定，这里我们使用 Click，所以将下面一行添加到文件
`click>=7.1.2`

我们完成了吗？我们可以开始编码了吗😐*？*

不，在进入应用程序的核心之前，我们还有一件事要做。设置`setup.py`。这个文件告诉`setuptools`如何打包工具。我花了一些研究和一点试验性的错误来弄清楚什么去哪里让工具正常工作。

```
from setuptools import setup, find_packageswith open("README.md", "r", encoding="utf-8") as fh:
    long_description = fh.read()
with open("requirements.txt", "r", encoding="utf-8") as fh:
    requirements = fh.read()setup(
    name = 'mytool',
    version = '0.0.1',
    author = 'John Doe',
    author_email = 'john.doe@foo.com',
    license = '<the license you chose>',
    description = '<short description for the tool>',
    long_description = long_description,
    long_description_content_type = "text/markdown",
    url = '<github url where the tool code will remain>',
    py_modules = ['my_tool', 'app'],
    packages = find_packages(),
    install_requires = [requirements],
    python_requires='>=3.7',
    classifiers=[
        "Programming Language :: Python :: 3.8",
        "Operating System :: OS Independent",
    ],
    entry_points = '''
        [console_scripts]
        cooltool=my_tool:cli
    '''
)
```

让我们来看看这个文件中一些令人困惑的部分。

`py_modules` —这是我们告诉`setuptools`打包工具时必须包含哪些模块的地方。在我们的例子中，我们要求它包含`my_tool.py`和整个`app`文件夹。这就是我们工具的主要功能所在。

`packages`—这告诉`setuptools`应该在当前目录中寻找的位置，以找到工具所需的包。`find_packages()`不带任何参数意味着在整个目录中查找任何所需的包。

`entry_points`——这是我们告诉如何调用我们的工具，以及当它被调用时必须调用什么函数的地方。
`[console_scripts]`告知 setuptools 该工具将被用作 CLI 工具(如 pip 或 npm)。

`cooltool=my_tool:cli`这告诉 setuptools，每当有人在终端中键入 cooltool 时，调用`my_tool.py`中的 cli 函数。如果我们有另一个名为`start`的函数，并且我们希望它被调用，我们应该编写`my_tool:start`

# 代码

好了，现在我们已经完成了所有的设置，我们可以开始我们真正要做的了。

**my_tool.py**

这是我们应用程序的入口。这就是我们需要通过编写`import click`来添加点击集成的地方，并设置我们想要添加到工具中的不同命令。

然后，我们使用@click.group() decorator 创建一个组对象

```
@click.group()
def cli():
    pass
```

这只是告诉 click 我们有一组命令，我们可以向其中添加多个子命令。

@cli.command() decorator 用来告诉 click 下面的函数作为命令调用时应该执行。
我们可以通过@click.option()为命令设置我们想要的输入。

例如，如果您希望命令`cooltool hello`将 Hello World 输出到终端

```
@cli.command()
def hello():
    click.echo("Hello World")
```

`click.echo`用于代替 print，以确保同时支持 Python2 和 Python3 终端，并且具有比普通 print()更广泛的功能。

假设我们想将名称作为命令的输入选项

```
@cli.command()
@click.option('-n', '--name', type=str, help='Name to greet', default='World')
def hello(name):
    click.echo(f'Hello {name}')
```

我们使用 Python3 的 f-Strings 来格式化字符串，这样速度更快，可读性更强，也更简洁。如果没有提供选项-n，将使用默认值 World 作为名称。

这是一个简单的例子。如果我们需要做一些更复杂的事情，我们可以在 app 文件夹中的 application.py 文件中编写代码，然后用`from app import application`将文件导入 my_tool.py，然后调用我们需要的相关函数。

# 测试工具

好了，现在我们已经准备好了一个工作版本，让我们来测试一下。
还记得我们装了个叫`virtualenv`的东西吗？是时候使用它了。

打开终端，导航到文件夹 MyTool。
输入`virtualenv <name for virtual env>`创建一个虚拟环境，例如:`virtualenv venv`

> 这将在您的系统上创建一个与 python env 无关的全新环境。这对于测试非常有用，因为我们不必在系统上安装不需要的依赖项。

用命令`source venv/bin/activate`激活虚拟环境。你应该会在你的终端看到 venv。

现在让我们在这个虚拟环境中安装工具。该命令是`python setup.py develop`。这会将 CLI 工具安装到虚拟环境中`venv`。
我们可以通过键入`which cooltool`进行验证，它应该会输出 cooltool 在 venv 文件夹中的位置。
我们可以通过键入`cooltool hello -n Python`来测试它，我们应该在终端上得到输出 Hello Python。

# 包装和分销

现在我们已经准备好了一个 cli 工具，我们想打包并分发它，以便我们的朋友或任何人都可以使用它。

为此，我们将再次使用 setuptools，但这一次不是要求它打包用于开发，而是要求它打包用于分发。

`python setup.py sdist bdist_wheel`

这个命令将在我们的 MyTool 目录中创建一个文件夹 dist。您可以查看使用 ls 命令构建了哪些二进制文件。应该有一个. whl，。egg 和一个. tar.gz 文件存在。

我们将把它上传到 pypi 的测试环境[https://test.pypi.org](http://test.pypi.org)，因为我们的 cli 工具只是用于演示目的。

导航至[https://test.pypi.org](https://test.pypi.org)并创建一个账户。然后创建一个 API 令牌，以便可以上传二进制文件进行分发。确保将令牌复制到本地的某个地方。

我们将使用 twine 将二进制文件上传到 test.pypi.org。

`twine upload --repository testpypi --skip-existing dist/*`
`--repository`很重要，当我们想要发布我们工具的进一步版本时，跳过现有版本将很有用。

输入用户名`__token__`和复制的完整令牌作为密码。

搞定了。！我们已经使用 Python 成功地打包和分发了一个 CLI 工具。

我们可以在系统中使用`pip install --index-url [https://test.pypi.org/simple/](https://test.pypi.org/simple/) cooltool`来安装它。这种安装将是全球性的，我们现在可以从任何地方运行该命令。

# 后续步骤

这是我们制作的演示工具。我们可以通过使用 Click 向工具添加更多命令来添加更多功能。

我们也可以将它分发到主[https://pypi.org](https://pypi.org)仓库。遵循我们在 test.pypi 中发布的步骤，包括创建一个帐户和令牌。

编码快乐！

[https://bongo.cat/](https://bongo.cat/)

# 参考

有关更多详细信息和文档，请参考以下链接

*   [点击](https://click.palletsprojects.com/en/7.x/quickstart/#echoing)
*   [Python 打包](https://packaging.python.org/tutorials/packaging-projects/)
*   [虚拟人](https://virtualenv.pypa.io/en/stable/)
*   [设置工具](https://setuptools.readthedocs.io/en/latest/userguide/quickstart.html)
*   [麻线](https://twine.readthedocs.io/en/latest/)
*   [车轮](https://wheel.readthedocs.io/en/stable/quickstart.html)

你也可以在这里查阅我为 CLI 工具[编写的代码。](https://github.com/kanish671/ranpass)