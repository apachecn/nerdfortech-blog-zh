# SAP 教程:完整的 CAP Java 第 6 部分

> 原文：<https://medium.com/nerd-for-tech/sap-tutorial-complete-cap-java-part-6-81e9e867bc60?source=collection_archive---------10----------------------->

实施评级字段

![](img/09048bacf746d9ceffd1a0cf19517a28.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的 [ThisisEngineering RAEng](https://unsplash.com/@thisisengineering?utm_source=medium&utm_medium=referral)

## 内容

*   [上一集:使用 CD 和费奥里元素中的单位](https://bnheise.medium.com/sap-tutorial-complete-cap-java-part-5-fb3ff81e64c1)
*   **当前:实施评级字段**
*   [接下来:CDS 数据模型中的类型、方面和关联](https://bnheise.medium.com/sap-tutorial-complete-cap-java-part-7-eba7169b38fd)

欢迎来到完整的 CAP Java 教程系列的第 6 部分，在这里我将一步一步地向您展示如何构建 SAP 的示例[书店](https://github.com/SAP-samples/cloud-cap-samples-java)应用程序。到目前为止，我们一直致力于建立应用程序浏览书籍页面。让我们快速看一下到目前为止我们已经构建了什么:

![](img/91f77176988d81abacac4ad74cc15c16.png)

我们的书店

这是我们的目标，我们今天的目标在红框中:

![](img/8ce5bb72b4d7e224e4880af987fc8d8a.png)

SAP 书店

因为我们的图书模型还不包含评级字段，所以让我们先来看看。

# 步骤 1:将评级字段添加到模式中

让我们仔细看看评级:

![](img/06af13465bfde634da70e053e88afc21.png)

我们可以看到评级显示为 5 颗星，因此我们似乎可以将其建模为 0.0-5.0 的整数范围。然而，我们也可以从第一行看到一个分数星，所以小数可能是一个更好的方法。让我们将它添加到 *db/Books.cds.* 中的 CDS 模型中

```
rating   : Decimal(2, 1);
```

![](img/88a94c2c942bae5dc94623292a31eae7.png)

我们为评级字段指定了小数类型，小数位数为 2，精度为 1，这意味着它将接受-9.9 到 9.9 范围内的数字。它接近我们想要的，但不精确。让我们添加一些验证，以确保我们只能获得从 0 到 5 的数字。为此，我们将使用@assert.range 注释。

```
rating   : Decimal(2, 1)[@assert](http://twitter.com/assert).range : [ 0.0, 5.0 ];
```

CAP 为验证提供了其他几种方便的注释。你可以在这里阅读更多关于他们的信息[。另外，您可以尝试为我们的其他一些字段实现一些验证。](https://cap.cloud.sap/docs/guides/providing-services#input-validation)

无论如何，让我们将这个字段添加到我们的模拟数据中*db/data/toad lop . bookshop-books . csv .*在 CSV 中添加一个名为 *RATING 的列；*并提供一些值。

您可能想要测试我们的输入验证，所以让我们来尝试一下。你可能会想到的一种测试方法是向种子数据添加一个超出范围的值，然后启动应用程序，但如果你这样做了，一切都会正常启动。原因是 *cds - deploy* 命令在后台运行，将数据发送到数据库，绕过了正常的输入检查。为了正确地测试它，我们必须使用我们在[第 1 部分](/nerd-for-tech/sap-tutorial-complete-cap-java-part-1-fc1868c7bbba)中创建的 *rest-test.http* 文件，并尝试实际发布不正确的数据。

如果您还记得，我们在那里有以下请求:

![](img/dcc21b9ff671453acba067e49a5438c1.png)

我们已经更新了我们的 oData 服务，所以我们需要修复 URL。特别是用 *api* 替换 *odata/v4* ，用 browse 替换 *CatalogService* 。此外，我们需要添加一些身份验证，所以将 admin:admin@放在 *localhost* 之前。结果应该如下所示:

```
POST http://admin:admin@localhost:8080/api/browse/Books HTTP/1.1
```

最后，添加一个大于 5 或小于 0 的评分字段，并尝试单击“发送请求”。确保不要发送字符串，否则它会因为类型错误而失败，而不是因为超出范围。

![](img/0e6a59f6d1cdeb8e046367db61c95cbe.png)

非常好。现在把它放在一个有效的范围内，看看会发生什么:

![](img/1d04b14126a5eeeff37da8ac180442fb.png)

很好！我们验证了键值的唯一性约束也得到维护(我们已经部署了一个 ID 为 1 的记录)。请将其设置为另一个 ID，然后重试:

![](img/c28ca552cd69fb2f675d717a50d69132.png)

成功！当然，现在我们数据库里有两本叫《呼啸山庄》的书。有趣的是，还可以尝试在 title 字段上实现唯一性约束。

接下来，让我们看看用户界面，看看它是什么样子的:

![](img/407390bcc904d19fbd0cc6cc2e929311.png)

还不错！数据会显示出来，但是我们仍然需要配置 UI 来显示它。我们将在步骤 2 中解决这个问题。

# 步骤 2:将评级字段显示为星号

和往常一样，第一步是使用我们的 *LineItem* 注释来设置该列默认显示，并使用 *title* 注释来设置该字段在应用程序中出现的公共标签。到现在为止，您应该知道如何做到这一点，所以我不会在这里介绍它。

然而，新的是我们行项目的内容。请记住，过去我们只使用了两个属性:包含字段名称的*值*，以及允许我们提供特定于表格的标签的*标签，这与适用于任何地方的标准标签相反。该行项目将不会使用这两个选项，而是看起来像这样:*

```
 {
      $Type  : 'UI.DataFieldForAnnotation',
      Target : '[@UI](http://twitter.com/UI).DataPoint#rating'
    },
```

我们从一个 *$Type* 注释开始。我们以前没有使用过它，因为省略它会产生默认的注释 UI。“DataField ”,这是表字段的标准类型，它只是按原样显示数据，不做任何处理。您甚至可以通过在[http://localhost:8080/API/browse/$ metadata](http://localhost:8080/api/browse/$metadata)查看我们的 oData 服务的元数据来确认这一点:

![](img/6e1d5b6378f3625f3ed2e5a8992c158b.png)

这里我们的类型是 *DataFieldForAnnotation* 。这仅仅意味着一个单独的注释将描述如何显示这个 *LineItem* 。

下一个注释是 *Target* ，它表示包含如何显示字段的信息的注释。*目标*属性的值为' [@UI](http://twitter.com/UI) 。“数据点#评级”。这意味着一个 *UI。ID 为“rating”的数据点*注释是可视化定义的位置。我们还没有做这个，所以让我们继续做吧。

在文件底部附近的 *PresentationVariant* 注释下方，添加以下内容:

```
 DataPoint #rating   : {
    Value         : rating,
    Visualization : #Rating,
    TargetValue   : 5
  }
```

这里我们定义了我们的*数据点*注释。注意# rating——它是这个注释的 ID。*数据点*注释用于可视化单个值；例如，还有其他可视化方式可以在图形中显示聚合数据。

*数据点*注释可以有很多不同的属性，但是在我们的例子中我们只需要三个:*值*、*可视化*和*目标值*。*值*是将使用此*数据点*可视化的字段，因此我们选择*评级*。*可视化*获取可视化类型的 ID。有许多选择，但我们选择 *#Rating* ，它将接受一个整数或小数，并将数据显示为星号。最后一个属性 *TargetValue* ，指定要显示的星星总数。因为我们之前将字段的最大值设置为 5，所以我们也将这里的目标值设置为 5。

让我们在 UI 中检查结果:

![](img/ff8c29e00574674ceda88ed7ba551c9a.png)

看起来不错！

# 结论

这一周到此结束。又短又甜！下周我们将开始研究你真正来这里的目的——当我们实现*添加评论*按钮时，如何向应用程序添加定制的 Java 逻辑。那里有很多事情要做，所以我们可能要花几个星期才能完成。敬请期待！

本教程中有什么不清楚的吗？请在下面留下问题，我会尽快回复您。有什么不对吗？请在下面留下评论，让我知道(更正的来源将是最有帮助的)。谢谢大家的评论！

# 支持

你喜欢这个博客吗？想确保我能继续创作吗？然后考虑在 [Patreon](https://www.patreon.com/toadhousetutorials) 上订阅！