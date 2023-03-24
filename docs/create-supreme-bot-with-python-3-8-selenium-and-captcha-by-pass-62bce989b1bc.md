# 用 Python 3.8，Selenium 和 Captcha by-pass 创建一个 Supreme bot

> 原文：<https://medium.com/nerd-for-tech/create-supreme-bot-with-python-3-8-selenium-and-captcha-by-pass-62bce989b1bc?source=collection_archive---------4----------------------->

如何使用 Chromedriver 和 Python 3.8 执行全功能的购买自动化流程，captcha 免费和快速性能。文章由 Louis Brulé Naudet 提供。

**建议**

在 MacOS 11.3 和更高版本的环境中运行 Python 3.8，执行兼容性测试。

使用的库:Six，Selenium，Sys。

实施我们的程序的第一步是配置 chrome 驱动程序，以禁用扩展、用于识别自动化软件控制的插件、可视界面以及指示 Google 由 Selenium 驱动的脚本控制的排除开关。

这样，浏览器控制台就不可能确定条目的控制是由人还是由自动化机器来确定。这种操作允许过滤大部分的验证码检查，消除了通常对基于 Chromedriver 的程序的怀疑。

接下来，有必要为 Selenium 配置定义一些基本数据。首先，执行更新的频率和引发超时异常之前的时间限制。

在 main()函数中，要求用户将其个人数据分配给大量变量，以便填写网站专用页面上的订单验证字段。因此，尊重每个条目的语法和类型是很重要的，特别是地址的格式，根据经验，地址可以分为两个不同的字段，一个是街道号码，另一个是姓名。

在以转售为目的的操纵中，实现“无论产品如何”的命令似乎很重要。因此，soldout_color_testing 类旨在随机选择所需产品的颜色，以防最初单击的商品不可用。

![](img/7430f829b8d189b61bb5d4b00662c6b4.png)

由[大卫·莱斯卡诺](https://unsplash.com/@_thedl?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

该协议是简单的迭代，系统地检查产品在页面上的指示可用性。一旦阻塞条件解除，循环就会被中断。

很自然，下一步就是点击按钮，将想要的商品添加到购物车中。最后一个是通过它的 xpath 地址定位的，这是通过互联网浏览器的开发工具识别的。

最后，只需填写允许将包裹发送到所需地址的字段，以及联系和支付信息。唯一的手动操作是激活表格末尾的接受网站使用的一般条件按钮。

> 此外，即使最初选择的产品的所有颜色都不可用，也可以迭代程序以继续执行。推理是相同的，用户将不得不在显示页面中手动选择另一篇文章。

感谢您的阅读，希望这篇教程让您更好地理解了 Selenium 和 Python 3.8 中的迭代算法在自动化过程中的工作方式。

Louis Brulé Naudet 拥有巴黎萨克莱大学的法律和经济学/管理学双学位。

[](https://louisbrulenaudet.com) [## 路易·布鲁莱·诺代

### Clover 和 Lemone 共同基金会，商务和财政法专业，程序设计界面概念…

louisbrulenaudet.com](https://louisbrulenaudet.com)