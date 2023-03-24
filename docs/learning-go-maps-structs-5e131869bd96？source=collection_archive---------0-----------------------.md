# 学习围棋—地图和结构

> 原文：<https://medium.com/nerd-for-tech/learning-go-maps-structs-5e131869bd96?source=collection_archive---------0----------------------->

![](img/f5bb59682f6053470d75c70cac45c605.png)

这篇文章致力于 Go 中的另一种数据类型——**映射**和**结构**。我们开始吧！

# **地图**

Map 是存储<key value="">对的数据结构。映射的键只能是值类型数据类型，即可以将 int、string、array 作为键。不能将类似切片的引用类型作为映射键。地图可以初始化为</key>

> **注意**:注意地图打印的顺序。<键，值>对以 lex 键排序打印。

## **添加一个<键，值>对**

```
makeMap["Punjab"] = 180
fmt.Println(makeMap["Punjab"])**Result**
180
```

## **访问一个值**

访问一个值类似于数组 **[]** 操作符。

```
fmt.Println(makeMap["UP"])**Result**
235
```

## **删除一个<键，值>对**

要删除 map 中的一个条目，我们可以使用内部的 **delete()** 方法。

```
fmt.Println(makeMap)
delete(makeMap, "Punjab")
fmt.Println(makeMap["Punjab"])
fmt.Println(makeMap)**Result**
map[Delhi:150 Karnataka:150 MP:175 Maharashtra:200 Punjab:180 Rajasthan:165 UP:235]
0  // Instead of returning an error or nil, **we get a 0?**
map[Delhi:150 Karnataka:150 MP:175 Maharashtra:200 Rajasthan:165 UP:235]
```

> →重新访问
> 如果您观察到**结果中的第二条语句，**我们**得到一个 0 值**用于打印关键字**“Punjab”**post，将其从地图中删除。这是令人困惑的，为什么我们得到任何值时，键已被删除。为了解决这个问题，可以使用什么来代替**访问地图**中的键

```
punjabPop, ok := statePop["Punjab"]
fmt.Println(punjabPop, ok)**Result**
0 false  // false indicates that key was not found. This can be used to verify for the presence of key and then using the value.
```

> 注意:ok 不是必须的，但它是围棋中遵循的惯例。这是一个布尔变量。

## **地图的长度**

使用 **len()** 函数可以确定地图的长度。

```
fmt.Println("Length of map : ", len(makeMap))**Result**
Length of map : 6
```

## **作为参考类型的地图**

地图是一种引用类型。因此，如果一个变量 **x 被赋予另一个映射**变量的值，x 开始引用该映射。在 x 上所做的任何改变也会改变底层地图。

```
cloneMap := makeMap
fmt.Println("makeMap : ", makeMap)
fmt.Println("cloneMap : ",cloneMap)
cloneMap["Maharashtra"] = 190
fmt.Println("makeMap : ", makeMap)
fmt.Println("cloneMap : ",cloneMap)**Result**
makeMap : map[Delhi:150 Karnataka:150 MP:175 Maharashtra:200 Rajasthan:165 UP:235]
cloneMap : map[Delhi:150 Karnataka:150 MP:175 Maharashtra:200 Rajasthan:165 UP:235]
makeMap : map[Delhi:150 Karnataka:150 MP:175 Maharashtra:190 Rajasthan:165 UP:235]
cloneMap : map[Delhi:150 Karnataka:150 MP:175 Maharashtra:190 Rajasthan:165 UP:235]
```

# **结构**

一个**struct**(“structure”的缩写)是一个带有声明数据类型的数据字段的集合。Go 能够通过组合一个或多个类型来声明和创建自己的数据类型，包括内置和用户定义的类型。结构中的每个数据字段都是用已知类型声明的，该类型可以是内置类型或其他用户定义的类型。

如果你在过去的编程经验中练习过 **C** ，那么 struct 对你来说很容易理解。

## 观察:

1.  结构定义在 **func main()** 之外声明
2.  struct 实例是在 **func main()** 中声明的
3.  在实例化过程中，字段被赋予一个值。
4.  **、**、 **}、**以尾随逗号结束。如果没有逗号，你会得到错误。

