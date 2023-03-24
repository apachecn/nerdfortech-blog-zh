# 学习围棋—数组和切片

> 原文：<https://medium.com/nerd-for-tech/learning-go-arrays-slices-bc8106f345e6?source=collection_archive---------4----------------------->

![](img/f5bb59682f6053470d75c70cac45c605.png)

在这篇文章中，我将讨论数据类型——数组和切片。

# **数组**

数组是存储在连续内存位置的数据集合。Go 中的数组可以初始化为

```
**Method 1**
<variable> := [size of array] <type> {valueset}
This will initialize an array of size as specified in - <size of array>
grades := [3] int{97, 98, 99}**Method 2**
<variable> := [...] <type> {valueset}
This will initialize an array with size as in the number of elements in the valueset.
grades := [...] int{97,98, 99, 100}
size - 4**Method 3**
var <variable> [size of array] <type>
This is declaration without initialising values.
var grades [3] int
grades[0] = 5
grades[1] = 10fmt.Printf("Grades : %v", grades)Result
Grades : [97 98 99]
Grades 2 : [97 98 99 100]
Grades 3 : [5 10 0]
```

# **访问特定的索引元素**

和其他语言一样，访问元素非常简单

```
<variable>[<index>]
Access 2nd element from our grades array - grades[1]
```

# **数组长度**

内置函数 len()返回数组的大小。

```
fmt.Printf("Grades : %v has size : %d\n", grades, len(grades))Result
Grades : [5 10 0] has size : 3
```

# **2D 阵**

2D 数组是指数组的数组。它们可以按如下方式初始化

```
var matrix [3][3] int
matrix[0] = [3] int{1, 0, 0}
matrix[1] = [3] int{0, 1, 0}
matrix[2] = [3] int{0, 0, 1}
OR
var matrix [3][3] int = [3][3] int{[3] int{1, 0, 0}, [3] int{0, 1, 0}, [3] int{0, 0, 1}}fmt.Printf("2D matrix : %v\n", matrix)Result
2D matrix : [[1 0 0] [0 1 0] [0 0 1]]
2D matrix : [[1 0 0] [0 1 0] [0 0 1]]fmt.Printf("2D matrix : %v, no of rows : %d, now of columns : %d\n", matrix, len(matrix), len(matrix[0]))
2D matrix : [[1 0 0] [0 1 0] [0 0 1]], no of rows : 3, now of columns : 3
```

> **数组是值类型** 正如我们在 Java、C 和 C++等编程语言中习惯于理解数组是引用类型一样，在 Go 中情况有所不同。Go 中的**数组**是**值类型**，即它们保存的是值，而不是对该值的内存位置的引用。

```
arrA := [...] int{1, 3, 5}
arrB := arrA
arrB[1] = 6
fmt.Println("arrA : ", arrA)
fmt.Println("arrB : ", arrB)Result
arrA :  [1 3 5]
arrB :  [1 6 5]The value in arrA[1] did not change even though we updated arrB[1]
```

> **注意**:通过给变量赋值将一个数组复制到另一个变量实际上创建了一个新数组。请记住，如果您将一个大数组复制到另一个数组，这在您的代码中可能是致命的。

# **&(地址操作符)用于数组引用**

为了通过引用提供 Java/C/C++数组副本，Go 提供了& operator。

```
arrAA := [...] int{1, 3, 5}
arrBB := &arrAA
arrBB[1] = 6
fmt.Println("arrAA : ", arrAA)
fmt.Println("arrBB : ", arrBB)Result
arrAA :  [1 6 5]
arrBB :  &[1 6 5]
```

# **切片**

数组适合在类似值类型、随机访问、连续内存位置等情况下使用数据类型。但是它们必须有固定的大小。

***从而解决了固定尺寸的问题，将切片变成图片*** 。它们类似于具有可变长度的强大的轻量级数组数据结构。

Slice 在内部将数据存储在数组中。Slice 只是对这个内部数组的引用。

```
An array is of fixed length. Slice stores a reference to the array. So how does slice manage to be variable length?
When a slice is added a value and the internal array is full, slice initialises a new array.
```

# **初始化**

