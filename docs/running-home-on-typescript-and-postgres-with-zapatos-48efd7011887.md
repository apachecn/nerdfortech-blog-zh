# 和 Zapatos 一起用打字稿和邮件回家

> 原文：<https://medium.com/nerd-for-tech/running-home-on-typescript-and-postgres-with-zapatos-48efd7011887?source=collection_archive---------5----------------------->

![](img/07cdee1cb1a5475e838a950e43a2b123.png)

马利克·斯凯兹加德在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

关于 ORM 的有效性和目的性的争论已经持续了一段时间。(对著名而可怕的类比 ORM 的总结是计算机科学中的[越南](http://blogs.tedneward.com/post/the-vietnam-of-computer-science/)可以在这里[找到](https://blog.codinghorror.com/object-relational-mapping-is-the-vietnam-of-computer-science/)。)其中最大的抱怨之一是它们提供的抽象不够。另一个是不管使用哪种方言(HQL/JPQL 等。)，最后是 SQL 与数据库对话，当问题变得复杂时，SQL 的使用变得不可避免。而且在 SQL 中微调查询总是更容易。Martin Fowler 在他的文章 [OrmHate](https://martinfowler.com/bliki/OrmHate.html) 中谈到了这两个问题，并建议更实际的解决方案是将关系模型存储在内存中，而不是试图避免它，并根据需要为 SQL 的使用提供足够的检修孔。

但是我不是来这里憎恨 ORMs 的，尽管我觉得这是开始这篇文章的明智方式。因为这是另一种选择。我目前正在做一个用 Postgres 数据库用 Typescript 编写的项目。架构师不希望承担拥有 ORM 的负担和开销，最终不得不编写 SQL，但他们仍然希望表的类型安全抽象能够改善开发人员的体验、代码效率、可维护性和可测试性。他们找到了一个很棒的库，我很喜欢使用它，它准确地回答了 ORM 有争议的缺点，同时保留了好的部分。

## 问题是

作为一名使用关系数据库的 Typescript 开发人员，我希望框架/库能够提供一些核心特性，让我更有效率。

1.  通过一组表示表的强类型接口来访问我的数据库模式表。
2.  避免为 CRUD 使用样板 SQL。
3.  在复杂场景中利用 SQL 功能的方法。

尽管这些听起来和任何 ORM 都具有的核心特性一模一样，但是这个列表缺少了一个重要的特性。关系引擎的抽象试图以面向对象的方式呈现它，最终导致对泄漏抽象的强烈反对。

Zapatos 为 Typescript 和 Postgres 开发者提供的正是这些。这不是奥姆。但是它是您作为开发人员高效工作和快速运行所需的一切！

## 走进萨巴托斯

在我们的项目中，我们有一个数据库优先的方法。所以我们已经有了模式和表。使用 Zapatos CLI，我们能够生成表示数据库中所有表的 Typescript 模式。

我将以一个有点“复杂”的表结构为例。请考虑以下情况。

上表有一个 jsonb 列和几个常见的文本列。运行 Zapatos CLI 后，我看到了类似下面的内容。

该模式由 Postgres 模式中每个表的名称空间定义组成。在名称空间中，有 4 个主要接口，开发者将与它们交互——可选的、可访问的、可插入的和可更新的。顾名思义，这为涉及给定表的所有基本操作提供了类型支持。

当然，CLI 用来生成 Zapatos 模式的配置选项允许您定制应该如何将列暴露给模式。例如，在此表中，我们可能需要将 user_name 列排除在可更新之外。这可以通过 ZapatosConfig 中的 columnOptions 轻松实现，其中所需的列可以从更新中排除。

## 如何 CRUD

基于生成的接口，Zapatos 提供了一套“快捷”方法，开发者可以在其中执行基本的 CRUD 操作。简单选择的代码如下所示。表名和列名是字符串文字，支持类型和自动完成。

这将返回一个 user_state 数组。JSONSelectable，它是从上面的 *schema.d.ts* 中的第 27 行派生的。

Zapatos 支持 select 的另外两种变体— *selectOne* 和 *selectExactlyOne* —其中两者都返回一个 user_state 对象，但是 selectOne 的类型是 user_state | undefined，如果没有找到记录或找到多条记录，selectExactlyOne 将抛出一个错误。

为插入、更新和删除提供了相同类型的快捷方式，使开发人员的生活变得非常容易。此外，Zapatos 通过快捷方式支持 count、upsert 和 truncate 操作。

这些快捷方式允许开发人员在开发时编写干净、简洁且类型安全的 SQL。它提供了与任何 ORM 相同的开发人员体验，并带有对象模型。

## 当事情变得复杂时…

但是当事情变得复杂时，对于复杂的选择查询，如何应用类型安全呢？Zapatos 有一个很好的后备功能(我喜欢这样称呼它，尽管它实际上是一个核心特性),它为开发人员提供了定义查询中使用的所需类型以及返回类型的选项。

在上面的代码块中，类型参数 *s.user_state。SQL* 和 *s.user_state。Selectable[]* 分别是使用字符串文字的 SQL 语句中使用的类型和返回类型。因为我们只从 user_state 表中查询，所以我们在 SQL 中需要的唯一类型是 user_state.SQL。第二个参数 user_state。Selectable[]表示该语句返回 user_state 的数组。运行时可选择类型。

*self* 属性允许开发人员引用键名(本例中为 state)前面的同一列，而 *param* 属性允许引用 SQL 之外的任何变量作为参数。这两个构造赋予了开发人员相当大的能力来编写复杂的 SQL 查询，同时仍然保留 SQL 语句和返回的对象中的类型安全。

## 关于自定义类型

我想强调的最后一个很酷的特性是为我们在数据库中定义的通用 jsonb 列定义定制类型的能力。重新访问 DDL 来创建 user_state 表，auth_context 列被定义为 jsonb 列，这意味着它可以保存任何 json 对象。

但是作为用 typescript 编写的开发人员，我们当然也希望对 JSON 对象的内容进行类型检查。Zapatos 用定制类型找到了答案。这是通过在 Postgres 中使用[域](https://www.postgresql.org/docs/13/sql-createdomain.html)来实现的。

首先，我们需要为所需的列定义一个域— auth_context。然后将定义为 jsonb 的列改为使用域类型，而不是 jsonb。

当模式生成再次完成时，Zapatos 用 zapatos/custom 中的域名创建一个新文件。这还会将 schema.d.ts 第 6 行中的类型更改为 AuthContextObject。最初生成的文件包含以下行。

现在我们可以在这里为 AuthContextObject 指定任何复杂类型，代替 db。JSONValue 为以前不明确的 JSON 对象提供完整的类型支持。

## 交易支持

Zapatos 让开发人员能够控制如何通过事务函数管理事务，这实质上是让开发人员在一个 BEGIN/COMMIT 块中运行一组给定的 Zapatos 操作。它还允许开发人员为事务定义一个期望的[隔离级别](https://www.postgresql.org/docs/13/transaction-iso.html)。

## 记录/调试

Zapatos 提供了一些运行时特性，开发人员可以从中提取构建的 SQL 查询、返回的结果和事务细节(比如重试)。这可用于调试和查询优化任务。

这是一次性的运行时配置，可以添加到 index.ts 中

## ORMs 缺少什么

正如我所说，萨巴托斯不是一个 ORM。因此，如果你要一对一地比较传统的 ORM 和 Zapatos，它在功能方面有一些缺点。既然我已经与 ORM 进行了比较，我将尽量做到公平，并记录一些 Zapatos 不支持的重要内容。

ORM 通常是数据库不可知的。如果我们采用代码优先的方法，它具有“*一次编写，随处部署*”的特性。但是 Zapatos 只和 Postgres 合作。当涉及到事务管理(隔离级别)时，它具有 Postgres 特有的特性，并且不支持任何其他数据库。

它也没有代码优先的方法。

因为 ORM 在关系模型之外创建了自己的对象模型，例如，如果我们查询一个带有外键引用的对象，ORM 能够将被引用的对象加载(惰性的或急切的)到内存中，并且开发人员能够使用点操作符访问该对象的属性。我们没有萨巴托斯那样的奢侈。如果我们需要访问被引用的表属性，就需要一个 join 或另一个 read 语句。

## 结尾注释

到目前为止，使用该库的体验相当顺利。需要注意的一点是，在生成 Zapatos 模式文件时，它不支持模式级别的区分。因此，如果您的 Postgres 数据库有多个模式，并且每个模式中都有重复的表名，那么您将会遇到一些问题。如果能通过快捷方式方法支持内部连接就更好了。目前它只支持横向连接。

总之，Zapatos 让使用 Typescript 和 Postgres 的开发人员的生活变得非常简单。虽然 Zapatos 没有用成熟的 ORM 来加重我们的负担，但是它提供了非常需要的类型和事务支持，以及发布 CRUD 语句的便利性，这基本上是使用 ORM 时的基本期望。

跑步快乐！

P.S. Zapatos 有一个相当广泛的文档，里面有很多例子[这里](https://jawj.github.io/zapatos/)，它将回答任何如何操作的问题。