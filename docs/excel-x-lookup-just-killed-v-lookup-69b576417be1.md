# Excel: X-Lookup 刚刚扼杀了 V-Lookup

> 原文：<https://medium.com/nerd-for-tech/excel-x-lookup-just-killed-v-lookup-69b576417be1?source=collection_archive---------9----------------------->

如果你有 Excel 365，是时候把 VLOOKUP 抛在脑后了！

微软发布了一个名为 XLOOKUP 的功能。这个函数应该取代广泛使用的 VLOOKUP、HLOOKUP 和 INDEX/MATCH 函数，在 Excel 数据表中进行搜索。

Excel 用户现在有了一个新的功能，更加友好，更加灵活，避免了一些令人沮丧和耗时的 VLOOKUP 错误。

![](img/90e3e2deff9c395240030625bd3a9342.png)

# Excel 中的 VLOOKUP 函数

看起来是这样的:

=VLOOKUP(查找值，表数组，列索引，[匹配模式])

*   lookup_value:这是您要查找的内容，这是您的第一个 id 列表。
*   table_array:这是你想找的地方，这里是第二个列表，是一个所有 id 和名称之间对应的表。
*   column_index:这是 table_array 中的列号，它包含您希望输出的数据。这里您需要第二列的内容(姓名)
*   [match_mode]:只有当 VLOOKUP 函数在表中找到精确匹配时，99%的情况下您才希望键入 0 来指定您想要的输出。如果未找到匹配项，将返回 [#N](https://www.powerusersoftwares.com/blog/hashtags/N) /A。当值为 0(或 FALSE)时，该函数仅在 table_array 中找到 lookup_value 时才返回值。
*   但是当它为 1(或 TRUE)时，即使没有找到 lookup_value，它也会返回找到的下一个最接近的值。有时它是有用的，但大多数时候它是一个危险的错误，给你错误的结果！

问题:

*   它只能从左到右工作。
*   具有期望结果的列由索引指定。如果希望 VLOOKUP 返回第二列的数据，需要键入 2。但是如果在某个时候插入另一列呢？索引仍然是 2，你的函数不再返回预期的结果。
*   微软在 VLOOKUP 上犯了一个大错误。在 VLOOKUP 中，match_mode 参数是可选的…但出于某种原因，他们决定默认情况下它将为真，而人们只将其用作假。所以虽然这个参数理论上是可选的，但实际上是强制的，因为每次都需要指定 FALSE。

# Excel 中的 XLookup

XLOOKUP 是 Excel 最新的查找函数，它解决了 VLOOKUP 的一些最常见的限制。

XLOOKUP 语法如下:

=XLOOKUP(查找值，查找数组，返回数组，[匹配模式]，[搜索模式])

像 VLOOKUP 或 INDEX/MATCH 一样，有 3 个强制参数。方括号中的最后 3 个参数是可选参数:

*   lookup_value:这是您要搜索的值
*   lookup_array:这是您要搜索的范围
*   return_array:这是您想要的结果的范围
*   [if_not_found]:代替 [#N](https://www.powerusersoftwares.com/blog/hashtags/N) /A，您可以指定在没有找到匹配的情况下应该返回什么。
*   [match_mode]:与 VLOOKUP 或 INDEX/MATCH 一样，使用 0 进行精确匹配(如果没有找到匹配，将返回 [#N](https://www.powerusersoftwares.com/blog/hashtags/N) /A)。但是但是…精确匹配是默认值，所以基本上大多数时候你不需要使用这个参数！如果没有找到匹配项，也可以使用-1 返回下一个较小的值，如果没有找到匹配项，则使用 1 返回下一个较大的值。也可以使用 2，这是一个通配符匹配，对字符“*”、“？”还有“~”。
*   [search_mode]:使用 1 从第一个项目开始搜索，使用-1 从最后一个项目开始搜索。还可以使用 2 来搜索按升序排序的 lookup_array，使用-2 来搜索按降序排序的 lookup_array。如果数组排序不当，返回的结果将无效。

有了 XLOOKUP，我们得到了两个世界的好处:像 VLOOKUP 一样，键入和记忆非常简单，像 INDEX/MATCH 一样，lookup_array 是一个引用，使它更加灵活。锦上添花的是，您不需要指定 match_mode 来获得精确匹配。

XLOOKUP 中的第四个参数是可选的[match_mode]。这个有四个选项。

前三个(0、1 或-1)类似于 MATCH 函数，允许执行精确匹配，以便在没有找到匹配的情况下获得下一个最小值，或者在没有找到匹配的情况下获得下一个更大的值。

第四个选项是通配符。使用 2 作为[match_mode]，它允许您利用通配符来运行部分匹配查找。这非常有用，是一个很棒的新特性。

星号(*)代表任意数量的字符，而问号(？)表示任何单个字符。

# XLOOKUP Vs VLOOKUP Vs 索引/匹配

让我们回顾一下 XLOOKUP 如何优于 VLOOKUP 和 INDEX/MATCH:

*   这是最简单的函数，大多数情况下只需要 3 个参数，因为默认的 match_mode 是 0(精确匹配)。
*   它是一个单一的函数，不像 INDEX/MATCH，所以打字更快。
*   它在垂直和水平方向都可以工作(不像 VLOOKUP 和它的另一个化身 HLOOKUP)。
*   它不要求查找值在左边(不像 VLOOKUP)。
*   当没有找到匹配时，它可以返回自定义结果而不是 [#N](https://www.powerusersoftwares.com/blog/hashtags/N) /A，而不需要组合附加函数。
*   它可以返回一个数组，而不仅仅是一个值。
*   当[search_mode]为-2 或 2 时，它可以进行二进制搜索，以更快地对排序后的数据进行计算。
*   当[match_mode]为 2 时，它可以运行部分搜索，利用通配符。
*   当[search_mode]为-1 时，它可以运行反向搜索。

# 为什么选择 Xlookup？

1)它可以从右边或左边返回数据(！)的查找列。

2)可以从上往下看，也可以从下往上看(！).

3)它包括一个“如果没有找到”参数(不需要 IFERROR！).

4)可以使用通配符进行部分匹配。

5)不需要对你的数据进行排序进行近似匹配，它可以返回值小于等于，或者大于等于(！)查找值。

好吧，这绝对是开始学习和使用 XLOOKUP 的一大堆理由！