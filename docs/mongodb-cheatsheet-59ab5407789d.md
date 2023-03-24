# MongoDB Cheatsheet

> 原文：<https://medium.com/nerd-for-tech/mongodb-cheatsheet-59ab5407789d?source=collection_archive---------2----------------------->

如果您已经熟悉了数据库管理系统，并且想快速入门 MongoDB 而不需要学习基础知识，那么这本书非常适合您。

![](img/e80f5f51e7c76fde4c56d2b83c485823.png)

## 什么是 MongoDB？

如果你以前使用过关系数据库，如 MSSQL 或 MySQL 你必须熟悉所有那些行、列和关系。然而，MongoDB 是一个非关系数据库。使用 MongoDB，您可以自由地工作，而不必担心结构。

由于它的可扩展性很强，开发速度也很快，因此它是物联网和大数据等许多工作领域的首选。

## 入门指南

要开始使用 MongoDB，只需访问[此链接](https://www.mongodb.com/)来安装和设置 MongoDB 社区服务器和 Mongosh shell。

现在打开一个终端，通过输入命令“mongo”和“mongosh”来检查安装是否完成。如果第一个命令失败，只需转到安装文件夹所在的位置，将路径复制到“mongod”应用程序中，并将其粘贴到环境变量中的 path 中。如果每个命令都返回除 command not found 之外的内容，那么就可以开始了。

## 通过 Shell 使用 MongoDB

在你的终端上运行“mongosh”首先打开 Mongo 终端。然后，您可以根据需要使用下面非常基本的命令:

*   **显示数据库:**显示您机器中现有的 MongoDB 数据库。
*   **使用 dbName:** 用于切换到名为 dbName 的数据库上运行命令。
*   **显示集合:**用于显示数据库中的集合(相当于表格)。
*   **db.dropDatabase():** 删除运行该命令的数据库。
*   **db . create collection(" dbName "):**创建一个名为 dbName 的集合。
*   **cls:** 清除端子。
*   **退出:**退出 Mongo shell。
*   **db:** 显示当前工作 db。
*   **db . dbname . insert one({…Json data here…}):**将一个 Json 数据插入到 db 中。
*   **db.dbName.find():** 列出该数据库中的所有数据。如果你把 Json 放在 find 方法中，它会搜索并带来包含 Json 对象的对象。
*   **db . dbname . insert many([{…Json here…}，{…Json here …}]):** 就像 insertOne 一样，但是用来插入多个对象。
*   **db . dbname . count documents({ param1:40 }):**得出 param 1 字段值为 40 的所有对象的计数。
*   **db . dbname . update one({ param1:10 }，{ $set { param1: 1}}):** 查找 param 1 字段等于 10 的第一个对象，并将其值更改为 30。
*   **db . dbname . update one({ _ id:10 }，{ $inc{ param1: 1}}):** 查找 id 为 10 的第一个对象，并将 1 添加到 param1 字段。
*   **db . dbname . update one({ _ id:10 }，{ $rename{ param1: "age"}}):** 查找第一个 id 为 10 的对象，并将列名 param1 更改为" age "。
*   **db . dbname . update one({ _ id:10 }，{ $unset: { age: "" }}):** 查找第一个 id 为 10 的对象，并删除年龄字段。
*   **db . dbname . update one({ _ id:10 }，{ $push: { param1: "data1"}}):** 查找第一个 id 为 10 的对象，然后查找由数组组成的名为 param1 的字段，最后将“data1”推入该数组内部。
*   **db . dbname . update one({ _ id:10 }，{ $pull: { param1: "data1"}}):** 查找第一个 id 为 10 的对象，然后查找由数组组成的名为 param1 的字段，最后从该数组中删除“data1”。
*   **db . dbname . replace one({ param1:10 }，{… Json here…}):** 查找 param 1 等于 10 的对象，用输入的 Json 对象替换整个对象。
*   **db . dbname . delete one({ param1:10 }):**查找 param 1 等于 10 的第一个对象并删除它。
*   **db . dbname . delete many({ param1:10 }):**查找 param 1 等于 10 的所有对象并删除它们。

存储在 MongoDB 中的每个对象都被称为一个**文档**。对于文档，您不需要太担心结构，甚至可以在插入 Json 时使用嵌套对象。

我们使用 find()方法的变体对数据进行排序。以下是一些命令示例:

*   **db.dbName.find()。limit(2):** 只从数据库中返回两个对象。
*   **db.dbName()。查找()。sort({ name:x }):**x 可以是 1 或-1，如果 name 属性是一个字符串，那么 1 将按字母顺序返回数据，而-1 将按相反的字母顺序返回数据。如果 name 参数是一个数字或日期，那么应该是降序/升序。您也可以按多个参数排序。
*   **db.dbName.find()。skip(1):** 跳过第一个条目并返回其余的条目。
*   **db.dbName.find({param1: 1，param2:1，param3:0}):** 将按字母/升序显示 param1 和 param2 字段，但不会显示 param3 字段。
*   **db . dbname . find({ param1:{ $ eq:" Hello world " } }):**得出 param 1 字段等于" Hello world "的结果。
*   **db . dbname . find({ param1:{ $ neq:" Hello world " } }):**得出 param 1 字段不等于" Hello world "的结果。
*   **db . dbname . find({ param1:{ $ gt:2 } }):**得出 param 1 字段大于 2 的结果。
*   **db . dbname . find({ param1:{ $ GTE:2 } }):**得出 param 1 字段大于或等于 2 的结果。
*   **db . dbname . find({ param1:{ $ LTE:2 } }):**得出 param 1 字段小于或等于 2 的结果。
*   **db . dbname . find({ param1:{ $ in:[10，20]}):**得出 param 1 字段等于 10 或 20 的结果。
*   **db . dbname . find({ param1:{ $ nin:[10，20]}}):** 得出 param 1 字段不等于 10 或 20 的所有结果。
*   **db . dbname . find({ param1:{ $ exists:true } }):**获取 param 1 字段存在的所有结果。
*   **db . dbname . find({ param1:{ $ exists:false } }):**显示 param 1 字段不存在的所有结果。
*   **db . dbname . find({ $ or { { param1:{ $ gt:2 } }，{ param2:" Hello world " } }):**得出 param 1 字段大于 2 或 param 2 字段等于" Hello world "的结果。
*   **db . dbname . find({ $ expr { $ gt:[" $ param1 "，" $ param2 "]}):**得出 param 1 字段大于 param 2 字段的结果。