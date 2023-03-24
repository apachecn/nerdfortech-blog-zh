# Django 中的自定义查找

> 原文：<https://medium.com/nerd-for-tech/custom-lookups-in-django-69fd13e35bdb?source=collection_archive---------5----------------------->

在我们检查自定义查找之前，让我们先看看基础知识，了解什么是查找

## 那么什么是查找呢…

根据 Django 文件

> 字段查找是指定 SQL `**WHERE**`子句内容的方式。它们被指定为`**QuerySet**`方法`[**filter()**](https://docs.djangoproject.com/en/3.1/ref/models/querysets/#django.db.models.query.QuerySet.filter)`、`[**exclude()**](https://docs.djangoproject.com/en/3.1/ref/models/querysets/#django.db.models.query.QuerySet.exclude)`和`[**get()**](https://docs.djangoproject.com/en/3.1/ref/models/querysets/#django.db.models.query.QuerySet.get)`的关键字参数。

不是很清楚，但一个例子会使它变得容易得多

```
Entry.objects.filter(id__in=[1, 3, 4])
```

在上面的例子中，‘中的**’就是所谓的查找，上面的查询所做的是过滤所有的**条目**数据，其中它们的**id**属于列表**【1，3，4】**中的任意值**

现在，我希望您对如何以及在哪里使用查找有所了解

Django 有许多内置的查找，如果没有指定，那么默认为 exact:

```
Entry.objects.get(id=14)
```

与相同

```
Entry.objects.get(id__exact=14)
```

内置查找的几个例子是[`in`、`iexact,`、`contains`、`gt`、`gte`、T10、`startswith`…。]

您可以查看其余内容的链接:[https://docs . django project . com/en/3.1/ref/models/query sets/# field-lookups](https://docs.djangoproject.com/en/3.1/ref/models/querysets/#field-lookups)

![](img/00683adc584b5c7bdfe9d6f068571b78.png)

# 让我们跳到并解决🐘在……里🏠— — — —习俗👀👆🏻

哈哈抱歉，我被表情符号冲昏了头脑，

# 那么我们需要定制查找是为了什么呢？

为什么，答案很简单，因为根据你的复杂需求，可能没有可以获取数据的查找

让我们来看一个来自 Djangodocs 的非常简单的例子，它会给你足够的信息来开始定制查找

我们将编写一个与`**exact**`相反的自定义查找`**ne**`。`**Author.objects.filter(name__ne='Jack')**`会翻译成 SQL:

```
"author"."name" <> 'Jack'
```

这个 SQL 是后端独立的，所以我们不需要担心不同的数据库。

要做到这一点，有两个步骤。首先我们需要实现查找，然后我们需要告诉 Django:

```
**from** **django.db.models** **import** Lookup**class** **NotEqual**(Lookup):
    lookup_name = 'ne' **def** as_sql(self, compiler, connection):
        lhs, lhs_params = self.process_lhs(compiler, connection)
        rhs, rhs_params = self.process_rhs(compiler, connection)
        params = lhs_params + rhs_params
        **return** '**%s** <> **%s**' % (lhs, rhs), params
```

为了注册`**NotEqual**`查询，我们需要在我们希望查询可用的字段类上调用`**register_lookup**`。在这种情况下，查找对所有的`**Field**`子类都有意义，所以我们直接用`**Field**`注册它:

```
**from** **django.db.models** **import** Field
Field.register_lookup(NotEqual)
```

查找注册也可以使用装饰模式来完成:

```
**from** **django.db.models** **import** Field@Field.register_lookup
**class** **NotEqualLookup**(Lookup):
```

查看更多信息:[https://docs.djangoproject.com/en/3.1/howto/custom-lookups/](https://docs.djangoproject.com/en/3.1/howto/custom-lookups/)

所有人都到此为止了，如果你一直读到这里，那么…..

![](img/dcf21bddde5fc157c9aac93e9c15ed20.png)