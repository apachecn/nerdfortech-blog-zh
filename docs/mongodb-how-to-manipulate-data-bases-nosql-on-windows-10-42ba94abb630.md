# MongoDB:如何在 Windows 10 上操作 NoSQL 数据库

> 原文：<https://medium.com/nerd-for-tech/mongodb-how-to-manipulate-data-bases-nosql-on-windows-10-42ba94abb630?source=collection_archive---------11----------------------->

![](img/5075e739caa9e388ba1c8927543a47d0.png)

要下载 MongoDB，请点击[此处](https://www.mongodb.com/try/download/community)

第一步是在我们的 Windows 命令提示符下输入，然后我们在 MongoDB 的根文件夹下输入，在 **bin** filder 下输入，然后我们在提示符下复制路径并更改目录:

> cd C:\Program Files\MongoDB\bin

下一步是通过键入以下命令进入 MongoDb 的 shell:

> 蒙戈

输出应该是这样的:

![](img/e85ddc87ba577049de1ebf62afbb9168.png)

产出 1

一旦我们做到这一点，我们可以开始管理 ou 数据库。

**创建新用户:**

要创建新用户，只需在命令提示符下键入以下代码:

![](img/74c5414859ab12cc1a9b21b591dbfd31.png)

输出应该是这样的:

![](img/43bc5481ccbc7fac154c8ade966a1352.png)

**显示数据库列表:**

只需在提示符下键入以下代码:

> 显示数据库

输出应该是这样的:

![](img/9995f494fe2dd117e101bad9f48c4b76.png)

**创建数据库**

要创建数据库，只需键入

> 使用 loja

(“loja”是数据库的名称)

![](img/ca1748070f8d95f897977b5f4d5ec5ae.png)

**创建收藏**

集合允许用户保存文档，与关系数据库中的表相同。

要创建集合，只需键入以下代码:

> db . create collection**(**' clientes’**)**；

和 verofy 是集合战争创建我们使用以下代码:

> 显示收藏

如下面的输出所示:

![](img/3b0cf7a74ca1e3eff7aab3c40dac97e8.png)

**在集合中插入文档**

现在我们要在集合“clientes”上插入文档，只需键入以下命令(这是一个示例):

> db . clientes . insert({ nome:" joo "，sobr enome:" antónio " })；

为了显示插入到确定集合中的文档，我们使用以下代码:

> db . clientes . find()；

输出应该是这样的:

![](img/94c4b80ca8c7ab34a599f38f7b960091.png)

**一次添加多个文档**

只需键入以下代码(以名称为例):

> db . clientes . insert([{ nome:" Pedro "，sobrenome:"Antunes"}，{nome:"Antonio "，sobrenome:"Santos "，genero:" Masculino " }])；

显示文档

有了这个代码，吴就可以以一种相关的方式查看我们的文档:

> db.clientes.find()。漂亮()

![](img/7f746c30599e218157d1717c5565fe7b.png)

现在，我们将通过键入以下代码(示例)在我们的文档中添加一个新字段:

![](img/5c7f6f98dc0ea20cce81cbdec1c0dae9.png)

**删除文件**

使用下面的代码，我们将删除每个名为“Pedro”的文档:

> db . clientes . remove({ nome:" Pedro " })；

搜索文档

因此，让我们只使用一个参数来搜索文档，以免键入以下代码:

> db . clientes . find({ nome:" Jose " })；

**结果排序**

要对我们的搜索进行排序，我们可以使用以下命令:

清点文件

键入以下代码:

> db.clientes.find()。count()；

![](img/b46741a90dce6e226be0fcdb4452680c.png)

我们还可以统计给定查询的结果:

![](img/05d7fa1399e0061e9918f09563041204.png)