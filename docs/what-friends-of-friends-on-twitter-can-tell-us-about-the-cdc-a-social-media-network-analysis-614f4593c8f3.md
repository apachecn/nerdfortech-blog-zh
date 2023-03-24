# Twitter 上的朋友能告诉我们关于 CDC 的什么——一项社交媒体网络分析

> 原文：<https://medium.com/nerd-for-tech/what-friends-of-friends-on-twitter-can-tell-us-about-the-cdc-a-social-media-network-analysis-614f4593c8f3?source=collection_archive---------15----------------------->

![](img/f55a6a78f6f98db2aae46d293f5756fd.png)

# **简介**

社交媒体平台已经成为十年来最受欢迎的技术之一。截至 2019 年，超过 72%的美国人使用某种类型的社交媒体，每天都有大量的人访问这些平台，大量用户每天花几个小时在这些平台上。[1]在 YouTube、脸书、Instagram、Reddit 和 Twitter 之间，社交媒体的用途是多种多样的——从上传视频、发布自拍、分享迷因到与朋友和家人保持联系或与在线社区建立联系。

然而，社交媒体领域也正在成为激进主义、政治和新闻的重要门户。越来越多的美国人比以往任何时候都更依赖这些平台来了解时事，Twitter 是用户关注新闻最多的网站之一。2018 年，71%的美国用户使用该网站获取新闻，Twitter 现在的主要目的之一是帮助公众保持知情。[2]在经历了 12 个月的总统选举、政治丑闻和疫情病毒爆发之后，2020 年给了世界许多信息需要消化。

也许过去一年最有新闻价值的话题是冠状病毒的传播，Twitter 用户也一直在关注这个话题。数百万人在推特上谈论新冠肺炎的演变，许多健康专家(如安东尼·福奇博士)在推特上告诉公众如何应对这种疾病，许多医疗机构(如疾病控制和预防中心)在推特上定期报告最新的病毒统计数据。Twitter 包含了一系列提到冠状病毒的推文，拥有大量数据，该网站甚至被科学研究人员用来跟踪新冠肺炎的传播。[3]

然而，尽管 Twitter 被用作信息枢纽，但它也成为了错误信息的热点，特别是关于冠状病毒的错误信息。健康和医学是两极分化的政治话题，新冠肺炎阴谋论比比皆是，对疾病预防控制中心等机构或福奇等专家的不信任达到了前所未有的高度。[4]

疾病控制中心尤其是这种怀疑的目标。许多公众人物，包括前总统和政治家，破坏了它曾经可信的声誉，歪曲它的声明，并对它发出严厉的批评。另一方面，对行政机关的怀疑不仅仅是第三方的过错。在冠状病毒期间，CDC 本身对其脆弱的可信度记录负有部分责任，因为它在报告中产生了明显的沟通失误和错误。[5]有些人认为，疾病预防控制中心也屈服于政治影响，削弱了它有效处理这场危机的能力，讽刺性的新闻网站嘲笑该机构在新冠肺炎问题上是一个不可信赖的权威。[6]

社交媒体在健康和医疗领域的角色是全新的，难以驾驭。信息和错误信息像野火一样在未被驯服的数字荒野中蔓延。必然如此，官员们前所未有地对自己的言论负责，因为 Twitter 上的每条推文都以近乎永久的记录存在，供世界上的每个人查看。像疾病预防控制中心这样的机构必须比以往任何时候都更了解情况，拥有更多的资源，才能向公众证明自己的可信度。

对于疾病预防控制中心来说，其更大的军火库中的一个资源包括其主要档案之外的各种社交媒体页面。除了 CDC 的主要用户名“@CDCgov”，该机构还与数十个二级 Twitter 账户相关联——所有这些账户都代表了一个独特的兴趣领域，致力于为特定受众提供专门的内容(通常不提及冠状病毒)。其中一些配置文件是针对特定地区的，如“@CDCRwanda”，而其他的则是针对某些医疗问题的，如“@CDCHeart_Stroke。”[7]

