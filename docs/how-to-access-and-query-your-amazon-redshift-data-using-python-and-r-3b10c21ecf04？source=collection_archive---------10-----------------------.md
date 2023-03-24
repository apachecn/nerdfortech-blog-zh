# 如何使用 Python 和 R 访问和查询您的 Amazon 红移数据

> 原文：<https://medium.com/nerd-for-tech/how-to-access-and-query-your-amazon-redshift-data-using-python-and-r-3b10c21ecf04?source=collection_archive---------10----------------------->

![](img/3fb4db31e0f71b674e75824e43546347.png)

# 概观

在本帖中，我们将看到如何使用 Python 访问和查询您的 Amazon 红移数据。在这个过程中，我们遵循两个步骤:

*   连接到红移仓库实例并使用 Python 加载数据
*   查询数据并存储结果以供分析

由于 Redshift 与 PostgreSQL 等其他数据库兼容，我们使用 Python [**psycopg**](https://www.psycopg.org/) 库来访问和查询 Redshift 中的数据。然后，我们将使用 [**SQLAlchemy**](https://www.sqlalchemy.org/) 库将查询结果作为数据帧存储在 pandas 中。

本练习的目的是利用 Python 中可用的统计技术从红移数据中获得有用的见解。你可以获得的一些见解包括更好地理解你的客户的产品行为，预测流失率等。

> *我们也有一个专门的博客用于* [*使用 Python 和 R*](https://rudderstack.com/blog/how-to-access-and-query-your-bigquery-data-using-python-and-r/) *操作和查询你的 Google BigQuery 数据，如果你感兴趣的话。*
> 
> 对于这篇文章，我们假设你已经在你的红移实例中加载了数据。如果您还没有，将数据加载到 Redshift 的最佳方式是利用客户数据基础设施工具，如 RudderStack。它们允许您收集所有客户接触点的数据，并以最小的努力将它们安全地加载到 Redshift 或您选择的任何其他仓库中。

# 使用 Python 连接到您的红移数据

要使用 Python 访问您的红移数据，我们首先需要连接到我们的实例。如上所述，Redshift 与 PostgreSQL 等其他数据库解决方案兼容。因此，您可以安全地使用工具来访问和查询 PostgreSQL 数据的红移。

我们将使用`psycopg` Python 驱动程序连接到我们的 Redshift 实例。也就是说，请随意尝试您选择的任何其他库来这样做。

```
import psycopg2
con=psycopg2.connect(dbname= 'dbname', host='host', 
port= 'port', user= 'user', password= 'pwd')
```

我们强烈建议您将上述代码用作管道的一部分，并将其封装在一个处理任何错误的函数中。您需要的参数是连接到任何数据库的典型参数:

*   数据库的名称
*   主机名
*   港口
*   用户名
*   密码

# 使用 Psycopg 对红移数据执行查询

一旦建立了数据库连接，我们就可以开始查询红移数据了。我们使用 SQL 查询来缩小我们分析所需的数据量。

为了用`psycopg`做到这一点，我们执行以下步骤:

*   我们从数据库连接中获得一个光标，如下所示:

cur = con.cursor()

*   我们从要从中提取数据的表中执行查询:

cur . execute(" SELECT * FROM ` table '；")

*   一旦查询成功执行，我们就指示`psycopg`从数据库获取数据。对于进一步的数据分析，获取完整的数据集是有意义的。因此，我们运行以下命令:

cur.fetchall()

*   最后，我们关闭光标和连接，就像这样:

当前关闭()

conn.close()

这里最重要的部分是我们执行的从数据集中获取记录的 SQL 查询。您还可以使用 SQL 对大块数据进行预处理，并设置适当的数据集，使您的数据分析更加容易。

例如，您可以联接数据库中的多个表，或者使用支持红移的聚合函数根据需要创建新字段。

# 使用查询的数据进行数据分析

既然我们已经成功地查询了红移数据，并将其提取出来用于我们的分析，那么是时候使用我们所拥有的数据分析工具来处理它了。

说到 Python，最流行的数据分析库是`NumPy`和`pandas`:

*   NumPy 是一个流行的 Python 库，主要用于数值计算。
*   pandas 是 Python 中一个广泛使用的数据分析库。它提供了一种称为 DataFrame 的高性能数据结构，用于处理类似表格的结构。

无论您希望做什么样的分析，您都需要使用这两个库中的一个来表示您的初始数据。

# 将数据加载到 NumPy

将数据转换成一个`NumPy`数组非常简单。我们初始化一个新的`NumPy`数组，并将包含查询结果的光标作为参数传递。

在 Python 控制台中运行以下代码:

将 numpy 作为 np 导入

data = np.array(cur.fetchall())

# 给熊猫加载数据

你也可以用熊猫代替`NumPy`进行你的数据分析。然而，对于这一点，所涉及的步骤有点不同。

请参考以下代码片段:

```
from sqlalchemy import create_engine
import pandas as pd
engine = create_engine('postgresql://scott:tiger@hredshift_host:<port_no>/mydatabase')
data_frame = pd.read_sql('SELECT * FROM `table`;', engine
```

如上面的代码所示，我们将使用 [**SQLAlchemy**](https://www.sqlalchemy.org/) 通过连接凭证连接到我们的红移实例。然后，我们使用 [**read_sql 方法**](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.read_sql.html) 对数据库进行 sql 查询。最后，我们可以将结果直接加载到数据框架中，并用于我们的分析。

# 如何使用 R 访问和查询红移数据？

在 r 中加载和查询红移数据同样简单。我们可以使用`RPostgreSQL`包连接到我们的红移实例，然后对数据运行查询。

以下是如何在 RStudio 中安装软件包:

install.packages("RPostgreSQL ")

require("RPostgreSQL ")

下一步是连接到 Redshift 实例，如下所示:

```
drv <- dbDriver("PostgreSQL")
con <-dbConnect(drv,dbname="dbname",host="host",port=1234,
                user="user",password="password")
dbDisconnect(con)
```

> *注意:从数据库中取出数据后，关闭连接是很重要的。*

查询数据并将其加载到 R 数据框中也很容易:

今日无云。

# 加入我们的 [Slack](https://resources.rudderstack.com/join-rudderstack-slack) 与我们的团队聊天，查看我们在 [GitHub](https://github.com/rudderlabs) 上的开源报告，订阅[我们的博客](https://rudderstack.com/blog/)，在社交上关注我们: [Twitter](https://twitter.com/RudderStack) 、 [LinkedIn](https://www.linkedin.com/company/rudderlabs/) 、 [dev.to](https://dev.to/rudderstack) 、 [Medium](https://rudderstack.medium.com/) 、 [YouTube](https://www.youtube.com/channel/UCgV-B77bV_-LOmKYHw8jvBw) 。不要错过任何更新。[立即订阅](https://rudderstack.com/blog/)我们的博客！

*原载于 https://rudderstack.com*[](https://rudderstack.com/blog/access-and-query-your-amazon-redshift-data-using-python-and-r)**。**

*While we focused on Amazon Redshift in this blog, the process is also applicable to other databases like PostgreSQL. In this post, we used psycopg to connect to our Redshift instance — the same Python connector can be used to connect to a PostgreSQL instance as well.*

*One of the advantages of residing your data within a database as opposed to other formats such as CSV files is the ability to query it using SQL. You can run complex SQL queries to preprocess your data effectively and save a lot of your time and effort in building statistical models for an in-depth data analysis.*

# *Try RudderStack Today*

*Start building a smarter customer data pipeline. Use all your customer data. Answer more difficult questions. Send insights to your whole customer data stack. Sign up for [RudderStack Cloud Free](https://app.rudderlabs.com/signup?type=freetrial) today.*

*Join our [Slack](https://resources.rudderstack.com/join-rudderstack-slack) to chat with our team, check out our open source repos on [GitHub](https://github.com/rudderlabs), subscribe to [our blog](https://rudderstack.com/blog/), and follow us on social: [Twitter](https://twitter.com/RudderStack), [LinkedIn](https://www.linkedin.com/company/rudderlabs/), [dev.to](https://dev.to/rudderstack), [Medium](https://rudderstack.medium.com/), [YouTube](https://www.youtube.com/channel/UCgV-B77bV_-LOmKYHw8jvBw). Don’t miss out on any updates. [Subscribe](https://rudderstack.com/blog/) to our blogs today!*

**Originally published at* [*https://rudderstack.com*](https://rudderstack.com/blog/access-and-query-your-amazon-redshift-data-using-python-and-r)*.**