# 我的 Python 样板和一点 Python-Fu

> 原文：<https://medium.com/nerd-for-tech/my-python-boilerplate-and-a-little-python-fu-e0ed59d97627?source=collection_archive---------8----------------------->

让我向您展示我的 Python 样板文件和一些我正在使用的漂亮的标准特性。可以从 github 下载[完整模板。这个帖子以后很可能会延长。](https://gist.github.com/nickyreinert/45c019f318fe7a10232327cdf8b119f4)

# 摘要

样板文件将帮助您实现以下功能:

*   使用虚拟环境
*   管理需求
*   电报通知
*   管理命令行参数
*   列表理解
*   循环打印进度
*   测量时间
*   循环结束后播放声音

# 虚拟环境

我喜欢在虚拟环境中工作。它帮助我处理单独的干净开发环境。你是怎么做到的？切换到您的主开发文件夹，为您的新项目创建一个子文件夹并启动它:

```
cd /development/
mkdir project1
python3 -m venv project1
cd project1
source bin/activate
```

给你。如果您安装软件包，它们仅位于此文件夹中。等等，包裹？好了，让我们进入下一步:

![](img/f982ce455dc0134fbc661b30d347e89c.png)

这篇文章没有任何图片，这就是为什么我向你展示这张我几年前在雅典拍摄的美丽图片——一座美妙的城市！

# 管理您的库

随着 Python 脚本的增长，无论是行数还是特性数，所使用的库的数量也会增长。但是如何在不同的机器上，不同的环境下运行这个脚本呢？如何轻松分享你的剧本？答案是 pipreqs:

```
pip install pipreqs
```

安装这个小工具后，针对您的脚本(或脚本文件夹)运行它:

```
pipreqs yourscript.py # one script only
pipreqs . # all scripts in the folder
```

Pipreqs 将扫描您的脚本中所需的库，并将它们列在一个名为 requirements.txt 的文件中(suprise！).您需要将该文本文件与您的 Python 脚本共享。想要运行该脚本的每个人都需要使用这行代码准备环境:

```
pip install -r requirements.txt
```

就这样。符合所有要求。

# 电报记录

> 请注意:使用像 telegram 这样的集中信息服务，你的内容存放在外国电脑上，可能是一个有风险的计划。我很清楚这一点，你也应该如此。这就是为什么我只和 telegram 分享不敏感的信息。这只是我的完美通知服务。

*电报有什么帮助？*你可能会问。想象一下，您有一个运行数小时甚至数天的脚本！其实你并不是真的知道。当脚本完成时，或者关于它的进展，得到一个消息不是很好吗？

首先让我们为 Telegram 创建你自己的机器人。打开 app，寻找“僵尸爸爸”。他会允许你创造一个新的机器人。名字不重要，重要的是秘密(！)令牌。拿着它和你的机器人聊天吧。然后浏览到此 url(用您的机密(！idspninfopath)替换令牌部分。)令牌):

```
https://api.telegram.org/bot<bot token>/getUpdates
```

该页面将返回一个 JSON 对象和一些信息，其中有你的 chat-id(它也是秘密的，一切都是秘密的！).抓住它和你的秘密放在一起(！！！)令牌进入**你的电报 _ 通知者. py:**

```
import requestsbot_token = 'secret token'
bot_chat_id = 'secret chat id'def send_text_to_telegram(bot_message):
   bot_token = bot_token

   send_text = '[https://api.telegram.org/bot'](https://api.telegram.org/bot') + bot_token + '/sendMessage?chat_id=' + bot_chat_id + '&parse_mode=Markdown&text=' + bot_message response = requests.get(send_text)

   return response.json()def send_image_to_telegram(image): bot_token = bot_token

   data = {'chat_id': bot_chat_id}

   url = '[https://api.telegram.org/bot'](https://api.telegram.org/bot') + bot_token + '/sendPhoto?chat_id=' + bot_chat_id + '&parse_mode=Markdown'

   with open(image, "rb") as image_file:

      response = requests.post(url, data=data, files={"photo": image_file})

   return response.json()
```

*请注意，这个脚本没有错误处理。*

恭喜你，你现在有两个功能来发送文本和图像到你的电报机器人。这将帮助您从 Python 脚本中获得通知。在您的每个实际脚本中导入这两个函数:

```
from telegram_notifier import *
```

# 在媒体分辨率中

## 命令行参数

准备够了，我们来谈谈剧本本身。第一行看起来像这样:

```
import time # for logging purposes
import argparse # for command line argumentsparser=argparse.ArgumentParser()parser.add_argument('--limit', help='Source files in CSV format', required=True, type=int)
parser.add_argument('--loop_length', help='Source files in CSV format', required=False, type=int)
parser.add_argument('--message', help='Give me a message', required=False, default="Hello World", type=str)args = parser.parse_args()
```

您会发现从命令行向脚本传递参数很有帮助。这就是 **argparse** 库的用途。这个库支持其他参数和特性的加载，我只是给你展示一些基本的来读取两个参数**极限**和**循环长度**。您可以使用 **args.limit** 或 **args.loop_length 来读取这些参数。**这么容易。举例？

