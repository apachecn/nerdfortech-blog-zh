# 如何用 pandas_datareader 获取印度股票数据？

> 原文：<https://medium.com/nerd-for-tech/how-to-get-indian-stock-data-using-pandas-datareader-d323866a65cb?source=collection_archive---------5----------------------->

在这篇博文中，我将指导你如何获取#NSE(国家证券交易所数据)的股票数据。

数据收集通常是初级数据科学家最容易忽视的部分。

我们开始吧

1.  获取股票行情列表。[链接](https://gist.github.com/tushifire/f5e14039618fb83ecfa277cd2e2d59ec)*为最具流动性和 F & O 的股票。*

2.安装 pandas_datareader:使用

```
pip install pandas_datareader
```

3.一些必需的进口:

```
**import** pandas **as** pd
**import** pandas_datareader **as** pdr
**import** time
```

4.提取股票代码名称。

股票选择

5.定义一个名为 fetch_data 的函数，该函数将输入作为符号名，并将股票价格保存在指定的目录中，这里是“stock_data/”。

提取股票数据功能

6.对所选股票运行 fetch_data 函数。

提取所选股票的股票数据

这里我使用了 time.sleep(10 ),因为 Yahoo API 在短时间内停止了许多请求。

**注意:**这在 google colab 上不起作用，所以在你的电脑上试试。