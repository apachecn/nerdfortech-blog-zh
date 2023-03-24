# Swift 中的排序扩展

> 原文：<https://medium.com/nerd-for-tech/sorting-extension-in-swift-438ed6f23ce1?source=collection_archive---------5----------------------->

![](img/a95c0099d4597fdf21d1bdfba23d06a8.png)

格伦·卡斯滕斯-彼得斯在 Unsplash 上的照片

在 iOS 开发中，我们多次遇到对数组进行排序的任务，然后我们在应用程序中任何需要的地方编写如下代码。*注意到* ***动物*** *的数组还没有定义，动物的结构也没有定义，但是不要担心，它们将在本文的后面被指定。*

```
animals.sorted { (left, right) -> Bool in {
    return left.id > right.id
}
```

## 1.问题是

如果不是按 ***id*** 对动物数组进行排序，而是现在需要按 ***名称*** 或动物拥有的任何其他属性进行排序呢？好吧，我们可能会重复相同的代码，但是通过所需的属性来改变 id。

```
animals.sorted { (left, right) -> Bool in {
    return left.name > right.name
}animals.sorted { (left, right) -> Bool in {
    return left.age > right.age
}
```

随着我们向项目中添加更多具有多个属性的结构，这个简单的任务开始变得乏味，并且在出现错误的情况下更难调试。一个更干净的方法是，创建一个数组的扩展，允许我们根据任何属性对任何数组进行排序。

## 2.我们想要达到的目标

那么，我们如何做到这一点呢？让我们首先定义我们将在本文中使用的结构动物，它的属性 *id* 为整数，属性 *name* 为字符串。

```
struct Animal: CustomStringConvertible {
    let id: Int
    let name: String

    var description: String {
        return "Animal(id: \(id), name: \(name))"
    }
}
```

现在，让我们找到一种更好的方法来编写我们看到的第一段代码，不是实现，而是当我们需要对任何类型的数组进行排序时，我们希望代码看起来是什么样子。

```
// 1\. Define the array of animals
let animals = [
    Animal(id: 1, name: "platypus"),
    Animal(id: 2, name: "chicken"),
    Animal(id: 3, name: "dog")
]// 2\. This is the way we's like to sort them, by id
animals.sorted(by: { $0.id })// 3\. or rather by name
animals.sorted(by: { $0.name })
```

请记住，我们不想将这种行为特别附加到动物或任何其他已定义的结构/类，相反，这种解决方案应该是通用的，并且在本示例之外的任何其他上下文中也可以重用。

## 3.解决方案

因为该方法是任何类型的数组扩展，所以让我们创建一个通用函数，它可以处理任何" *sortable* "对象，我所说的 sortable 是指任何符合协议 *Comparable、*的东西，这样我们就知道哪些元素比其他元素大，即使它们不是具体的类型(T)。此外，这个函数应该返回一个新的排序数组，这样我们就可以重用 Swift 的“sorted()”内置函数。

```
extension Array {
    func sorted<T: Comparable>() -> Array {
        // Here, our nice code
    }
}
```

这个函数需要接收什么参数？这当然是一个闭包，但是这个闭包需要接受一个类型为*元素*的对象，并返回另一个类型为 *T* 的对象。请注意，我将闭包命名为 ***compare*** ，因为它是一个在比较每一对值时将对数组中的每个元素执行的块。

```
func sorted<T: Comparable>(by compare: (Element) -> T) -> Array {
    // Here, our nice code
}
```

我们可以使用带有一对元素的*比较*块，根据同一个块*比较的结果来检查哪一个大于另一个。*例如:

```
// We can now compare using > or < on the results of compare()compare(array.first!) > compare(array.last)// NOTE: We don't really know what the result of this operation is, because it's a generic array of T, but what we do know, is that *first* and *last* elements can be compared, because T is a comformance of the *Comparable* protocolcompare(array[0]) will give <T: Comparable> value in return
compare(array[1]) will give another <T: Comparable> value in return
```

填充函数后，到目前为止，我们已经有了这段代码，看起来很整洁，并且(不完全)按照我们的期望…按照任何给定的属性对任何类型的元素数组进行排序。

```
func sorted<T: Comparable>(by compare: (Element) -> T) -> Array {
   return self.sorted {
        // BUG 1\. We are forcing the comparison to be >
        compare($0) > compare($1)
    }
}
```

然而，正如评论 *BUG 1* 。说，我们对所有的排序调用强制进行大于或小于的比较，但这不是应该的工作方式，有时我们需要一个动物数组升序排序，但也需要一个用户数组降序排序。这就是为什么我们需要添加一个新的布尔参数叫做 *ascendant(例子中标注为****ASC****)*其默认值为 true *。*

这是整个扩展最终的样子:

## 奖金

在 Swift 的新版本中，我们可以使用这个完全相同的扩展，但是我们可以发送一个属性 KeyPath，而不是发送一个闭包，好的一点是，价格相同，我们不需要额外的代码。因此，我们可以这样排序数组:

```
// KeyPath for Animal.name
animals.sorted(by: \.name)// KeyPath for Animal.id
animals.sorted(by: \.id, asc = false)
```

这个排序包装器扩展的另一个很棒的用例是当结构有嵌套对象时。假设您有动物和人，其中动物属于一个人，并且您希望按照动物所属的人的属性对动物进行排序。例如:

```
// CREATE THE STRUCTURESstruct Person {
    let id: Int
    let name: String
}struct Animal {
    let id: Int
    let name: String
    let owner: Person
}let person1 = Person(id: 3, name: "bob")
let person2 = Person(id: 2, name: "alice")
let person3 = Person(id: 1, name: "charlie")let animals = [
    Animal(id: 1, name: "dog", owner: person1),
    Animal(id: 2, name: "cat", owner: person2),
    Animal(id: 3, name: "turtle", owner: person3)
]// SORT THE ARRAYS// sort animals by the owner's id
animals.sorted(by: { $0.owner.id })// => animal: 3-turtle, person: 1-charlie
// => animal: 2-cat, person: 2-alice
// => animal: 1-dog, person: 3-bob// sort animals by the owner's name
animals.sorted(by: { $0.owner.name })// => animal: 2-cat, person: 2-alice
// => animal: 1-dog, person: 3-bob
// => animal: 3-turtle, person: 1-charlie
```

*感谢阅读。我希望你喜欢这个小教程，如果它对你有用，不要害羞👏关于这篇文章。下次见。*