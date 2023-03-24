# 让 swift 的“contains()”和“filter()”为 iOS 开发做好准备

> 原文：<https://medium.com/nerd-for-tech/make-swift-contains-and-filter-ready-for-ios-development-e60a6570f5a2?source=collection_archive---------8----------------------->

![](img/4c43b9321fdb7efe66bd62701ec85ff3.png)

丹尼斯·布雷克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Swift 拥有大量功能，可随时用于 iOS 开发。swift 中的 contains()和 filter()在我们需要根据条件分别搜索列表和过滤器列表中是否存在某个编号时非常有用。

在某些情况下，这些功能不能这样使用。一个这样的场景是搜索字符串(在联系人中搜索的功能)。在联系人中搜索获取一个或多个关键字，并在每个联系人的名字或姓氏中搜索它们，即搜索顺序无关紧要的位置。

我们将扩展 Array 来使用 contains 和 filter 来搜索和过滤一个数组，其中关键字的顺序无关紧要，并且关键字部分存在于列表的成员中。

# partiallyContains()/partiallyContains filter()

当我们在数组上直接应用 contains 时，只有当关键字完全匹配时，它才返回 true。当我们在['array']上应用带有' arr '的 contains 时，它返回 false。如果我们想让它更灵活，那么就使用下面的扩展。

```
**extension** Array { **func** partiallyContains(check: Element) -> Bool **where** Element ==   
   String { **for** str **in** **self** { **if** str.contains(check) { **return** **true** } } **return** **false** } **func** partiallyContainsFilter(word: Element) -> [Element] **where**   
   Element == String { **self**.filter({$0.contains(word)}) }
}**var** array = ["hello","world","hello world"]array.contains("he") // falsearray.partiallyContains(check: "he") // truearray.partiallyContainsFilter(word: "he") // ["hello", "hello world"]
```

# relaxedOrderContains()/relaxedOrderFilter()

当我们在["hello world"]上应用带有' world hello '的 contains 时，它返回 false。但是，如果你想有宽松的订单功能，然后使用以下扩展。

```
extension Array { **func** relaxedOrderElementsEqual(with array: [Element]) -> Bool 
  **where** Element == String { **var** result = **true** **for** i **in** array { **if** !**self**.contains(i) { result = **false** **return** result } } **return** result }
  **func** relaxedOrderContains(word: Element) -> Bool **where** Element == 
  String { **let** arrayToBeChecked = word.components(separatedBy: " ") **var** res = **false** **for** i **in** **self** { **let** currentArrayToBeChecked = i.components(separatedBy: " ") **if** arrayToBeChecked.relaxedOrderElementsEqual(with:    
       currentArrayToBeChecked) { res = **true** **return** res } } **return** res } **func** relaxedOrderFilter(word: Element) -> [Element] **where** Element 
  == String { **let** arrayToBeChecked = word.components(separatedBy: " ") **return** **self**.filter { i **in** **let** wordtobechecked = i.components(separatedBy: " ") **return** wordtobechecked.relaxedOrderElementsEqual(with:   
               arrayToBeChecked) } }
}**var** array = ["hello","world","hello world"]array.contains("world hello") // falsearray.relaxedOrderContains(word: "world hello") //truearray.relaxedOrderFilter(word: "world hello") // ["hello world"]
```

# 柔性过滤器()

但最后，因为我们的目标是文章的开始，我们需要为数组增加更多的灵活性，以便在“搜索联系人”的情况下使用。

```
extension Array { **func** flexibleElementsEqual(with array: [Element]) -> Bool **where** 
   Element == String { **var** result = **true** **for** i **in** array { **if** !**self**.partiallyContains(check: i) { result = **false** **return** result } } **return** result
  }
  **func** flexibleFilter(word: Element) -> [Element] **where** Element == 
   String { **let** arrayToBeChecked = word.components(separatedBy: " ") **return** **self**.filter { i **in** **let** wordtobechecked = i.components(separatedBy: " ") **return** wordtobechecked.flexibleElementsEqual(with: 
               arrayToBeChecked) } }}**var** array = ["hello","world","hello world"]array.flexibleFilter(word: "wo") // ["world", "hello world"]array.flexibleFilter(word: "rld") // ["world", "hello world"]
```

就是这样！！

当您需要灵活性时，可以使用这些扩展。

快乐发展…