```
loop_length = 1000000 if args.loop_length == None else args.loop_length
limit = args.limit
```

这是 if 条件的简化版本。如果设定了循环长度，就接受它。否则使用默认值 1.000.000。该限制总是被设置为用户传递的值，因为它是一个强制参数，因此总是存在。下一个，请。

## 记录

为什么日志记录对我如此重要？为什么这对你很重要？**当客户问你**时，你就说到点子上了:

> *用 2mo 处理这个数据集要多长时间。行？这要花多少钱？*
> 
> *~某个客户，某个时间*

或者你想比较两种不同的方法。诸如此类。我是(广泛的)日志记录的狂热爱好者。很有帮助。相信我。那么，我们如何开始？

```
feedback_frequency = 10                                               count = 1  
start = time.time()
```

频率？为什么？假设您有一个处理 100 万次的脚本。行。根据周围的参数，它要么很快，要么很慢。何时创建日志消息？使用您定义的 feedback_frequency，您希望查看日志消息的次数。将其设置为 10 以每 1 Mio 获得一次反馈。/ 10 行。增加它以获得更多的反馈。

## 颜色；色彩；色调

如果您向控制台发送大量信息，您可能希望使用彩色输出。首先让我们为此定义一个自定义类。如您所见，这也允许您将字体样式设置为粗体或下划线。

```
class bcolors:
    HEADER = '\033[95m'
    OKBLUE = '\033[94m'
    OKCYAN = '\033[96m'
    OKGREEN = '\033[92m'
    WARNING = '\033[93m'
    FAIL = '\033[91m'
    ENDC = '\033[0m'
    BOLD = '\033[1m'
    UNDERLINE = '\033[4m'
```

现在，将该类实现到输出中，如下所示:

```
print(f'{bcolors.FAIL}{bcolors.BOLD}Bold red text{bcolors.ENDC}')
```

## 一个简单的循环——理解

列表或字典理解功能是你喜欢或不理解的功能。我甚至不确定我是否真正理解这个特性。然而，这里有一个例子有希望解释它。

列表理解也适用于字典。它允许您循环遍历数据，而无需编写一个完整的循环。它应该比普通的循环更快。

以下示例遍历从 1 到 loop_length=1.000.000 的数字范围(*倒数第二行*)。

本理解的最后一行说:如果索引大于 10，只处理当前行。

*第一行*只是取索引值。用它工作。加算，四舍五入，计算。这就是我们的列表 **a_list** 中的值。

*第二行*是处理条件的第二种方式:根据指标的实际值，定义一个结果。如果指数小于 50，则继续传递。否则传递在 **default_value** 中定义的值。

```
default_value = 'x'
a_list = [
    index
    if index < 50 else default_value
    for index in range(1, loop_length)
    if index > 10
    ]
```

我知道这很棘手。只要你需要，你就会得到它。；P

## 事实上是伐木

接下来的几行将向电报和标准输出发送一条消息。

```
duration = time.time() - start message = '\r\nDone with list comprehension after %s seconds ' % (      '{:.2f}'.format(duration) # two decimal places
)
                                               send_text_to_telegram(message)                                               print(message, flush=True)
```

除了 flush-Parameter 之外，您应该从代码中理解这一点。这个命令强制 Python 脚本将消息发送到输出，而不等待脚本结束。这就是你的实时更新保证！

## 成熟的循环

这是一个完整的循环，允许您添加任意多的条件和计算。

```
for i in range(1, loop_length): if i > limit - 1 and limit > 0: break

   count += 1 if i % (limit / feedback_frequency) == 0 or i % (round(loop_length / feedback_frequency, 0)) == 0:

      progress = i / limit if limit > 0 else i / loop_length message = (
            '%s%%...' % 
            ('{:.1f}'.format(100 * progress))
        ) print(
            message,            
            end = '', # no line break
            flush=True # don't buffer output
      )
```

第一行很简单。如果达到极限，则中断循环。最令人兴奋的是日志功能。如上所述，它只会记录符合频率要求的消息。同时检查印刷的**终端参数**。它从 print-command 中删除了换行符。

## 最终消息

最后，在我们的脚本逻辑完成之后，我们发送一条消息。这一次也来电报了。

```
duration = time.time() - startmessage = '\r\nDone with processing %s iterations after %s seconds ' % (
    '{:0,.0f}'.format(count), # formatting integers with thousand separator
    '{:.2f}'.format(duration) # formatting decimals with two decimals
    )send_text_to_telegram(message)print(message)
```

这里没有什么特别的事情发生。唯一有趣的是，电报也接受图像。如果您正在创建一些复杂的统计分析并希望看到输出，这是很好的。再次提醒你:记住 Telegram 把你的信息存储在国外服务器上。记住这一点。

# 临终遗言

我一直在学习，我希望你也学到了一些东西。以后我打算把这个帖子延续下去。如果你有更多的技巧给我，给我留言。