虽然数百万人在 Twitter 上关注疾控中心的主要资料，但这些外围资料的关注者往往少得多，往往只有数千人。虽然@CDCGov 本身只关注少数用户，也就是 Twitter 上的朋友，但其中许多朋友都有自己的个人资料。乍一看，这看起来像一个孤立的网络，只有一组单一的兴趣和有限数量的与其他背景用户的链接。

但是疾控中心名单上的其他朋友呢？除了与该机构直接相关的用户之外，CDC 认为哪些额外的帐户足够重要，值得关注？那些账号*关注哪些用户？在这个社交媒体网络的外围用户之间存在什么样的连接？*

**更重要的是，我们能够根据疾病控制中心为自己建立的朋友网络来评估它的品质吗？**

我们能否根据直接或间接关联的用户来发现任何潜在的政治倾向、从属关系或对某些团体或观点的密切关系？我们能从这个网络中看到任何清晰定义的社区吗？通过了解 CDC 在社交媒体上的关系网，我们可以获得什么样的见解？或许我们可以从疾病预防控制中心与其他社交媒体用户的联系中了解到它是否真的是一个值得信赖和公正的信息来源？

# **网络分析**

为了探索这些问题，我们必须进行**网络分析**——一种通过分解和可视化数据，从相互联系的元素(如社会结构中的个人)之间的复杂关系中提取有用信息的方法。网络通常以图表的形式表示，图表上的点或**节点**作为单独的元素，节点之间的线或**边**作为不同元素之间的关系。

![](img/3e973ecd8de2c7e3f03861920d250ddc.png)

基本自我网络示例

一种类型的关系结构被称为**自我中心网络**，它由单个中心节点或**自我**组成，连接网络中的所有其他节点或**改变**。在这种情况下，中心节点或 ego 将是 CDC 的主要 Twitter 帐户，直接围绕 ego 的节点或 alters 将是 CDC 关注的所有 Twitter 帐户，围绕 alters 的节点将是 alters 关注的 Twitter 帐户的集合。这个网络也将被可视化为一个**无向图** **图**，其中节点之间的边描绘了账户之间的友谊或相互关注——与**有向图**相反，在有向图中，不同元素之间的关系只向一个方向流动，例如 Twitter 关注是不相互的。

# **API、库和程序**

执行适当的社交媒体网络分析需要一套特定的工具:

*   Twitter 开发者账户
*   带 Python 的 Jupyter 笔记本
*   Tweepy，Twitter API 的 Python 库
*   Pandas，一个用于组织和分析数据的 Python 库
*   NetworkX，用于创建和操作网络的 Python 库
*   Gephi，一个可视化网络和图形的桌面程序

# **连接推特**

首先，我们必须申请一个 Twitter 的开发者账户。[8]一旦得到 Twitter 工作人员的批准，我们就可以查看我们的应用程序设置，看看我们的密钥和访问令牌是什么。然后我们可以打开 Jupyter Notebook 并安装 Tweepy，我们可以使用 pip 安装 Tweepy。

```
pip install tweepy
```

从这里，我们将库导入到我们的笔记本中，并创建一些新的标识符来表示我们的密钥和令牌(这里用 X 表示，以保持我自己 ID 的匿名性)。

```
import tweepyapi_key = “XXX”api_secret = “XXX”bearer_token = “XXX”
```

之后，我们授权 tweepy 使用我们的 Twitter 凭证并创建一个 API 对象。因为 Twitter 对开发人员在给定时间内可以检索的 tweets 或帐户的数量有一定的速率限制，所以我们希望我们的 API 在达到速率限制时暂停工作，然后在下一个时间窗口打开时继续工作。[9]

```
auth = tweepy.AppAuthHandler(api_key, api_secret)api = tweepy.API(auth, wait_on_rate_limit=True)
```

当回答需要通过大量信息迭代的 API 请求时，Twitter 在一系列离散页面中反馈其数据，这有助于返回比单个响应中检索到的更多的结果。这种方法称为分页。[10]