## **不同的实例化方法**

**位置声明—** 我们可以在初始化期间赋值时省略字段名。Go 将根据字段声明的顺序，自动将值映射到定义中的相应字段。**我不推荐**这样做，因为如果有一天，定义在预声明的字段之间添加了一个新字段，那么 Go 编译器会按照新的字段声明顺序映射值，这会产生一个错误。

```
emp2 := Employee{
   123,
   "prateek",
   []string {
      "Technology",
      "MFP",
      "Delivery",
   },
}
```

> **注意:**正如在这篇[文章](https://guptaprateektechtalks.wordpress.com/2020/04/04/learning-go-variables/)中所讨论的，如果**以大写字母开始命名，那么结构定义和字段可以在当前包之外公开。**

## **速记符号**

这个简写符号是匿名结构。这仅在使用它的函数中可用，而不是在包级别。

```
doc := struct {
   name string
}{name: "prateek"}
fmt.Println("Doctor : ", doc)Result
Doctor : {prateek}
```

## **结构是值类型**

**与映射不同，结构是值类型，而不是引用类型**。当变量值被赋予结构变量值时，该值被复制。对新变量的更改不会对原始结构产生副作用。

```
anotherDoc := doc
anotherDoc.name = "gupta"
fmt.Println("doc Doctor : ", doc)
fmt.Println("anotherDoc Doctor : ", anotherDoc)**Result**
doc Doctor : {prateek}
anotherDoc Doctor : {gupta}
```

## **访问结构的值**

可以使用**点(.)**操作员。

```
fmt.Printf( "Employee id : %v and name : %v and departments : %v", 
emp.id, emp.name, emp.dept)**Result** Employee id : 123 and name : prateek and departments : [Technology MFP Delivery]
```

## **&【地址】操作员**

就像数组一样， **&** 在结构上也很相似。如果裁判更新了值，则被引用的结构会被修改。

```
refDoc := &doc
refDoc.name = "gupta"
fmt.Println("doc Doctor : ", doc)
fmt.Println("refDoc Doctor : ", refDoc)**Result**
doc Doctor : {gupta}
refDoc Doctor : &{gupta}
```

## **嵌套结构**

围棋**没有*传承*。**代替**跟随*构图*。** Go 不遵循 IsA 关系，而是遵循 hasA 关系。**嵌套结构**就是实现这一点的方法。

```
type Person struct {
   name string
   age int
}type Profession struct {
   Person
   myProfession string
}func main() {
myProf := Profession{
   Person : Person{
      name : "prateek",
      age : 27,
   },
   myProfession: "Engineer",
}
fmt.Println(myProf)yourProf := Profession{}
yourProf.name = "prateek"
yourProf.age = 27
yourProf.myProfession = "Engineer"
fmt.Println(yourProf)
}**Result**
{{prateek 27} Engineer}
{{prateek 27} Engineer}
```

观察**结构人在结构职业中的嵌套。** 可以观察变量 **myProf** 和 **yourProf。**两者都是初始化结构体的不同方法。

## **标签**

标记是突出显示结构中验证规则的一种方式。您可以**将规则标记到结构内部的字段**，并使用**反射库**将这些映射的规则打印/返回到字段。

```
// Updated the previously declared Person struct with tagstype Person struct {
   name string `required max : "10"`
   age int `required min > 18 and max < 80`
}type Profession struct {
   Person
   myProfession string
}refPerson := reflect.TypeOf(Person{})
field, _ := refPerson.FieldByName("name")
fmt.Println(field.Tag)**Result**
required max : "10"
```

这里我们到了帖子的结尾。我们今天讨论了**贴图**和**结构**。我们讨论了很多关于声明的东西，不同的声明和初始化方法，标签和使用。

我将在下一期的**学习围棋系列文章中回来。**在此之前，如果你发现任何差异，或者如果你想让这篇文章更好，请发表你的**评论，如果你觉得有用，请** **分享**。