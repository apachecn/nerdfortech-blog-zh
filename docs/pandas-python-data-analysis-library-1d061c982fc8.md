# 熊猫:Python 数据分析库

> 原文：<https://medium.com/nerd-for-tech/pandas-python-data-analysis-library-1d061c982fc8?source=collection_archive---------22----------------------->

![](img/b057369d3e0ed321bc174e4aab43d7bf.png)

> **Pandas** 是一款快速、强大、灵活且易于使用的开源数据分析和操作工具，构建于 [Python](https://www.python.org/) 编程语言之上。

**安装熊猫**

```
pip install pandas
```

**进口熊猫**

```
import pandas as pd
```

**创建数据帧**

![](img/cbf96b52548c853669ee2d97a8bd3336.png)

**读取. csv 文件**

这里，我们将使用一个' *hotel_booking* '数据集来了解熊猫的各种功能。

```
data = pd.read_csv('hotel_bookings.csv')
data.head()
```

![](img/96137bf018d921de7e664e94d2e95506.png)

在数据集中给出的 32 列中，上面只显示了几列。

**读取前 5 行**

```
data.head()
```

**读取最后 5 行**

```
data.tail()
```

**数据帧的形状**

```
data.shape
```

**显示关于数据集的信息**

```
data.info()
```

**获取列名**

```
data.columns
```

**检查 NaN 值的数量**

这给出了数据集中存在的空值的数量。

```
data.isnull().sum()
```

**显示统计信息**

它给出分布的平均值、最大值、最小值、标准差、计数、25%等。

```
data.describe()
```

![](img/bd917b3a301fa23daa111a9c1e17acbb.png)

**分度**

![](img/4e39ae2606415e1c30de97a367cb18aa.png)

*。iloc[start_row: end_row，start_col : end_col]* 将显示范围内的行

[开始行，结束行]和范围[开始列，结束列]中的列

**删除列**

![](img/0542bba17f9e2bba7fcb8634a26ef8b4.png)

**处理缺失值**

*用平均值填充缺失的数值*

![](img/65ea824daf72fae1b6559b8d0487b239.png)

*缺失的分类值用模式值*填充

![](img/65cca403d76c6e4036adfc38e358d313.png)

**使用' *astype()'*** 更改列的数据类型

![](img/678d9c5eabf55f17dc2e21fd2ab748bd.png)

另一种方式:

![](img/eab7267d7bb166dd1fd3c92bd2eaf93b.png)

要了解更多，请点击这里查看熊猫的官方文件。

快乐学习！