因此，一些 API 请求需要输入页面参数来遍历分页数据。幸运的是，Tweepy 库提供了一个 Cursor 对象来帮助我们在幕后遍历这些页面，消除了我们提供页面参数的需要，简化了我们的代码，使我们的生活变得更加轻松。[11]

我们可以看到疾控中心正在关注 269 个不同的 Twitter 账户，所以让我们输入这个数字。

```
for friend in tweepy.Cursor(api.friends, id=”CDCgov”).items(269):print(“CDCgov:”, friend.screen_name, “|”, friend.name)
```

该程序打印了疾控中心账户上的所有 269 位好友。以下是前 10 个。

> *CDCgov: JAMA_current | JAMA*
> 
> *CDCgov: WHCOVIDResponse |白宫新冠肺炎响应小组*
> 
> *CDCgov: YouTube | YouTube*
> 
> 国家农村卫生办公室组织
> 
> *CDC gov:rural health info | rhi hub*
> 
> *CDC gov:realtimevid 19 |新冠肺炎实时学习网络(RTLN)*
> 
> *CDCgov: CDC_Firstline | CDC 的项目 Firstline*
> 
> 联邦调查局|联邦调查局
> 
> *CDCgov: CDC_DRH | CDC 生殖健康科*
> 
> *CDC gov:Nida news | Nida news*

现在让我们为 CDC 的前三个朋友中的每一个尝试三个朋友的朋友。

```
for friend in tweepy.Cursor(api.friends, id=”CDCgov”).items(3):print(“CDCgov:”, friend.screen_name, “|”, friend.name)for friend_of_friend in tweepy.Cursor(api.friends, id=friend.screen_name).items(3):print(“\t\t”, friend_of_friend.screen_name, “|”, friend_of_friend.name)
```

> 贾马当前|贾马
> 
> 南希·梅索尼埃博士
> 
> 尼尔动力|尼尔动力
> 
> 丹尼斯·肖尔腾斯
> 
> 白宫新冠肺炎响应小组
> 
> *疾病预防控制中心主任|罗谢尔·瓦伦斯基，医学博士，公共卫生硕士*
> 
> *副总裁|副总裁卡玛拉·哈里斯*
> 
> 总统|拜登总统
> 
> *CDCgov: YouTube | YouTube*
> 
> *uni_mugi_hachi | 仲良し保護猫 うに むぎ はち むー😸🇯🇵*
> 
> ⚡️·佐伊·克拉普·⚡️
> 
> *itsjojosiwa | JoJo Siwa！🌈❤️🎀*

如果我们想知道每个疾控中心的朋友有多少朋友呢？

```
for friend in tweepy.Cursor(api.friends, id=”CDCgov”).items():print(“Friends of”, friend.screen_name, “|”, friend.name, “:”, friend.friends_count)
```

> *JAMA_current | JAMA : 863*
> 
> 白宫新冠肺炎响应小组:4 人
> 
> *YouTube | YouTube : 1201*
> 
> *NOSORH |国家农村卫生办公室国家组织:375*
> 
> *rural health info | rhi hub:355*
> 
> *Real Time vid 19 |新冠肺炎实时学习网(RTLN) : 30*
> 
> CDC_Firstline | CDC 的项目 Firstline : 21
> 
> *FBI | FBI : 2126*
> 
> *疾控中心 _DRH |疾控中心生殖健康科:131*
> 
> *NIDAnews | NIDAnews : 264*

这些数字变化很大。要查看作为 CDC 好友的所有帐户的最高和最低好友计数，我们可以创建一个列表，将其余的好友添加到列表中，将一些列表值添加到 Pandas 数据帧中，并按好友计数对数据帧进行排序。

```
numFriends = []for friend in tweepy.Cursor(api.friends, id=”CDCgov”).items():numFriends.append(friend)friendFrame = pd.DataFrame()friendFrame[‘screen_name’] = [friend.screen_name for friend in numFriends]friendFrame[‘name’] = [friend.name for friend in numFriends]friendFrame[‘friend_count’] = [friend.friends_count for friend in numFriends]friendFrame = friendFrame.sort_values(by=[‘friend_count’])friendFrame
```

