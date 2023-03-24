# 如何创建一个时间表来跟踪你的成就

> 原文：<https://medium.com/nerd-for-tech/how-to-create-a-timeline-to-track-your-achievements-94762f93f599?source=collection_archive---------1----------------------->

![](img/263eade6b72338c162af7e6260572448.png)

# 介绍

最近，我偶然看到一条由 Florin Pop 发布的推文，它谈到了使用时间表来跟踪你的次要和主要成就。这是一个有趣的想法..我怎么从来没想过这个。因此，受到启发，我开始创作自己的版本。在这篇文章中，你会看到我从灵感到创作的历程。

# 技术堆栈

我最后想要的非常清楚，只有我需要使用的技术需要被弄清楚。我选择 Django 作为后端..因为为什么不呢..它提供了易于管理的模型-视图-控制架构，开箱即用的管理面板和前端将是简单的 Html 和 CSS，我不是一个幻想的人..简单对我来说很好。

# 开始

我首先创建了一个简单的 Django 项目，然后创建了一个 timeline 应用程序。我计划添加更多的东西，所以创建一个单独的应用程序是一个好主意。此外，我添加了*卡片*模型，简单地保留**标题**、**正文**、**日期**作为字段，如果任何其他东西需要的话，比如**标签**..(我计划稍后添加)可以轻松添加。

```
class Card(models.Model):
 “””
 In timeline: Each achievement is shown in card
 “””
    heading = models.CharField(max_length=55, default=”I forgot to add heading..”, blank=False, null=False)
    body = models.TextField(max_length=999, default=””, null=True, blank=True)
    date = models.DateField(default=timezone.now, null=False, blank=False)
    def __str__(self) -> str:
        return f”{self.heading}.. on {self.date}”
```

一旦模型完成..是时候创建一些视图来处理一些幕后的东西了。我想要一个时间表，其中所有的卡片(即成就)都按年份分开..因此我需要处理这件事。

对于视图，我使用了基于类的通用视图`django.views.generic.View`,并定义了自己的请求处理方法..因为我想我将来可能会需要定制的东西。

```
class CardView(View):
    template_name = 'timeline/card_list_view.html'
    def get(self, request):
        cards = Card.objects.all().order_by('-date')
        return render(request, template_name=self.template_name, context={'timeline_data':cards})
```

对于我的第一种方法，我只是尝试将所有的卡片都呈现在前面..我很快意识到这是行不通的。因为在 Django 我没有办法(至少我找不到)以这样一种方式查询数据库，可以给我以年份分隔的卡片。因此，我添加了自己的逻辑来处理这个问题。

```
class CardView(View):
    template_name = 'timeline/card_list_view.html'
    def get(self, request):
        cards = Card.objects.all().order_by('-date').values() qs = defaultdict(list)
        for query in cards:
            card_year = query['date'].year
            qs[card_year] += [query]
        qs = dict(qs) return render(request, template_name=self.template_name, context={'timeline_data':qs})
```

我用了`defaultdict`，非常有用的工具..给我省了一些`if-else`和`try/except`。如果你想知道我在这里做什么——首先我按照从最新到最老的顺序得到所有的卡片，然后`.values()`把它转换成字典列表。然后我创建了一个 defaultdict 对象..其中每一项都有`list`作为默认值，并在一个键-值对中添加了同年的所有卡片。好了..这就是后端代码的深度。

向前看，我有点懒得创建自己的前端，因此我四处看看，发现了一个很棒的时间线模板，正如我想要的那样——简单而强大(查看[这里](https://codepen.io/htmlcodex/details/KKVpdNK))。

有一个问题..它使用不同的 css 类来替换奇数卡左边的*和偶数卡右边的*。所以，我需要一种方法来使用它**

1.  Django 模板中的计数器，以及
2.  找出它们是偶数还是奇数的方法..因为我们不能在那里使用简单的%2。

因此，我环顾四周，发现了两件意想不到的事情..一起解决我的问题。首先是`forloop.counter`，顾名思义..这是 django 中`for`循环的计数器..第二个`divisibleby`过滤器(字面上 Django 拥有一切)。最后，我的前端看起来像这样..(我修剪了不相关的代码)

```
<div class="timeline-continue">
    {% for year, card_list in timeline_data.items %}
        {% for card  in card_list %}
            {% if forloop.counter|divisibleby:2 %}
                {% include 'timeline/snippets/right_card.html' with heading=card.heading date=card.date body=card.body %}
            {% else %} 
                {% include 'timeline/snippets/left_card.html'  with heading=card.heading date=card.date body=card.body %}
            {% endif %}
        {% endfor %}
        {% include 'timeline/snippets/year_circle.html' with year=year %}                    
    {% endfor %}
</div>
```

是的，我用片段来遵循干燥的原则..左右两边的卡片片段看起来像这样。左卡

```
<div class="row timeline-right">
    <div class="col-md-6">
        <p class="timeline-date">
            {{date|date:"F  "}}
        </p>
    </div>
    <div class="col-md-6">
        <div class="timeline-box">
            <div class="timeline-icon">
                <i class="fa fa-gift"></i>
            </div>
            <div class="timeline-text">
                <center>
                <h3>{{heading|title }}</h3>
                <p>{{body}}</p>
                </center>
            </div>
        </div>
    </div>
</div>
```

右卡

```
<div class="row timeline-left">
    <div class="col-md-6 d-md-none d-block">
      <p class="timeline-date">
        {{date|date:"F"}}
      </p>
    </div>
    <div class="col-md-6">
      <div class="timeline-box">
        <div class="timeline-icon d-md-none d-block">
          <i class="fa fa-business-time"></i>
        </div>
        <div class="timeline-text">
          <center>
          <h3>{{heading|title }}</h3>
          <p>{{body}}</p>
          </center>
        </div>
        <div class="timeline-icon d-md-block d-none">
          <i class="fa fa-business-time"></i>
        </div>
      </div>
    </div>
    <div class="col-md-6 d-md-block d-none">
      <p class="timeline-date">
        {{date|date:"F"}}
      </p>
    </div>
  </div>
```

老实说，就是它。这只是在数据库中创建一些卡片来检查最终产品的问题。我相信结果很好。你可以看到这个最终产品[在这里](https://amangoyal.ml/timeline/)

你可以在这里看完整的代码库[。](https://amangoyal.ml/timeline/)

[](https://github.com/goyal-aman/portfolio/) [## goyal-aman/作品集

### 包含我即将推出的投资组合的源代码。创建账户，为 goyal-aman/产品组合的发展做出贡献…

github.com](https://github.com/goyal-aman/portfolio/) 

*最初发布于*[*https://amango yal . hash node . dev*](https://amangoyal.hashnode.dev/how-to-create-a-timeline-to-track-your-achievements)*。*

鸣谢:文章封面照片由[弗洛里安·克劳尔](https://unsplash.com/@florianklauer?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/cover-photo?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