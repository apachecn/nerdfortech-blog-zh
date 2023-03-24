# 使用 Node JS 和 Mongo DB 的脸书刮刀机器人

> 原文：<https://medium.com/nerd-for-tech/facebook-scraper-bot-using-node-js-and-mongo-db-cb5864208126?source=collection_archive---------0----------------------->

![](img/9c34ae1606e414e69b2a2349b165ad18.png)

我正在开发一个应用程序，该应用程序需要使用脸书 oEmbed 端点根据话题频道呈现脸书供稿，可以在社交媒体上找到、过滤并与客户互动。脸书 oEmbed 端点允许您获取页面、帖子和视频的嵌入式 HTML 和基本元数据，以便在另一个网站或应用程序中显示它们。由于脸书官方 API 文档不赞成获取最近的媒体，包括提要内容，URL，创建时间戳，喜欢/评论和提要的共享计数，我设计了一个脸书刮刀来提取这些信息。在这里，我将使用 Node JS[puppet er](https://www.npmjs.com/package/puppeteer)作为自动化工具，使用 Mongo DB 存储收集到的提要数据来实现我的解决方案。

Puppeteer 是一个节点库，提供高级 API 来控制 chrome 或 chrome，我们可以在浏览器中手动做的大多数事情都可以使用 puppeteer 来完成。

我研究了很多第三方库，但是有一些限制，因为脸书平台更新了他们的 CSS 样式和类，我们不能使用动态生成的类样式来获得稳定的解决方案。因此，依赖第三方解决方案不是一个好的做法，因为我们不能确保它是否能与脸书最新的更新一起工作。所以我用 Node JS 设计了一个自己的脸书包装器，下面我会一步一步解释我的实现。

所以让我们开始吧。

**先决条件:**

*   节点 JS 的基础知识

**步骤 01 :**

作为第一步，添加了导入木偶库的`initPuppeteer()`方法，启动浏览器并创建一个新页面，以及设置浏览器的宽度和高度并覆盖权限。

**步骤 02 :**

在这一步中，我在一个单独的文件中添加了一些常见的配置消息，如下所示。

这里我们将添加一个`loginFacebook()`来访问脸书网站，并等待网络空闲，直到它出现在登录界面并执行登录操作，如下所示。

我设置了随机超时来等待动作，因为如果动作太快，我们的机器人将很容易被脸书检测到，这可能会导致我们的帐户被阻止或列入黑名单。

这里我通过评估内部 HTML 来选择 UI 元素，因为在脸书选择器是随机变化的。

**步骤 03 :**

在这一步，我们将开始基于过滤标签如`@mentions/keywords/hashtags`的抓取。因此，我的下一步将是根据下面的过滤器标签直接到脸书页面。我已经在我的配置文件中配置了基本 URL。

```
const page_url = config.base_url + filter_tag;await this.page.goto(page_url, {waitUntil: "networkidle2",});
```

接下来，我要检查页面是否可用，以启动如下的抓取步骤。这里我使用了一些 UI 显示文本内容，向用户表明页面的存在。所以我在配置文件中配置了这些可能的消息。

**步骤 04 :**

接下来，如果相关过滤器标签的脸书页面可用，我将滚动页面，并呈现如下 DOM 内容的提要。在这里，我们可以使用`post_count`参数配置要删除的帖子数量。在这里，我发现有一个帖子是围绕着`div[role="article"]`标签的。所以我要数不。通过在页面上滚动来呈现 DOM 内容的`div[role="article"]`标签。

**步骤 05 :**

我的下一步是识别要提取的帖子，因为在上一步中，我们完成了基于`div[role="article"]`标签的滚动逻辑，但我发现它还包括一些其他文本内容。所以我们需要过滤掉那些。在这里，作为一个独特的元素，我已经确定，如果它是一个要提取的帖子，它的`ariaLabel`是一个空值。在此基础上，我将添加一个过滤器如下，对于每一篇文章，我将返回文本内容和内部 HTML。

**步骤 06 :**

下一步，我将遍历我的过滤器列表，提取帖子内容、反应计数、分享计数和评论计数以及帖子创建的时间戳。这里提取这些数字需要通过一个 html 解析器解析 post 内部 HTML，并得到如下的根 HTML。为此，我使用了 [node-html-parser](https://www.npmjs.com/package/node-html-parser) ，它将生成一个简化的 DOM 树，支持元素查询。

```
import { parse } from “node-html-parser”;const root_html = parse(filtered_list[i].html);
```

**提取帖子内容:**

**提取评论数:**

**提取反应计数:**

**提取股份数:**

作为我的下一步，我需要如下格式化这些数据计数。

**提取帖子创建的时间戳:**

这里，脸书平台不允许我们直接从 DOM 内容中抓取时间戳。他们使用样式标签对时间戳字符串中的每个元素进行了排序。所以我们需要遍历 span 标签，并且需要找到这些元素的样式来找到它们的顺序。接下来，在我的逻辑中，我使用样式顺序作为数组索引，将这些元素插入到一个数组中。接下来，我发现有`{display:none;}`样式类，它们以样式顺序注入到这些元素中。所以我需要跳过下面这些元素。

接下来，我们需要将 post 创建的时间戳格式化为通用格式，以便保存在数据库中，为此，我将通过格式化抓取的 post 创建的时间戳来返回纪元时间。

之后，我可以创建后创建的时间保存在数据库如下。

```
const ymd_timestamp = moment.unix(timestamp / 1000).format(config.date_time_format);
```

**步骤 07 :**

作为我的下一步，我需要提取帖子的 URL，以使用脸书 oEmbed 在 web 应用程序中呈现脸书提要，如下所示。从帖子操作中，我将提取 feed oEmbed，并使用此字符串提取帖子 URL 和帖子 id，如下所示。

**步骤 08 :**

作为最后一步，我将使用提取的脸书 feed 记录更新 Mongo DB，格式如下。

```
let dataObj = {post_id: post_id,post_text: post_text,screen_name: filter_tag,post_created_at: ymd_timestamp,attributes: {share_count: share_count,comment_count: comment_count,reaction_count: reaction_count,page_link: page_url,link: post_url,},time_stamp: Math.round(new Date().getTime() / 1000),};
```

这种解决方案有局限性，因为

脸书经常更改 UI 元素选择器，因为我已经对页面进行了评估，以找出可见文本内容并执行抓取。但是也有改变这些因素的风险。所以我们需要维持我们的解决方案。

有安全方面的担忧，好像铲运机在短时间内做了很多动作脸书·布莱克因为超过速率限制上市了一段时间。

感谢您的阅读！