![](img/10596a58a30165c225a39d5713dd0892.png)

按朋友计数排序的 CDC 朋友

现在我们知道我们在处理什么样的范围了:RNsightsSchoolNurses 和白宫新冠肺炎响应团队的 Twitter 账户的好友数量最少，分别为 3 和 4，而美国内政部和美国癌症协会的好友数量分别为 127，768 和 175，736。通过熊猫求和功能，我们还可以获得列表中每个朋友的朋友总数。

```
friendFrame.sum(axis=0)friend_count 671333
```

这是一个很大的推特账户。根据 API 的速度限制，检索这个网络中每个朋友的朋友的名字需要 180 多个小时。这相当于连续 8 天的自动请求，这不是一个可行的工作时间框架。如果我们想分析这个网络，为了节省时间，我们必须做出一些妥协。由于 CDC 有 269 个自己的朋友，让我们设置一个严格的上限，即从 CDC 的每个朋友那里获取 269 个其他朋友。如果这 269 个用户中的每个人都有 269 个自己的朋友，那么我们的网络可能有多达 70，000 个节点，并且可能需要长达 24 小时来提取。这些都是很大的数字和时间范围，但它们只是在可行的范围内。

我们的下一步是导入 NetworkX，从 Twitter 上调用 CDC 的朋友和朋友的朋友，将所有的朋友和关系作为节点和边添加到图中——然后等待。

```
import networkx as nxgraph = nx.Graph()for friend in tweepy.Cursor(api.friends, id=”CDCgov”).items(269):graph.add_node(friend.screen_name)graph.add_edge(“CDCgov”, friend.screen_name)print(“Working:”, friend.screen_name)for friend_of_friend in tweepy.Cursor(api.friends, id=friend.screen_name).items(269):graph.add_node(friend_of_friend.screen_name)graph.add_edge(friend.screen_name, friend_of_friend.screen_name)print(“\t\t”, friend_of_friend.screen_name)
```

在等待了一整天并观察程序打印出用户名之后，图就创建好了，我们可以将它作为文件写给 Gephi。

```
nx.write_gexf(graph, “CDCgraph.gexf”)
```

# **统计和术语**

当我们第一次在 Gephi 中打开我们的文件时，我们知道我们有 35，262 个节点和 60，702 条边——一个巨大的 Twitter 连接网络。虽然图中确实存在节点和边，但它们没有以任何方式组织起来，整个集群看起来只不过是一个密集的黑色方块。为了将这个网络组织成一个更有意义的形式，我们必须运行一些测试来获得关于它的某些统计数据。

![](img/7ce78a9b131ff8efec4d611c9b84d97a.png)![](img/b673f5ead2f887ef362c85f7d92df314.png)

导入到 Gephi(左)时的节点数和边数，Gephi 中的无组织图形(右)

![](img/f3ea42bee6465aef7284a52ff6c6d74c.png)

节点的**度**是它与网络中其他节点的边数，而图的**平均度**是每个节点的平均边数的度量。因为这个图的边没有权重，**平均加权度**和平均度都是一样的，平均每个节点 3.443 条边。

**网络直径**是网络中两个最远节点之间的最短距离。因为这个图只包括朋友的朋友，所以任何一对节点之间最多有 4 步——CDC 在中间，CDC 的朋友在两边，朋友的朋友在最远的两端。

![](img/b79f4f798f511c443cf92e2b0299df70.png)![](img/ae3de5a66e2e516aa8fe86b72d7fa4e5.png)

贴近度中心分布(左)，特征向量中心分布(右)

**接近中心性分布**显示了网络中每个节点的**中心性**的度量，或者该节点平均离所有其他节点有多远。紧密度高的节点到其他节点的距离最短。

**特征向量中心性** **分布**显示了网络中每个节点具有的**影响**或重要性的度量，这是节点自身的度和它所连接的节点的度的因素。网络中具有最大影响力的节点是那些连接到大多数其他节点的节点，这些节点本身也具有很大的影响力。

