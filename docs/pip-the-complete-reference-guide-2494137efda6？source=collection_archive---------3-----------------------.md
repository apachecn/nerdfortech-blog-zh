# Pip —完整的参考指南

> 原文：<https://medium.com/nerd-for-tech/pip-the-complete-reference-guide-2494137efda6?source=collection_archive---------3----------------------->

## 所有 pip 命令的一站式商店

![](img/0ba5d747a2936f5139f885efea308aa3.png)

[Jaredd Craig](https://unsplash.com/@jaredd_craig?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

**注:非会员也可在**[**https://dineshkumarkb . com/tech/pip-the-complete-reference-guide/**](https://dineshkumarkb.com/tech/pip-the-complete-reference-guide/)获取本文

## 简介:

`pip`是 python 的包管理器。 [pip](https://pypi.org/project/pip/) 可以用来安装来自 [PyPI](https://pypi.org/) 的 python 库。换句话说，`pip`就是`python`的`npm`。我们使用 pip 来安装和管理不属于 python 标准库的 python 库。

## 动机:

任何 python 开发人员都会日复一日地使用`pip`。但是，使用仅限于安装和偶尔升级。

本文是每个 python 开发人员都应该知道的所有 pip 命令的一个合并的非详尽列表。这可以作为任何未来 pip 命令的参考。

## 命令:

## 安装—普通安装:

install 命令用于从 pypi 包安装指定的库。这里我没有给出 pip 命令的输出，因为它们是众所周知的。

```
*$pip install [package name]**$pip install boto3*
```

**每用户安装量:**

这在以下场景中可能会派上用场

*   您没有使用虚拟环境
*   您使用的是共享电脑
*   您没有在系统级别安装软件包的管理员权限

```
*$pip install --user [package name]**$pip install --user boto3*
```

默认情况下，pip 软件包安装在系统目录中。对于 linux，它是(/usr/local/lib/python 3.8/dist-packages)。当安装有`--user` 标志时，为特定用户安装。

下面是获取 sitepackages 和 usersitepackages 的代码片段。

```
import site
print(site.getsitepackages()) 
print(site.getusersitepackages()) # usersite packagesOutput:
['/usr/local/lib/python3.8/dist-packages', '/usr/lib/python3/dist-packages', '/usr/lib/python3.8/dist-packages']
'/home/dineshkumarkb/.local/lib/python3.8/site-packages'
```

**安装—带代理:**

如果您的系统位于代理之后，并且您无法直接从 PyPI 下载，您可以使用您的组织提供的代理。

```
*$pip install --proxy=<proxy-url> [packagename]*
```

**安装—升级:**

该命令升级现有的 pip 库。

```
*$pip install --upgrade [package name]
$pip install --upgrade pyinstaller*
```

**安装—从不同的主机:**

从不同于 PyPI 包的主机上安装。

```
*$pip install <packagename> --index <internal host url>*
```

**安装—来自 requirements.txt:**

从`requirements.txt`文件安装。

```
*$pip install -r requirements.txt*
```

**安装—可编辑模式:**

在可编辑模式下安装本地软件包。如果您有一个本地包并且想要使用它，这将非常有用。本地包仍在开发中，对包的任何更改都会反映出来。

```
*$pip install -e <path to your package/repository>*
```

**安装—安装到目标位置:**

将软件包安装到特定位置

```
*$pip install -t <package name> <path to install>
$pip install -t matplotlib .*
```

将 matplotlib 安装到当前目录。如果您正在将项目上传到`AWS Lambda`，这可能会很有用。

**安装—从本地目录:**

从本地目录安装，不要扫描 PyPI。

```
*$pip install --no-index --find-links=. certifi*Output:
Looking in links: .
Requirement already satisfied: chardet in ./pipenvexec/lib/python3.8/site-packages (3.0.4)
```

这里，查找链接指向当前目录，因为它有`certifi`库的 wheel 文件。`--no-index` 标志指定不要查看 PyPI 包。

**安装—来自 VCS:**

这些库可以直接从 git 这样的版本控制系统安装。这对于将项目作为依赖项的大型组织来说非常有用。如果对存储库的访问受到限制，可以直接从 git 安装。

```
*$pip install SomeProject@git+https://git.repo/some_pkg.git@1.3.1*
```

## 从计算机上卸载

卸载库..

```
*$pip uninstall [package name]
$pip uninstall pyinstaller*
```

## 版本:

检查 pip 版本

```
*$pip --version*Output:
pip 21.0.1 from /home/<username>/.local/lib/python3.8/site-packages/pip (python 3.8)
```

## 显示:

获取关于已安装软件包的信息。这包括版本、软件包的安装位置及其依赖项。

```
*$pip show boto3*Output:
Name: boto3
Version: 1.17.29
Summary: The AWS SDK for Python
Home-page: [https://github.com/boto/boto3](https://github.com/boto/boto3)
Author: Amazon Web Services
Author-email: None
License: Apache License 2.0
Location: /home/dineshkumarkb/MyGitHub/pipexec/pipenvexec/lib/python3.8/site-packages
Requires: s3transfer, jmespath, botocore
Required-by:
```

## 冻结:

主要用作`requirements.txt` 文件的输入。

```
*$pip freeze*Output:
altgraph==0.17
appdirs==1.4.3
boto3==1.17.29
...
```

如果你正在使用 virtualenv，并想将所有的依赖项复制到`requirements.txt`，这将非常有帮助。

```
*$pip freeze > requirements.txt*
```

## 列表:

列出所有已安装的软件包。如果在 virtualenv 中执行，它将列出安装在虚拟 env 中的所有包。

```
*$pip list*Output:
Package                Version
---------------------- --------------------
alembic                1.1.0.dev0
apache-airflow         1.10.12
apispec                1.3.3
appdirs                1.4.3
apturl                 0.5.2
argcomplete            1.12.1
arrow                  0.17.0
asn1crypto             0.24.0
attrs                  19.3.0
...
```

## 下载:

下载 python 库，但不要安装它们

```
*$pip download [package name]
$pip download requests
$pip download requests <path to download>*
```

download 命令下载指定库的 wheel 文件及其依赖关系。上面的命令应该给出以下输出。默认情况下，文件下载到当前目录。

```
Output:
Collecting requests
  Using cached requests-2.25.1-py2.py3-none-any.whl (61 kB)
  Saved ./requests-2.25.1-py2.py3-none-any.whl
Collecting chardet<5,>=3.0.2
  Downloading chardet-4.0.0-py2.py3-none-any.whl (178 kB)
     |████████████████████████████████| 178 kB 1.4 MB/s 
  Saved ./chardet-4.0.0-py2.py3-none-any.whl
Collecting certifi>=2017.4.17
  Downloading certifi-2020.12.5-py2.py3-none-any.whl (147 kB)
     |████████████████████████████████| 147 kB 1.9 MB/s 
  Saved ./certifi-2020.12.5-py2.py3-none-any.whl
Collecting urllib3<1.27,>=1.21.1
  Downloading urllib3-1.26.4-py2.py3-none-any.whl (153 kB)
     |████████████████████████████████| 153 kB 1.8 MB/s 
  Saved ./urllib3-1.26.4-py2.py3-none-any.whl
Collecting idna<3,>=2.5
  Using cached idna-2.10-py2.py3-none-any.whl (58 kB)
  Saved ./idna-2.10-py2.py3-none-any.whl
Successfully downloaded requests chardet certifi urllib3 idna
```

## 搜索:

pip search 搜索 python 包。它搜索的默认位置是 PyPI 包([https://pypi.org/pypi](https://pypi.org/pypi))。

这个现在不行了。

```
*$pip search requests*Output:
ERROR: XMLRPC request failed [code: -32500]
RuntimeError: PyPI's XMLRPC API is currently disabled due to unmanageable load and will be deprecated in the near future. See [https://status.python.org/](https://status.python.org/) for more information.
```

然而，如果您有像`artifcatory`这样的组织内部托管的库，这可能是有用的。

默认的 URL 可能会被-i 标志覆盖。

```
*$pip search <internalpackagename> --index <internal-artifactory-url>*
```

## 检查:

检查已安装软件包的兼容性

```
*$pip check requests*Output:
launchpadlib 1.10.13 requires testresources, which is not installed.
pyasn1-modules 0.2.8 has requirement pyasn1<0.5.0,>=0.4.6, but you have pyasn1 0.4.2.
marshmallow-sqlalchemy 0.24.0 has requirement marshmallow>=3.0.0, but you have marshmallow 2.21.0.
```

## 缓存:

pip 通常将所有安装的库存储在缓存中。

```
*$pip cache dir*Output:
/home/<username>/.cache/pip
```

车轮文件缓存在`dir`目录下。每当 pip 安装被触发时，它检查本地缓存，然后连接到 PyPI。

pip 缓存背后的想法很简单，当您第一次使用 pip 安装 Python 包时，它会保存在缓存中。如果您再次尝试下载/安装相同版本的软件包，pip 将只使用本地缓存的副本

请注意，这在虚拟环境中不起作用，因为可能不需要缓存库。

## 禁用缓存:

安装软件包，但不保存到缓存。

```
*$pip install matplotlib --no-cache-dir*
```

## 帮助:

显示 pip 命令的帮助。

```
*$pip help*Output:
Usage:   
  pip <command> [options]Commands:
  install                     Install packages.
  download                    Download packages.
  uninstall                   Uninstall packages.
  freeze                      Output installed packages in requirements format.
  list                        List installed packages.
  show                        Show information about installed packages.
```

## 详细:

在详细模式下运行 pip 以获取更多信息。

```
$pip install --verbose click
```

这产生了一堆包含大量信息的输出。

## 禁用 pip 版本检查:

如果有新版本的 pip 可用，则禁用对 PyPI 的定期检查。

```
$pip install --disable-pip-version-check [package name]
```

我建议不要禁用这个，因为你可能会错过来自 PyPI 的重要更新。

当我碰巧使用它们时，我将继续添加更多的命令。

感谢阅读。

## 参考资料:

*   [https://real python . com/what-is-pip/#:~:text = The % 20 pip % 20 install % 20 command，参见% 2C % 20 multiple % 20 packages % 20 wers % 20 installed](https://realpython.com/what-is-pip/#:~:text=The%20pip%20install%20command,see%2C%20multiple%20packages%20were%20installed)。
*   https://pip.pypa.io/en/stable/reference/