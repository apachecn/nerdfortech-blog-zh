# 感受并让数组在 iOS 开发中更加强大

> 原文：<https://medium.com/nerd-for-tech/feel-and-make-the-arrays-more-powerful-in-ios-development-5de1b8b0ff3f?source=collection_archive---------16----------------------->

![](img/4785a04a10bcb07bfeefe0ba32f5fb24.png)

照片由[穆罕默德·迈赫迪·阿巴斯](https://unsplash.com/@mohamad_mahdi?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

在编写自定义函数之前，让我们重新访问一下可用的内置函数，这些函数相对较少使用，但功能强大。

## 作为索引的范围运算符

我们可以使用开值域或闭值域操作符来访问和修改数组中的元素。如果数组不能适应变化，那么它将抛出错误。

```
var array = [1,2,3]array[0…1] = [2,2]
print(array[0..4]) // error: Index Out Of Range
```

## 枚举

enumerated()将在每次迭代中返回一个包含索引和元素的对。

```
array.enumerated().forEach({print(“\($0) -> \($1)”)})
```

## 元素序列

基于索引比较两个数组中的每一个元素，只有在各自索引处的所有元素都相等时才返回 true。这与 array == arrayCopy 相同。

```
array.elementsEqual(arrayCopy)
```

## 前缀

如果我们想从数组中取出前 n 个元素，并且不确定数组的大小，那么我们可以使用 prefix。这将返回指定数量的元素(如果可用)。但是如果数字超过了数组的长度，那么它将返回完整的数组。

```
array.prefix(20)**var** test:[Int] = []print(test.prefix(10)) // []
```

## FirstIndex/LastIndex

firstIndex(of:)或 lastIndex(of:)通常用于了解元素的索引。但是还有 firstIndex(where:)或者 lastIndex(where:)更强大。我们可以使用自定义谓词来查找索引，如下所示。

```
//Finding first number which is greater than 2array.firstIndex(where: {$0 > 2})//Finding first empty string in a listarrayNames.firstIndex(where: {$0.isEmpty})
```

## 斯瓦帕特

为了交换数组内部的元素，我们可以使用 swapAt(from_index，to_index)。

```
array.swapAt(0, 1)
```

# 延长

现在让我们给数组更多的功能

## 替换事件

当我们需要用其他元素替换一个元素时，我们可以利用这个扩展

```
**extension** Array { **mutating** **func** replaceOccurrences(of value: Element, with val:    
   Element) **where**  Element: Equatable { **var** ex = **false** **while** ex == **false** { **if** **let** i = **self**.firstIndex(of: value ) { **self**[i] = val } **else** { ex = **true** }
   }
 } **mutating** **func** replaceFirstOccurence(of value: Element, with val:    
   Element) **where**  Element: Equatable { **if** **let** i = **self**.firstIndex(of: value ) { **self**[i] = val } 
 } **mutating** **func** replaceLastOccurence(of value: Element, with val:    
   Element) **where**  Element: Equatable { **if** **let** i = **self**.lastIndex(of: value ) { **self**[i] = val } 
 }
}
```

## 独特元素

拥有关于数组中唯一元素的信息总是有用的。在那种情况下，下面的内容会很有用。

```
**extension** Array { **func** unique() -> [Element] **where** Element: Equatable { **var** newArray: [Element] = [] **self**.forEach { i **in** **if** !newArray.contains(i) { newArray.append(i) } } **return** newArray } **func** uniqueElementsCount() -> [Element : Int] **where** Element: 
     Equatable { **var** newArray: [Element:Int] = [:] **self**.forEach { i **in** **if** !newArray.keys.contains(i) { newArray[i] = 0 } **else** { newArray[i] = (newArray[i] ?? 0) + 1 } } **return** newArray }}
```

## 元素计数

当您需要知道一个元素在数组中出现了多少次时，可以使用下面的代码片段

```
**extension** Array { **func** countOf(element: Element) -> Int **where** Element: Equatable { **var** ex = 0 **self**.forEach { i **in** **if** i == element { ex = ex + 1 } } **return** ex }}
```

## 分组字符串

当你有一系列的名字，你想把它们按字母顺序排列，使用下面的扩展。

```
**extension** Array {**func** grouped() -> [(alphabet: Character, values: [String])] **where** 
  Element == String  { **return** Dictionary(grouping: **self**) { (country) -> Character **in** **return** country.first ?? "-" } .map({(alphabet: $0, values: $1)})       .sorted(by: {$0.alphabet < $1.alphabet}) }}
```

# 我们无意中犯的常见错误

我无意中犯的一个常见错误是试图添加不同数据类型的元素。

```
var arr = [1,2,3]
arr.appennd("four") // Not possible
```

最好的解决方法是用“Any”使数组匿名

```
var arr: [Any] = [1,2,3]
arr.append("four")
```

好了..

感谢阅读…