![](img/137d6e853b41d38c41f541467b163260.png)

模块化报告

**模块性**显示了如何根据不同节点组之间的连接将网络划分为不同的社区或集群。具有高模块化的网络的特征是节点密集地连接到它们自己的模块内的节点，但是稀疏地连接到它们的模块外的节点。分辨率为 1 时，该网络的模块性为 0.652，有 48 个不同的社区。然而，只有少数社区是相当大的，最大的社区包含 10.99%(大约 3800 个)的节点，几十个最小的社区仅包含 0.66%-0.75%(在 200 年代中期)。这是有意义的，因为 CDC 的 269 个朋友中的一些有多达 269 个他们自己的朋友，但是与 CDC 的其他朋友的关系稀疏——导致许多相对孤立的模块具有有限数量的节点。

![](img/1cbe68b72b846d1dac1da5c57ec9bbef.png)

我们还可以打开数据表来检查每个节点的统计数据。有趣的是，虽然 CDCgov 是我们的焦点节点，并且是具有最高特征向量中心性的节点(或具有最大影响力的节点)，但它并没有最高的度(或与其他节点的边)，甚至没有列在前 10 名中。尽管我们只允许 API 从每个帐户收集最多 269 个好友，但是最高的度节点是 CDCDirector，得分为 338。这是因为 CDCDirector 不仅链接到它自己的 269 个朋友，它还在许多其他帐户的列表中作为朋友出现。因此，来自其他帐户的连接被计入其自身，其程度高于其他任何人。

![](img/2cca42e8729ef8fa751e071f15977b26.png)![](img/373482f2bcabca2b9b4a3aa0ba13b5db.png)

按特征向量中心性排序的节点(左)与按度数排序的节点(右)

现在我们已经对我们的网络进行了一些统计，我们可以开始将它组织成一个更清晰的组合。通过使用 OpenOrd 布局调整，我们可以将节点和边分散到一个紧密缠绕但均匀分布的重叠连接网络中。然后，我们可以为不同的模块应用一组颜色来突出节点和边缘。因为有这么多不同的社区，很难解释所有的社区，所以我们必须决定只关注最重要的社区。6 种加粗颜色应用于 6 个最大的社区，6 种去饱和颜色应用于接下来的 6 个社区，其余较小的社区设置为中性灰色调。然后，我们可以运行 ForceAtlas 2 来扩展每个模块，以便各个节点更加可见。

![](img/377ef3acd2f209119993c21947e2734d.png)![](img/fefd4d5a8dc7a84310b5ad81514f44d5.png)

OpenNord 后的网络(左)与模块着色后的网络(右)

![](img/04c7b785ae928b38ea52d0051018a9eb.png)

ForceAtlas 2 之后的网络

虽然这个网络在这一点上还很难理解，但我们仍然可以看到一些有趣的模式出现。大多数模块的大小和形状相似，每个模块由 CDC 的一个朋友和多达 269 个自己的朋友组成。许多模块彼此相距遥远，互不相同，这表明社区与网络的其他部分联系很少。然而，作为一个以自我为中心的网络，所有节点和社区都从中心的单个焦点节点向外辐射，距离中心最近的模块开始合并在一起。红色的模块#3 是最大、最密集和最集中的社区，占网络的 10.99%。蓝色的模块#23 是第二大模块，占 8.64%，其节点的很大一部分与模块#3 融合在一起，几个较小的社区与外围的其他社区分离开来。绿色模块#16、橙色模块#21、青色模块#41 和粉色模块#15 都更小且更分散，但仍相对密集且位于中心。之后的每个模块在大小、密度和中心性上逐渐减少。

# **识别组**

现在，为了理解每个模块的身份，我们必须将标签应用到所有单独的节点上。我们还可以调整每个节点的大小，以放大网络中最有影响力的元素，同时缩小不太重要的节点。然而，我们在使用标签时会遇到一些问题…

![](img/f5877ec6d91c4082ae287558cddf1d1b.png)

