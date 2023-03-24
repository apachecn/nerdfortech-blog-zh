# 熊猫中的数据过滤

> 原文：<https://medium.com/nerd-for-tech/data-filtering-in-pandas-7ecd90d4a81a?source=collection_archive---------2----------------------->

Pandas 提供了广泛的数据选择方法，以下是一些您可以选择的方法。

*   将条件表达式传递给`[]`操作。
*   使用`query()`功能。
*   使用`eval()`功能。

为了了解哪种数据过滤方式更优化，我们将创建一个包含大量数据集的样本数据框架，并对其进行检验。

![](img/f46b6b8f50c003946a57e9a85ce72113.png)

由[戴尔芬·杜卡瑞格](https://unsplash.com/@delphinenz?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

```
#creating a sample large data set
import numpy as np
import pandas as pd
import timeit
np.random.seed(42)
d = np.random.randint(1, 10, size=(500000, 2))
df = pd.DataFrame(d)
df.columns = ["col1 ", "col2"]
```

我们将使用该数据集进行整个分析 [repo_link](https://replit.com/join/spkzgwslra-dinirockz93) ，当数据量较小时，您选择哪一个都没关系，但`[]`操作可能会更快(取决于您选择的系统)。如果数据量大或者过滤条件复杂，就要慎重选择。

# **通过**选择数据`**[]**`

索引是过滤数据的更简单的方法，我们可以使用 pandas 索引来划分数据帧的子集。实际上，条件表达式创建了一个布尔序列，我们可以用它来过滤数据帧。

```
# [] operator
start = timeit.default_timer()
dfTemp = df[(df["col1 "] % 2==0) & (df["col2"] > 5)]
stop = timeit.default_timer()
print("Run time of df[df[(df[\"col1\"] % 2==0) & (df[\"col2\"] > 5)] is {} seconds.".format(stop-start))
#output
Run time of df[df[(df["col1"] % 2==0) & (df["col2"] > 5)] is 0.020918252997944364 seconds.
```

# 通过查询选择数据()

**query()** 是一种更干净、更容易的过滤行的方法。只需将一个带引号的条件表达式传递给`query()`。而且，`query()`支持复杂得多的条件表达式。

> ***注意*** *:这个操作不会改变数据帧的位置。想改就过* `*inplace=True*` *。*

```
#select data by query() operator
start = timeit.default_timer()
dfTemp = df.query("col1 % 2 == 0 & col2 > 5")
stop = timeit.default_timer()
print("Running time of df.query(\"col1 % 2 == 0 & col2 > 5\") is {} seconds.".format(stop-start))#output
Running time of df.query("col1 % 2 == 0 & col2 > 5") is 0.01401120700073079 seconds. 
```

与[]相比，Query 似乎更快，我们能做得更好吗！？

# 通过 eval()选择数据

Pandas `**dataframe.eval()**`函数用于在调用 dataframe 实例的上下文中计算表达式。表达式在数据帧的列上计算，并根据需要过滤数据。

```
#select data by eval() operator
start = timeit.default_timer()
dfTemp = df[df.eval('col1 % 2 == 0 & col2 > 5')]
stop = timeit.default_timer()
print("Running time of df[df.eval('col1 % 2 == 0 & col2 > 5')] is {} seconds.".format(stop-start))#output
Running time of df[df.eval('col1 % 2 == 0 & col2 > 5')] is 0.001771175499743549 seconds. 
```

> 当数据很小时，正统的`*[]*`运算往往会更快。`*query()*`和`*eval()*`的优势在于海量的数据集。此外，`*eval()*`的语法更加简洁。

下面是一些受`eval`支持的条件语句和操作符:

*   除左移(`<<`)和右移(`>>)`运算符)以外的算术运算。
*   比较操作。
*   布尔运算，如`df.col1> 5`和`df.col2 <=10`
*   属性访问，比如`df.col1`。
*   下标表达式，比如`df[0]`。
*   数学函数。

```
import math
df[df.eval('math.sin(df.col1)>0')]
```

*   列表和元组文本。

```
df[df.eval('df.col1 in ["Sun", "Moon"]')]
```

![](img/1d0591a6a5752e141433883ea6c99c3e.png)

照片由[卢卡·布拉沃](https://unsplash.com/@lucabravo?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

优化我们过滤数据的方式，将是在使用 dataframe 时研究系统以提高性能的选项之一，优化过滤操作`eval(), query(), []`并不能保证性能提高，这是一个多变量方程，但这将是一个令人痛苦的变量，值得一试，干杯！