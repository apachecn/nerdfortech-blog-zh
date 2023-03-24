# 面向数据库人员的 ElasticSearch (ELK)

> 原文：<https://medium.com/nerd-for-tech/elasticsearch-elk-for-database-people-e28ce5e77610?source=collection_archive---------3----------------------->

ElasticSearch 是 google 针对企业的搜索。它是一个“数据库”，被拥有面向客户的应用程序的企业广泛使用，这些应用程序需要较短的响应时间来搜索*结构化*企业数据和*文本*数据。

**其他数据库没有解决的问题** SQL 和 NoSQL 数据库都需要特殊的设计考虑，以允许对它们保存的数据进行快速搜索。所有这些归结为两个解决方案:索引和分区。索引支持*大海捞针*类型的搜索。分区使*能够存储*以缩小搜索范围。许多产品允许以有趣的方式结合这两种技术。

Amazon Redshift 实现了一种叫做 Zone Map 的东西，在实现上有点类似于索引，但在逻辑上更接近于分区修剪。当请求搜索时，区域映射跳过所请求的记录不存在的数据库记录块。Redshift 通过存储按区域映射列排序的记录来做到这一点，这使它能够跟踪每个数据库块上该列的上限和下限，然后可以用于跳过。

**数据库搜索—缺点和解决方法** 这些技术中的每一种，索引和分区，都有自己基于数据库产品的衍生物，但是所有这些都指向一个限制:*必须预先知道哪些列有更高的机会被搜索*。在 SQL 和 NoSQL 系统中，数据库都是根据这些知识精心设计的。所以新的搜索标准需要设计上的改变，这有时是不可行的。

一些常用的解决方法可以缓解这种情况。一种方法是根据搜索列的需要创建尽可能多的索引。另一种类似的方法是根据需要创建尽可能多的表来进行窄搜索。第三种也是 web 应用中最主要的方法是将数据缓存到离应用服务器更近的地方(比如 Coherence、内存扩展等产品)。).

所有这些解决方法的缺点是需要保持额外的数据结构同步，这会影响每个数据库写操作的响应时间。

**elastic search** elastic search 是一款无时无刻索引所有领域的产品。没有表或约束，只有索引。

如果你有一个应用程序需要搜索任意数量的字段，有大量的数据，甚至可能包含文本数据，ElasticSearch 提供了一个很好的解决方案。

然而，弹性搜索就是这样——一种弹性的搜索方式。我们期望从数据库中得到的与事务一致性或数据完整性相关的所有其他东西都不存在。通常提供搜索的 web 应用程序有两个数据库系统，一个(或多个)用于应用程序的数据，另一个是 ElasticSearch 数据库。一个 Logstash 过程保持 ElasticSearch 中的数据与主数据库同步。

一个简单的搜索示例是，当用户键入城市名称的前几个字母时，UI 会自动填充以这些字符开头的潜在城市列表。该应用程序使用 ElasticSearch 来完成这项工作。所有其他数据请求都发送到主数据库。

**麋栈**
麋栈由

*   ElasticSearch 服务器将数据记录(文档)保存在索引中，以便应用程序可以通过 http 请求进行搜索(非 HTTP 请求也可以通过 ElasticSearch 开发的专有二进制协议来支持)
*   LogStash 是一个用于 es 的 ETL 接口，它从 MySQL 等外部数据源获取数据，并在 ES 上执行 CRUD 操作
*   Kibana 是一个可视化工具，让您可以在 ES 上查询和执行手动或临时的 CRUD 操作。

AWS ElasticSearch(现在的 OpenSearch)自带 es 和 Kibana。您必须在另一个服务器(EC2 或其他地方)上单独安装 LogStash，或者使用 AWS Lambda 进行自动加载。

**示例 ES 查询运行于 Kibana**
GET _ search
{
" query ":{
" match ":{
" first _ name ":" Tom "
}
}
}

**示例 log stash config**
**input**{
JDBC {
JDBC _ connection _ string =>" JDBC:MySQL://1189-webservr-Drupal-01 . mince . com:3306/rsys？use SSL = false "
JDBC _ user =>" test "
JDBC _ password =>" E！test @ "
JDBC _ driver _ library =>"/usr/share/log stash/driver/MySQL-connector-Java-5 . 1 . 42-bin . jar "
JDBC _ driver _ class =>" com . MySQL . JDBC . driver "
schedule =>* * * * * * "
statement =>" SELECT * FROM city WHERE updated _ at>:SQL _ last _ value "
tracking _ at

**输出** {

if[type]= = " city " {
elastic search {
user =>" estest "
password =>" test %&es "
hosts =>" elastic search:9200 "
index =>" cities "
document _ type =>" document "
document _ id =>" " { rx _ ipid }。

*输入*配置指定从哪里读取，读取什么记录等。*输出*配置有输出设置，即写入 ES。这个配置文件可以在 ES install 的主文件夹中用任何文件名(例如:config.txt)创建，例如/etc/logstash/bin/config.txt。它通常是 ElasticSearch 中“代码”的内容，是应用程序代码管道的一部分。