放大影响节点后的网络特写

![](img/33d1ea616b3c8a3c111a3c60d83eff76.png)

标记节点后的网络特写

网络绝对是巨大的，节点极其密集，标签一个叠一个。我们理解这一点的唯一方法是使用标签调整功能，该功能将每个节点推离其他节点，使其标签更加可见。然而，label adjust 运行得非常慢，我们也没有兴趣检查网络中所有的 35，000 个节点。为了加快这个过程，我们可以隔离每个社区，将网络的其余部分灰化，除去除了我们已经选择的标签之外的所有标签，然后仅在图形被简化之后运行该函数。

![](img/b6da5bbe6ff99501a96dacc38a09418b.png)

模块#3 特写

![](img/058e838094dfd6a4ec51fe49247b5af0.png)

LabelAdjust 后模块#3 的特写

![](img/e3a45b1babe7261555a5ec1430caafd6.png)

模块#3 的数据表

它并不完美，但现在我们可以看到模块#3 中最有影响力的帐户的名称，这是网络中最大和最核心的模块。我们还可以检查模块 3 的数据表，以便进行更全面的分析。在这个大约 3800 人的社区中，最突出的互连节点似乎是用户名与 CDC 直接相关的配置文件，如我们的中心节点 CDCgov，以及 CDC_eHealth、CDCDiabetes、CDCemergency、CDCPCD、CDC_NCBD、CDCChronic、CDCInjury、CDCEnvironment、CDCespanol 和其他几个节点。此外，该组中与 CDC 没有直接关系的最有影响力的用户包括那些名称与 FDA(食品药品监督管理局)、HHS(卫生与公众服务部)、NIH(国家卫生研究院)以及其他与卫生相关的美国政府机构或非盈利组织相关的用户。考虑到 CDC 自身作为美国卫生机构的地位，这些都不足为奇。然而，在这个模块中，即使没有数千个节点，也有数百个节点不容易识别，甚至不容易看到。虽然放大图形以便更仔细地观察很容易，但是确定模块中每个节点的性质将是一项乏味且耗时的任务。

![](img/74c23b5cd3f417c1a56deadfb01a1f7c.png)

模块#23 的全景

![](img/9153cd535d54548c320ed25164b716d6.png)

标签后模块#23 的特写镜头

![](img/78bdb2966787b5fa96de732ae26684ec.png)

模块#23 的数据表

![](img/cd6b12590abda1458f7b466deb94c408.png)

模块#23 的外围节点

![](img/330fbee6ffe61900fc2bac49cac6df9f.png)

模块#23 的外围节点

与模块#3 相交的是模块#23 的第二大组，仅超过 3000 个节点。为了组织这一部分，我们重复与上次相同的步骤。虽然以前的社区位于图形的中心，大量的节点聚集在它的核心周围，但是这个模块更加分散。与网络焦点非常接近的许多最突出的节点都与 CDC 相关，就像上一个集群中的节点一样。然而，这些疾病预防控制中心的账户似乎更侧重于全球某些人或地区，而不是解决具体的健康问题——CDCDirecotr、DrKhabbazCDC、DrMartinCDC、CDCGlobal、CDCGlobalJobs、CDCTravel、CDCMalawi、CDCHaiti、cdcsoutha 非、CDCKenya、CDCRwanda、CDCNamibia 等。在这个区域的边缘还有 USAID、StateDept 和 TravelGov，这进一步表明这个群体的用户集中在国家问题上。单从它们的名字来看，Interior、American_Heart 和 DeptVetAffairs 的外围节点也与该组的其他节点有相似的兴趣，但它们与中心的距离意味着它们与 CDC 直接关联的边较少。

![](img/a578816a5772c9016cf2a00fe36b5419.png)

模块#16 的全景

![](img/062a28b7291adf5ab6b31ea0f3c1e3d4.png)

标签后模块#16 的特写镜头

![](img/b16d8c45edf68a7079af2d2076c2f8e2.png)

模块#16 的数据表

