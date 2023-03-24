# 使用 Python 创建账单拆分和小费计算器

> 原文：<https://medium.com/nerd-for-tech/creating-a-bill-split-and-tip-calculator-using-python-7f1f8a0ca265?source=collection_archive---------1----------------------->

![](img/6534ae23eb594b93e1a829b3f755d3ab.png)

这个项目将需要 Python。如果您目前没有安装 python，请点击下面的链接了解更多关于如何安装的信息。你需要自制软件来安装苹果操作系统和巧克力来安装视窗操作系统。
[https://wiki.python.org/moin/BeginnersGuide/Download](https://wiki.python.org/moin/BeginnersGuide/Download)

安装后，验证您已安装的版本

```
$ python3 —- version
Python 3.7.6
```

现在，我们可以开始使用代码编辑器(如 VSCode 或 Atom)创建账单拆分和小费计算器。我将一步一步地指导您，但是我的全部代码都列在文章的末尾。

创建新的。py 文件，并使用以下命令启动它:

```
#!/usr/bin/env python3.7.6
# Python implementation here
```

让我们开始创建计算器。我们将为用户打印一些文本，让他们知道他们正在使用账单拆分和小费计算器。

```
print(“Welcome To The Bill Split Calculator”)
```

为了计算小费和每个人欠账单的金额，我们需要提示用户账单的金额、他们想要给的小费百分比以及有多少人分摊账单。

```
total_bill = float(input(“What is the total bill?: “))percentage_tip = int(input(“What % tip would you like to give?: “))number_of_people = int(input(“How many people are splitting the bill?: “))
```

现在我们有了关于账单的所有信息以及有多少人分摊账单，我们可以计算每个人需要支付多少。

```
payment_per_person = (round(float(((percentage_tip / 100 +1) * total_bill) / number_of_people), 2))
```

最后，打印每个人需要支付的金额。

```
print(f”Each person owes: ${payment_per_person}”)
```

在我们的终端上运行文件。输出应该如下所示:

```
$ python3 tip_calculator.py
Welcome To The Bill Split Calculator
What is the total bill?: 200
What % tip would you like to give?: 18
How many people are splitting the bill?: 5
Each person owes: $47.2
```

我在输出中注意到的一件事是，对于以 0 结尾的十进制值，结尾的零没有被打印出来。在本例中，拆分金额为$47.20，但打印为“$47.2”。小数. x1 — .x9 打印正确，因为我在 round 函数中使用了 2 位小数。我不喜欢省略尾随零，所以我将调整代码，以确保它总是打印 2 个小数位。为此，请在 round 函数前面添加“%.2f”。

```
payment_per_person = (**“%.2f” %** round(float(((percentage_tip / 100 +1) * total_bill) / number_of_people), 2))
```

我们应该添加到输出中的是为用户打印小费金额。如果用户需要在账单上写小费，这可能会有帮助。我们还会加上包括小费在内的总数，这样用户就不用计算了。我们必须将 total_bill 和 tip_amount 转换为浮点值。

```
tip_amount = "%.2f" % float(percentage_tip / 100 * total_bill)
print(f"Tip amount: ${tip_amount}")#convert total bill and tip amount to floats so they can be added
total_bill_float = float(total_bill)
tip_amount_float = float(tip_amount)
tip_and_total = (total_bill_float+tip_amount_float)
print(f"Total bill including tip: ${tip_and_total}")
```

经过我们的编辑，最终的输出应该类似于下图。我添加了一个换行符来分隔输入和输出，以便于阅读。

```
python3 tip_calculator.py
Welcome To The Bill Split Calculator
What is the total bill?: 219.47
What % tip would you like to give?: 20
How many people are splitting the bill?: 5Tip amount: $43.89
Total bill including tip: $263.36
Each person owes: $52.67
```

我是学习 Python 的新手，所以在创建计算器时遇到了一些问题。一个是。需要修复的 py 文件，以便显示按人支付浮动值，而不是“{payment_per_person}”。请确保在“打印(f“每个人的欠款:${payment_per_person}”)中包含“f”此外，要注意每个函数所需的括号数，因为这也将返回语法错误。我花了太多时间在代码中寻找错误，在谷歌上搜索我可能做错的地方，结果发现是少了一个括号。

感谢您跟随本项目教程。我希望你在创建计算器的时候学到了一些东西。我的最终代码如下。

```
#!/usr/bin/env python3.7.6
# Python implementation hereprint("Welcome To The Bill Split Calculator")#Prompt the user to enter the amount on the bill
total_bill = float(input("What is the total bill?: "))#Prompt the user for the % tip they'd like to leave
percentage_tip = int(input("What % tip would you like to give?: "))#Prompt the user for the # of people splitting the bill
number_of_people = int(input("How many people are splitting the bill?: "))print("\n")#Calculate the value each person owes based on the bill and tips the user entered
payment_per_person = ("%.2f" % round(float(((percentage_tip / 100 +1) * total_bill) / number_of_people), 2))#Print the $ amount of the tip
tip_amount = "%.2f" % float(percentage_tip / 100 * total_bill)
print(f"Tip amount: ${tip_amount}")#convert total bill and tip amount to floats so they can be added
total_bill_float = float(total_bill)
tip_amount_float = float(tip_amount)
tip_and_total = (total_bill_float+tip_amount_float)
print(f"Total bill including tip: ${tip_and_total}")#Print what each person needs to pay
print(f"Each person owes: ${payment_per_person}")
```