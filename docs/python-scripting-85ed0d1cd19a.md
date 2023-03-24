# 学习 Python 脚本的技巧[示例]

> 原文：<https://medium.com/nerd-for-tech/python-scripting-85ed0d1cd19a?source=collection_archive---------14----------------------->

坦纳·琼斯@全技术男

![](img/40e487871d09f16183a2e7fdd9db5426.png)

[图像来源](https://codedancers.com/python-tutorial-episode-32-pass-statement-in-python/)

Python 是一种面向对象的编程语言，也是当今最流行的编程语言之一。您应该学习 Python 的原因有很多。我之前写过一篇文章，帮助自学的程序员。查看那篇文章 [**这里**](/nerd-for-tech/tips-for-learning-python-ea9eb25fc03d) 。

Python 是一种强大的编程语言，Python 最好的特性之一就是可读性。该语法旨在生成易于理解的清晰可读的代码。这种语言的设计考虑了几个指导原则。这些“指导原则”被称为[*Python 之禅*](https://www.python.org/dev/peps/pep-0020/) *:*

*   漂亮总比难看好。
*   显性比隐性好。
*   简单比复杂好。
*   复杂总比复杂好。
*   查看[此处](https://www.python.org/dev/peps/pep-0020/)查看完整列表。

虽然所有的列表都是 Python 如此强大的原因，但是 Python 成为我最喜欢的语言的原因已经在上面列出的前 4 条原则中解释过了。无论您是想成为一名全栈开发人员，还是将编程作为业余爱好，有效地使用 Python 都会非常有用(而且非常有趣)！

Python 的*禅确保生成的代码高效优雅，但总体上易于阅读。使用 Python 编码对象有几种方法，但是要记住可读性是关键。这就是 Python 非常适合编写脚本的原因。脚本创建简短的程序，执行这些程序是为了自动化日常任务。你有没有遇到过每天、每周或每月都要做的任务会占用你很多时间？我敢肯定你的答案是**是的**，而且你宁愿花时间和精力去做别的事情！这就是 Python 脚本派上用场并扭转乾坤的地方！记住这些指导原则，让我们深入研究一个脚本示例。*

在本例中，我将创建一个脚本来自动创建文件夹目录。创建此文件夹目录是为了跟踪在笔测试期间收集的信息。我把剧本分成了 4 个主要部分。

通过快速浏览下面的代码，您会发现它有几个主文件夹，分别叫做“文档”、“发现”和“数据”。在这些文件夹中，创建了几个文件夹，如“客户信息”和“报告”。可以非常容易地改变文件夹结构，以适应所使用的新工具或添加/删除不必要的信息。这可以节省大量的时间！想象一下，每次进行 pen 测试时都必须手动创建这些文件夹，或者必须为企业的每个客户创建类似的文件夹结构。这需要时间，并且可能会发生错误，占用更多的时间、精力和资源。

```
**#PART 1: Import required modules**

import os
def main():**#PART 2:Creating folder structure**

"""Documentation"""
Doc = "1.Documentation"
Cust = "a. Customer Info"
Reports = "b. Reports"

#Findings Folder
"""Findings"""
Findings = "2.Findings"
Ext_F = "a. External"
Int_F = "b. Internal"
Phishing = "c. Phishing"

#Data Folder
"""Data"""
Data ="3.Data"
Database = "a. Database"
Phishing = "b. Phishing"
Templates = "i. Templates"
Payloads = "ii. Payloads"
Network = "c. Network Mapping"
ExtM = "i. External"
Nmap_E ="1\. Nmap"
Eye_E = "2\. Eyewitness"
Int_M = "Internal"
Nmap_I ="1\. Nmap"
Eye_I = "2\. Eyewitness"
Pen_Test = "d. Penetration Test"
Vul_Scan = "e. Vulnerability Scanning"
Ext_V = "i. External"
Nessus_E = "1\. Nessus"
Internal_I = "1\. Nessus"
Web_App = "f. Web App"
Burp = "i. Burp"
Nikto = "ii. Nikto"**#PART 3: Directory** 
#Where the directory will be exectuted
root = "<insert directory>"**#Part 4: Folder Creation**
path = f"{root}/{Doc}/{Cust}/{Reports}"

"""
path = f"{root}/{Findings}/{Int_F}/{Ext_F}/{Phishing}"
path = f"{root}/{Data}/{Database}/{Phishing}/{Database}/{Phishing}"
"""
print(path)

if not os.path.exists(path):
    os.makedirs(path)
else:
    os.removedirs(path)
```

在代码段的底部，您可以将根目录更改为将创建文件夹的位置。“if”语句将检查并查看路径是否已经创建，否则将创建路径。查看我的 [github 库](https://github.com/alltechguyblog/Python_Automation/blob/main/folder_creation.py)以获得代码的副本。这只是一个例子，说明如何使用 Python 通过创建简短易读的脚本来自动化日常任务。现在出去自己创作一个脚本吧！

请务必在下面评论您的脚本想法。

感谢阅读！