![](img/61b44ec24091d06e04d01918c2309d24.png)

模块#16 的外围节点

标记为模块#16 的下一个集群比其他两个集群更小、更不集中，只有不到 2，500 个节点，但是它的一些用户仍然与中间的社区重叠。然而，该模块看起来不太关注与健康相关的账户，而更关注与美国政府相关的账户，其中最具影响力的节点具有用户名，如 USAGov、USAGovEspanol、USDOL、DHSgov、Readygov、Digital_Gov、FBI、EPA、fema、femaregion2 和 FEMA_fenton。出乎意料的是，这个模块中唯一一个不靠近主要群体的主要节点是 nycHealthy，它自己的许多朋友都提到了纽约市。

![](img/7ac9f53c435b6778b9bdebe06604c4ae.png)

模块#21 的全景

![](img/d5017a734ede6acc52dbc218feb8bb97.png)

标签后模块#21 的特写镜头

![](img/eedc4249c00be2af3d957a0984f97c2f.png)

模块#21 的数据表

![](img/34ea0f53df2aaa663c7c5c8b0a34a26c.png)

模块#21 的外围节点

![](img/795bb15799dff3775dd4b91bc2110dbd.png)

模块#21 的外围节点

大约 1750 个节点的模块#21 很有趣，因为它几乎完全由名字中带有“NIOSH”的有影响力的用户以及这些用户的朋友组成。NIOSH、nioshbreathe、NIOSH_TWH、NIOSHMining、NIOSHConstruct、NIOSHNoise、NIOSHFACE、NIOSH_NPPTL、NIOSHFishing、NIOSH_MVSafety、NIOSHOilandGas 等。快速的网络搜索显示，NIOSH 代表国家职业安全与健康研究所，是疾病预防控制中心的一部分。NIOSHespanol 子组与此相关，但更多的是外围的离群值，与模块中心的链接较少，这可能是由于各个用户之间的语言或区域差异，而 USDA(美国农业部)位于图表的最边缘，与模块#16 的其他有影响力的美国政府节点共享许多相似的朋友。

![](img/3725c307256811dbcb3ae1041b69e2ff.png)

模块#41 的全景

![](img/a1893ff7ab4fd878a6c796089694b811.png)

标签后模块#41 的特写镜头

![](img/878a3c997eb08c0c0862769bbd1baef5.png)

模块#41 的数据表

![](img/a2035f5d97399c8ff125e7fd51e28a65.png)

模块#41 的外围节点

第二组约有 1，750 个节点，是模块#41，由更有影响力的节点和与 CDC 直接相关的用户组成— DrNancyM_CDC、CDC_TB、CDC_DASH、cdchep、CDCPIN、CDCSTD、DrDeanCDC 等。该组中的许多其他节点也与健康相关，如注射安全性、lapublichealth 和 AmericanCancer 下的外周亚群。

![](img/4ef250d4af4c6efc84e1d7b785446868.png)

模块#15 的全景

![](img/6cedefc41c253395b93fc724c709a22e.png)

标签后模块#15 的特写镜头

![](img/1fbef517a5e5d0ae22c8a01c00c7aa12.png)

模块#15 的数据表

![](img/1004e1f50ebf1efdb859bfe6eadde0fa.png)

模块#15 的外围节点

值得一提的最后一个模块是大约 1，500 个节点的模块#15，其最有影响力的节点再次围绕健康主题，特别是艾滋病毒，其中最突出的是 HIVGov、talkHIV、HIVinfo_NIH，仅次于 DrMerminCDC、nationshealth、ANACnurses 和 HamCoHealth 下的外围子组。

之后，接下来的 6 个模块在大小、密度、中心性和影响力上开始下降。虽然这些模块中的一些彼此保持一定的接近度，但是它们的连接是松散的，并且随着它们的总节点百分比的每一次下降，它们变得与图的其余部分不那么相关。到了第 13 大模，只包含全网的 1.96%，之后的任何一组都不值一提。

![](img/adcaef603f855aea9554aa4e93f2421a.png)

