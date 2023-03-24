# Swift 中的等价和可比

> 原文：<https://medium.com/nerd-for-tech/equatable-and-comparable-in-swift-77f626d2d3d5?source=collection_archive---------9----------------------->

## 探索可比性和等价性，帮助你解决常见问题。

![](img/247c98591dcf0de4c5763d9e8e26c796.png)

格伦·卡斯滕斯-彼得斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

## 介绍

在本文中，我将展示实现 Comparable 和 Equatable 在大多数情况下是如何帮助你的。我们走吧…

## 等价的

基于苹果的 Swift 文档:

> 可以比较其值是否相等的类型。符合`**Equatable**`协议的类型可以使用等于运算符(`**==**`)比较相等性，或者使用不等于运算符(`**!=**`)比较不相等性。Swift 标准库中的大多数基本类型都符合`**Equatable**`。

## 问题

```
struct Employee {
   let department: String
   let name: String
   let happinessIndex: Int
}**///employee we want to compare
let** johnFromIT = Employee(department: "IT", name: "John Doe", happinessIndex: **0.8**)**let** johnFromHR = Employee(department: "HR", name: "John Doe", happinessIndex: **0.8**)**let** alsoJohnFromIT = Employee(department: "IT", name: "John Doe", happinessIndex: **0.3**)
```

如果我们想要检查是否是同一个员工，我们必须检查那个人是否有**相同的名字**和**相同的部门**。

```
**func** isSamePerson(lhs: Employee, rhs: Employee) -> **Bool** {
   **return** lhs.name == rhs.name && lhs.department == rhs.department
}**///result** isSamePerson(lhs: johnFromIT, rhs: johnFromHR) **//false**
isSamePerson(lhs: johnFromIT, rhs: alsoJohnFromIT) **//true**
```

让我们用等式操作符(`***==***`)或(`***!=***`)来简化，要做到这一点，我们必须让我们的雇员符合等价的扩展。

```
struct Employee: Equatable {
   let department: String
   let name: String
   let happinessIndex: Int
}**///result** johnFromHR == johnFromIT **//false**
johnFromIT == alsoJohnFromIT **//false**
```

从上面的结果来看， **johnFromIT** 和**alsohanfromit**T21 不是同一个人，因为编译器会比较存储的每一个等价属性(包括 **happinessIndex** )。所以要使这成为我们想要的，我们必须实现我们自己的比较方法。

```
**struct** Employee: Equatable {
   **let** department: String
   **let** name: String
   **let** happinessIndex: Double **static** **func** == (lhs: Employee, rhs: Employee) -> **Bool** {
      **return** lhs.name == rhs.name && 
             lhs.department == rhs.department
   }
}**///result** johnFromHR == johnFromIT **//false**
johnFromIT == alsoJohnFromIT **//true** [johnFromHR, johnFromIT].contains(alsoJohnFromIT) **//true**
```

## 可比较的

基于苹果的 Swift 文档:

> `**Comparable**`协议用于具有固有顺序的类型，比如数字和字符串。当您希望能够使用关系操作符比较实例或者使用为`**Comparable**`类型设计的标准库方法时，将`**Comparable**`一致性添加到您自己的定制类型中。

简而言之，可比协议允许值被认为小于或大于其他值。所以让我们继续以员工为例。

```
**let** geralt = Employee(department: "Finance", name: "Geralt Rivia", happinessIndex: 0.9)**let** anna = Employee(department: "HR", name: "Anna Henrietta", happinessIndex: 0.7)**let** triss = Employee(department: "IT", name: "Triss Merigold", happinessIndex: 0.8)**let** employees = [geralt, anna, triss]
```

假设我们的目标是从员工列表中找到最快乐的员工

```
**struct** Employee: Comparable {
...
   **static** **func** < (lhs: Employee, rhs: Employee) -> Bool {
      **return** lhs.happinessIndex < rhs.happinessIndex
   }
}
**///result** empployees.sorted() **//[Anna, Triss, Geralt] sort by lowest index** empployees.max() **//Geralt, is happiest employee** empployees.min() **//Anna**
```

欢迎提问和指正。