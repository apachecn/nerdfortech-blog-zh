# 通过减少测试中的噪音获得更好的产品代码

> 原文：<https://medium.com/nerd-for-tech/getting-better-production-code-by-reducing-noise-in-tests-88e65f82ad4f?source=collection_archive---------11----------------------->

![](img/75426c2592bd643c4c54bfc551236cd0.png)

有时很难看到测试树后面的产品代码。照片来自个人收藏

您是否注意到，有时您需要编写大量的测试代码，只是为了验证生产逻辑的某些非常具体的部分？

当我自己遇到这种情况时，我的第一反应是隐藏所有这些无趣的细节。例如，通过在测试代码中引入工厂方法或构建器。通常这确实已经足够好了。然而，在以这种方式清理测试之前，最好考虑一下是否不是产品代码需要清理。

> 关注因子低的测试是设计糟糕的产品代码的标志。

这里的焦点因素是对测试行为真正重要的代码和所有测试代码之间的比率。

# 示例—初始迭代

不久前，我们的团队需要优化从关系数据库中删除过期报价的流程。为了解决索引碎片问题，我们决定按月对数据进行分区，并用过期数据截断整个分区(而不是逐个删除记录)。

我们最初的测试(用 Groovy 和[斯波克](http://spockframework.org/)编写)看起来像这样:

```
class DeleteExpiredQuotesSpec extends Specification { QuotesRepository quotes = new QuotesJdbcRepository(...) def "deletes quotes that were created 1 month before the previous month"() {
    given:
    def quote1 = aQuote().id(id1).createdAt("2008-01-01T11:11:11Z"). build()
    def quote2 = aQuote().id(id2).createdAt("2008-02-31T11:11:11Z"). build()
    def quote3 = aQuote().id(id3).createdAt("2008-03-02T11:11:11Z"). build() quotes.add(quote1)
    quotes.add(quote2)
    quotes.add(quote3) when:    
    def now = Instant.parse("2008-03-28T13:11:11Z")
    quotes.deleteExpired(now) then:
    !quotes.contains(quote1.id)
    quotes.contains(quote2.id)
    quotes.contains(quote3.id)
  }}
```

该测试验证两件事:

*   我们能删除数据吗？
*   我们删除了正确的数据吗？

当当前月份是二月，我们需要删除前一年 12 月创建的报价时，我们的逻辑还能工作吗？没问题——我们可以添加另一个测试。

可以进行测试来验证我们的存储库是否删除了报价。然而，现在我们有两个测试贯穿整个过程，这样我们就可以验证一个特定的细节——找到要删除的正确分区。

如果我们只想验证是否找到了正确的分区，那么这些测试中有很多东西并不是很有趣。下面是同样的测试，其中重要代码用粗体标记:

```
given:
def quote1 = **aQuote**().id(id1).**createdAt**("2008-**01**-01T11:11:11Z"). build()
def quote2 = **aQuote**().id(id2).**createdAt**("2008-**02**-31T11:11:11Z"). build()
def quote3 = **aQuote**().id(id3).**createdAt**("2008-**03**-02T11:11:11Z"). build()**quotes**.**add**(**quote1**)
**quotes**.**add**(**quote2**)
**quotes**.**add**(**quote3**)when:    
def **now** = Instant.parse("2008-**03**-28T13:11:11Z")
**quotes**.**deleteExpired**(**now**)then:
**!quotes.contains(quote1**.id**)
quotes.contains(quote2**.id**)
quotes.contains(quote3**.id**)**
```

# 降低测试中的噪音

让我们通过隐藏不重要的东西来简化这个测试:

```
given:
def quoteInJan = **addQuoteCreatedIn**(**JANUARY**)
def quoteInFeb = **addQuoteCreatedIn**(**FEBRUARY**)
def quoteInMarch = **addQuoteCreatedIn**(**MARCH**)**quotes**.**deleteExpired**(someTimeIn(**MARCH**))expect:
**!quotes.contains(quoteInJan)
quotes.contains(quoteInFeb)
quotes.contains(quoteInMarch)**
```

现在更容易看到创建时间的月份部分。然而，如果我们停在这里，我们只是应用了一些化妆品。即使测试现在变得更好了，我们仍然忽略了它试图告诉我们的东西:“产品代码中缺少一些东西”！

# 改进生产代码

测试仍然验证我们在开始时确定的两件事。这是因为在生产代码中没有专门的概念来负责选择正确的分区。

再来补充一下`ExpiredQuotesCleanupMonth`。现在，我们可以编写更加集中的测试，只验证月份/分区的发现:

```
def "expired quotes cleanup month is 1 month before the previous month"() { 
  expect:
new **ExpiredQuotesCleanupMonth**(someTimeIn(**MARCH**))
    .value() == **JANUARY** }
```

这样好多了。现在我们可以重构我们的存储库，使用`ExpiredQuotesCleanupMonth`作为参数，而不是`Instant`。我们可以将策略(需要截断什么分区)和实现(如何截断分区)分开。

然而，测试中仍然有一种方法可以隐藏东西— `someTimeIn`。让我们利用这个机会让我们的产品代码更有表现力。

我们可以引入`CurrentMonth`类。它封装了从当前时间中获取月份的一点点逻辑，并不复杂。但是，它增加了一个值，现在只需查看`ExpiredQuotesCleanupMonth`构造函数就可以清楚地知道预期的参数是什么——它不仅仅是一个时间瞬间(其中只有月份部分重要)或任何月份，而是当前月份:

```
new ExpiredQuotesCleanupMonth(new CurrentMonth(MARCH))
```

# 结果

我们已经使用了测试来移除通用目的类型的使用，取而代之的是引入我们自己的特定领域包装器(`CurrentMonth`)。我们还将数据库更新机制(存储库实现)从指定删除标准的策略(`ExpiredQuotesCleanupMonth`)中分离出来。

下面是最终的存储库测试:

```
given:
def quoteInJan = **addQuoteCreatedIn**(**JANUARY**)
def quoteInFeb = **addQuoteCreatedIn**(**FEBRUARY**)
def quoteInMarch = **addQuoteCreatedIn**(**MARCH**)**quotes**.**deleteBy**(
  new **ExpiredQuotesCleanupMonth**(new **CurrentMonth**(**MARCH**)))expect:
**!quotes.contains(quoteInJan)
quotes.contains(quoteInFeb)
quotes.contains(quoteInMarch)**
```