网络中的更多模块

唯一引起一些兴趣的额外模块是两个远离网络其余部分的模块，它们以自己的权利脱颖而出——一个是基于 Todobebe 的子组，Todobebe 是一个关于生育和婴儿健康的西班牙语 Twitter 帐户，另一个子组是基于 HomeDepotFound，Home Depot Foundation。

![](img/487a80a7fb385aad4354d6622c1ff126.png)![](img/660ddf86b24069a67f1678b7eb23bfb2.png)

Todobebe(左)和 HomeDepotFound(右)下的远程模块

# **限制**

虽然这个项目的规模出乎意料的大，但是它也受到了严重的限制。首先，API 的速率限制阻止了在合理的时间内完全检索 CDCgov 的所有朋友和朋友的朋友，如果不考虑时间限制，这将导致超过 650，000 个个人用户。最重要的是，Tweepy 按照添加顺序提取 Twitter 好友和关注者，因此我们最终获得的实际好友列表是任意的。由于这些因素，这一分析从一开始就不完整。然而，即使 35，000 个用户也是如此巨大的节点数量，以至于这个图几乎难以辨认。如果不依靠其他数据分析软件，根本没有办法检查那么多 Twitter 账户。我们也许能够指出具有可识别的用户名和高度中心性或影响力的有趣节点，并且我们也许能够根据这些用户模块中其他用户的名字来识别这些用户的一些兴趣，但是如果不从他们的 Twitter 账户中提取更详细的信息，就没有办法对他们进行真正的评估，或者对他们成为 CDC 的朋友意味着什么。

# **结论**

不幸的是，我们从这一分析中获得的唯一主要见解是可预测的——这个网络的最核心用户是疾控中心的其他分支机构，疾控中心社交媒体网络中大多数其他关系良好的用户都与政府机构和卫生组织有关，少数与这些主题不相关的用户也与网络其余部分的其他用户联系较少。事后看来，认为我们可以根据 CDC 的社交媒体网络来判断它的性质，听起来是一个有缺陷的概念。如果不对每个用户进行广泛的研究，也没有能力将他们的个人特征量化成可测量的东西，这个项目的结果是不确定的。

# **参考文献**

[1][https://www . pewresearch . org/internet/fact-sheet/social-media/](https://www.pewresearch.org/internet/fact-sheet/social-media/)

[2][https://www . pewresearch . org/fact-tank/2019/08/02/10-facts-about-Americans-and-Twitter/](https://www.pewresearch.org/fact-tank/2019/08/02/10-facts-about-americans-and-twitter/)

[3][https://news . gsu . edu/2020/04/06/佐治亚州-州立大学-追踪-新冠肺炎/](https://news.gsu.edu/2020/04/06/georgia-state-university-tracking-covid-19/)

[4][https://www . nytimes . com/2020/03/08/technology/coronavirus-misinformation-social-media . html](https://www.nytimes.com/2020/03/08/technology/coronavirus-misinformation-social-media.html)

[5][https://www . nytimes . com/2020/08/31/opinion/CDC-testing-coronavirus . html](https://www.nytimes.com/2020/08/31/opinion/cdc-testing-coronavirus.html)

[6][https://www . the onion . com/CDC-unveils-list-of-Twitter-accounts-you-can-follow-to-1845993857](https://www.theonion.com/cdc-unveils-list-of-twitter-accounts-you-can-follow-to-1845993857)

[https://www.cdc.gov/socialmedia/tools/Twitter.html](https://www.cdc.gov/socialmedia/tools/Twitter.html)

[https://developer.twitter.com/en/apply-for-access](https://developer.twitter.com/en/apply-for-access)

[9][https://developer . Twitter . com/en/docs/Twitter-API/rate-limits](https://developer.twitter.com/en/docs/twitter-api/rate-limits)

[10][https://developer . Twitter . com/en/docs/Twitter-API/pagination](https://developer.twitter.com/en/docs/twitter-api/pagination)

[11]https://docs.tweepy.org/en/v3.10.0/cursor_tutorial.html