```
**Method 1**
<variable> := [empty] <type> {<initial elements>}
sliceA := [] int{1, 2, 5}
sliceAA := [] int  // is a nil slice with no internal array initialised. has some specific behaviour.**Method 2**
arrC := [...] int{1,2,3,4,5,6,7}
sliceB := arrC[2:4]fmt.Printf("sliceA : %v, length of sliceA : %v, capacity of sliceA : %v", sliceA, len(sliceA), cap(sliceA))Result
sliceA : [1 2 5], length of sliceA : 3, capacity of sliceA : 3
```

# **切片的容量— cap()**

除了数组的长度函数，还有一个切片的容量函数。**容量是指切片可以容纳的元素数量**。这取决于切片开始的数组的索引。**片可以保存从起始索引开始的值，直到内部数组**结束。

```
arrA := [...] int {1,2,3,4,5,6,7}
sliceA := arr[2:4]  // [3, 4]
len(sliceA) = 2
cap(sliceA) = 5  // indices - 2,3,4,5,6
```

# **标准内置功能**

## **### make()函数**

```
makeSlice := make([]int, 4)
makeSlice := make([]int, 4, 100)fmt.Printf("makeSlice : %v, length of makeSlice : %v, capacity of makeSlice : %v", makeSlice, len(makeSlice), cap(makeSlice))Result
makeSlice : [0 0 0 0], length of makeSlice : 4, capacity of makeSlice : 4
makeSlice : [0 0 0 0], length of makeSlice : 4, capacity of makeSlice : 100
```

> **注**:根据论点观察容量差异。您可以使用不同的初始长度 0 和容量来初始化切片。

## **### append()函数**

可以通过内部 append()函数向 slice 添加值。
类似于 java 中的 hashmap 大小行为，当新元素被添加到 hashmap 中并且 hashmap 已满时，hashmap 大小以 2 的顺序递增。

```
makeSlice := make([]int, 4, 100)
fmt.Printf("makeSlice : %v, length of makeSlice : %v, capacity of makeSlice : %v\n", makeSlice, len(makeSlice), cap(makeSlice))
makeSlice = append(makeSlice, 1, 2, 4, 6)
fmt.Printf("makeSlice : %v, length of makeSlice : %v, capacity of makeSlice : %v\n", makeSlice, len(makeSlice), cap(makeSlice))ResultmakeSlice : [0 0 0 0], length of makeSlice : 4, capacity of makeSlice : 100
makeSlice : [0 0 0 0 1 2 4 6], length of makeSlice : 8, capacity of makeSlice : 100
```

**用例**
追加在尝试处理以下内容时很方便:

*   追加两个切片
*   final slice = append(make slice 1[0:1]，makeSlice2[4:6]…)
*   在单个切片中追加两组数据
*   final slice = append(make slice[0:2]，makeSlice[4，6]…)

> **注意—** 请记住在 append()的第二个参数末尾使用扩展运算符(…)

## **不寻常的行为**

```
fmt.Println(makeSlice)
newSlice := append(makeSlice[0:2], makeSlice[4:6]...)
fmt.Println(makeSlice)
fmt.Println(newSlice)Result[0 0 0 0 1 2 4 6]  // makeSlice initial
[0 0 1 2 1 2 4 6]  // makeSlice got updated!!! HOW?
[0 0 1 2]  // newSlice
```

> **注意**:注意 makeSlice 在对其中的值进行切片形成 newSlice 时的异常修改。我们将在后面的帖子中讨论这个问题！！！

**### copy()函数** Go 提供了一个内置的 **copy()** 函数，用于将值从一个片复制到另一个片

```
func copy(dst []Type, src []Type) int
copy (sliceA, sliceB)  // copies value from sliceB to sliceA till length min (sliceA, sliceB)
```

> **注意**:如果你在 python 中遇到过 slicing()，那么你将能够在 Go 的上下文中更好地关联 slice

**不同类型的切片** 以下是可以创建的各种类型的切片

```
a := [...] int{1,2,3,4,5,6}
b := a[:]  // slice of all elements
c := a[3:]  // slice of all elements from index 3
d := a[:6]  // slice of all elements till index 6-1 = 5
e := a[3:6]  // slice of elements starting index 3 till 6-1 = 5
```

我已经到了帖子的末尾。这对我来说是一次有趣的学习。我未来的帖子将会有更多基于学习的东西，只要我能够分享。到那时再见。我还会发布一个关于 golang 中常量和枚举器的小帖子。我以前错过了它，将在适当的时候发布。