# 使用 Prophet 进行股票预测

> 原文：<https://medium.com/nerd-for-tech/stock-prediction-using-prophet-31c6e09de0d3?source=collection_archive---------17----------------------->

# 设置阶段

在这篇文章中，我将讨论预测股票价格的股票使用脸书先知，这是一个开源库。Prophet 使用时间序列数据框架进行未来预测。我将使用 google colab 来演示这一点。为了搭建平台，我们需要安装 yahoofinace(yfinance ),它将是提取历史股票时间序列数据的来源，Prophet(prophet)库将用于预测，pandas 将修改数据框架以适应 Prophet 的要求。金融、先知和熊猫这三个库是因为使用导入函数而被调用的。

> **！pip 安装 yfinance**
> 
> **T5！pip 安装先知 **
> 
> ***导入熊猫为 pd***
> 
> ***导入 yfinance 为 yf***
> 
> ***从先知导入先知***

# 正在准备数据集

现在使用雅虎财经(yfinance)下载股票的历史数据。股票的股票代码可以从雅虎财经网站获得。在目前的情况下，我已经创建了 RIL 数据框架，其中包含从 yf 下载的印度信实有限公司的历史市场数据。当前情况下的股票代码是 RELIANCE.NS。需要使用 head/tail 函数检查数据框。在本例中，我们可以看到 RIL 数据框架有“日期”、“开盘价”、“最高价”、“最低价”、“收盘价”、“调整收盘价”和“成交量”栏。

> ***RIL = YF . download(‘信诚。*NS’)**
> 
> ***RIL=RIL.sort_values('日期')。*reset _ index()**
> 
> ***RIL.tail(3)***

# 数据集分析和工程

在一个理想的世界里，所有的方面都应该被考虑到股票的市场价格中。鉴于这一点和先知图书馆的要求，我们将从 RIL 数据框架中删除除“日期”和“调整关闭”之外的所有列。Prophet 接受熊猫期望的 ds(日期戳)格式，即 YYYY-MM-DD 表示日期，YYYY-MM-DD 表示时间戳。为了将数据帧 RIL 与 Prophet 一起使用，我们需要将列名更改为“ds”和“y”

> ***RIL = RIL[['日期'，'形容词关闭']]***
> 
> ***【RIL = RIL . rename(columns = { ' Date ':' ds '，' Adj Close': 'y'})***
> 
> ***RIL . head②***

# 将 Prophet 模型应用于数据集

我们现在都准备好了。调用先知函数并命名。我把它命名为模型(可能是任何东西)。现在把这个模型放到 RIL 的数据框架中。还有其他一些我们在本文中不会考虑的参数，因此，一旦执行了单元代码，请忽略#INFO。

> ***型号=先知()***
> 
> ***【RIL】***

# 创建测试/预测数据集

为了进行预测，我们需要一个带有 ds 列的数据帧，其中包含要进行预测的日期。这可以使用帮助器方法 Prophet.make_future_dataframe 适当地创建，指定需要进行预测的天数。默认情况下，该数据框架还将包括历史记录中的日期，以便我们可以检查模型是否合适。

> ***future = model . make _ future _ data frame(periods = 30)***

# *获得预测结果*

*当我们执行 model.predict(future)时，根据历史数据(上面的 RIL 数据帧)训练的模型现在用于将预测值(yhat)传递给未来数据集(类似于测试数据集)的每一行。结果保存在新的数据集预测中，除了时间序列标记(ds)、预测值(ys)值之外，还包括误差的上限(yhat_upper)和下限(yhat_lower)。*

> ****fore*cast =***
> 
> *****预测[['ds '，' yhat '，' yhat_lower '，' yhat_upper']]。*尾巴()****

# **交互式可视化**

**为了便于可视化，可以调用 plot_plotly、plot_components_plotly，并使用“模型”和“预测”执行函数 plot_plotly。**

> ***从 prophet.plot 导入 plot_plotly，plot_components_plotly***
> 
> ***plot_plotly(模型，预测)***

# **参考**

**[](https://facebook.github.io/prophet/docs/quick_start.html) [## 快速启动

### Prophet 遵循 sklearn 模型 API。我们创建一个 Prophet 类的实例，然后调用它的 fit 和 predict…

facebook.github.io](https://facebook.github.io/prophet/docs/quick_start.html)**