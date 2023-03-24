# Python 中的股票交易新闻提醒:当你的股票价格发生变化时收到短信

> 原文：<https://medium.com/nerd-for-tech/stock-trading-news-alert-in-python-get-sms-when-your-stock-prices-change-edd9ffbe382c?source=collection_archive---------0----------------------->

我们都想在人生的某个阶段投资股票。当你梦想中的公司股价下跌时买入，或者当股价下跌时卖出，这是轻松赚钱的终极方法。

![](img/bc10c2c624a17f81a044a8c9db27877a.png)

图片来源:快门股票

但是不可能一直跟踪股票价格。对吗？

因为我在高通有股份。我想到创建一个 python 脚本，每当高通股票在合理的范围内波动时，它可以通过短信提醒我。

> 如果我们也能通过最新的新闻文章知道股票价格波动的原因，岂不是锦上添花？这个项目就是这么做的。**在发送新闻主题中有公司名称的三篇文章的同时，提醒用户股票价格的变化。**

## 在您的设备上设置 Python(如果已经设置好，请跳过)

*   Python 设置教程[点击这里](https://realpython.com/installing-python/)

# **创建脚本的步骤**

1.  导入请求和 twilio.rest 库
2.  定义全局常数
3.  获取昨天的收盘价
4.  获取前天的收盘股价
5.  找出昨天收盘股价和前天收盘股价的正差值
6.  计算出昨天收盘价和前天收盘价之间的价格差百分比
7.  使用新闻 API 获取关于公司的新闻
8.  使用 python slice 运算符列出前三篇文章
9.  使用列表理解创建前三篇文章标题和描述的新列表
10.  通过 Twilio 将每篇文章作为单独的消息发送

## 步骤 1:导入请求和 twilio.rest 库

```
import requests
from twilio.rest import Client
```

请求模块允许您使用 Python 发送 HTTP 请求。HTTP 请求返回一个包含所有响应数据(内容、编码、状态等)的响应对象。

Twilio 是一款允许我们发送短信的网络应用。 [Twilio Python 助手库](https://www.twilio.com/docs/libraries/python)使得从您的 Python 应用程序与 Twilio API 交互变得容易。该库的最新版本可以在 PyPi 上找到。

## 步骤 2:定义全局常数

```
STOCK_NAME = "QCOM"
COMPANY_NAME = "QUALCOMM"

STOCK_ENDPOINT = "https://www.alphavantage.co/query"
NEWS_ENDPOINT = "https://newsapi.org/v2/everything"

STOCK_API_KEY = "<Your Stock API Key>"
NEWS_API_KEY = "<Your News API Key>"
TWILIO_SID = "<Your TWILIO SID >"
TWILIO_TOKEN = "<Your TWILIO Authorisation Token>"
```

> 分别在 alphavantage.co 和 newsapi.org 注册后，股票和新闻 API 密钥都可用。

对于我的目的，我想跟踪高通的股票价格，因此在 STOCK_NAME 中，我给了 QCOM，这是高通在纳斯达克的上市名称。

一旦用户注册了 [twilio](https://www.twilio.com/) ，TWILIO_SID 和 TWILIO_TOKEN 都是可用的。

## 第三步:获取昨天的收盘价

```
stock_params = {
    "function": "TIME_SERIES_DAILY",
    "symbol": STOCK_NAME,
    "apikey": STOCK_API_KEY
}
response = requests.get(STOCK_ENDPOINT, params=stock_params)
data = response.json()["Time Series (Daily)"]
# converting the json data to a list
data_list = [value for (key, value) in data.items()]
print(data_list)
yesterday_data = data_list[0]
yesterday_closing_price = yesterday_data["4\. close"]
print(yesterday_closing_price)
```

> 我们需要昨天和前天的股票价格来计算百分比高低的差异。

“stock_params”列出了将作为查询发送到 alphavantage.co 的参数，或者换句话说，是发送到网页的查询。响应存储在“response”变量中。

为了便于阅读和提取，我们将响应转换成 json 格式。我们关心的是“时间序列(每日)”属性，它基本上列出了一天中的开盘价和收盘价。

我们将 json 数据转换成数据表。

```
data_list = [value for (key, value) in data.items()]
```

为了得到昨天的数据，我们提取第一个索引的值。

```
yesterday_data = data_list[0]
```

为了得到股票的收盘价，我们去掉“4。收盘”是指收盘价。

```
yesterday_closing_price = yesterday_data["4\. close"]
```

## 第四步:获取前一天的收盘价

```
day_before_yesterday_data = data_list[1]
day_before_yesterday_closing_price = day_before_yesterday_data["4\. close"]
print(day_before_yesterday_closing_price)
```

数据列表中的第二个指数包含前天的收盘价。

## 第五步:找出昨天收盘股价和前天收盘股价的差异

```
difference = float(yesterday_closing_price) -float(day_before_yesterday_closing_price)
up_down = None
if difference > 0:
    up_down = "🔺"
else:
    up_down = "🔻"
print(difference)
```

因为，我们还需要知道股票昨天收盘价和前天收盘价的差异。因此，我分配了一个上下箭头表情符号，它存储在“up_down”变量中。

这包含在将发送给用户的 SMS 中。

## 第六步:计算出昨天收盘价和前天收盘价的差价百分比

```
diff_percent = (difference / float(yesterday_closing_price)) * 100
print(diff_percent)
```

## 步骤 7:使用新闻 API 获取关于公司的新闻

```
if abs(diff_percent) > 0:
    news_params = {
        "apiKey": NEWS_API_KEY,
        "q": COMPANY_NAME,
        "searchIn": "title"

    }
    news_response = requests.get(NEWS_ENDPOINT, params=news_params)
    articles = news_response.json()["articles"]
    print(news_response.json())
    print(articles)
```

因为我们想要 diff_percent 的绝对值，所以我们沿着“abs”方法传递它。

> **差价百分比是两天内股票价格的差异。您可以根据需要设置限制。因为，我需要测试我的代码，我在这里给了 0。**
> 
> **但是如果你想将差异百分比改为另一个值，比如说 2%或 5%，而不是 0，你可以编码 2/5。这将确保短信只在价格变化 2%或 5%时发送。**

API 参数包含在 news_params 中，作为请求传递给 API 网页。可以根据需要修改查询参数。

我已经包含了 searchIn 参数和公司名称。searchIn 基本上告诉 API 在哪里搜索公司名称，无论是标题还是描述。

去看看[文档](https://newsapi.org/docs)。

“新闻响应”收集查询数据。“articles”变量主要从 json 数据中提取文章字段。

## 步骤 8:使用 python slice 操作符列出前三篇文章

```
first_three_article = articles[:3]
print(first_three_article)
```

该查询将包含数百篇文章，我们将只关心前三篇文章。因为给所有人发信息会很不方便。谁会阅读 400 篇关于科技公司的文章呢？:)

这里，我使用了 python slice 操作符。请参考此处的文档[。](https://www.geeksforgeeks.org/python-list-slicing/)

## 步骤 9:使用列表理解(短信内容)创建前三篇文章标题和描述的新列表

```
formatted_articles = [f"{STOCK_NAME}: {up_down}{diff_percent}%\nHeadline: {article['title']}, \nBrief: {article['description']}" for article in first_three_article]
```

格式化的文章基本上包含消息内容，包括前三篇文章。在 f 字符串中，我们有股票名称，up down 表情符号，显示股票价格在两天内变化的差异百分比，文章标题和简短摘要。

## 第十步:通过 Twilio 将每篇文章作为单独的消息发送

```
client = Client(TWILIO_SID, TWILIO_TOKEN)
for article in formatted_articles:
    message = client.messages.create(
        body=article,
        from_="<Your Twilio Phone number>",
        to="<Your Phone number>"
    )
```

“客户端”将传递 TWILIO_SID 和 TWILIO_TOKEN，这是 TWILIO 授权用户的方式。在这里查看文档。

运行脚本后。结果如下:

我希望这是一个伟大的学习经验，任何人谁承担阅读和实施它的痛苦😃。你可以在这里找到《T4》的全部源代码。

你喜欢我的努力吗？如果是的话，请跟我来获取我的最新帖子和更新，或者更好的是，请我喝杯咖啡！☕

在社交媒体上联系我👇
[领英](https://www.linkedin.com/in/ayushdixitpage/)
[Instagram](https://www.instagram.com/code.with.ayush/)

[![](img/2093d0f16d94a8942508624035f676b1.png)](https://www.buymeacoffee.com/ayushdixit)[](https://www.buymeacoffee.com/ayushdixit) [## ayushdixit 正在编码、部署项目和写博客

### 嘿👋我刚刚在这里创建了一个页面。你现在可以给我买杯咖啡了！

www.buymeacoffee.com](https://www.buymeacoffee.com/ayushdixit)