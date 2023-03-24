# Python 基础

> 原文：<https://medium.com/nerd-for-tech/python-basics-84a5a6d58145?source=collection_archive---------15----------------------->

## 我的数据科学之旅(第 4 部分)

![](img/ff9dafd41c66a4e62e7d99491d4227f5.png)

Fidias Cervantes 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Python 是一种高级通用编程语言。Python 用于 web 开发、人工智能、机器学习、操作系统、移动应用程序开发和视频游戏。

Python 是一种中间方法(JIT)。Python 代码是用**中的**编写的。py 文件，并首先编译成字节码，用. pyc 或。pyo 格式。

**步骤**

1.  源代码:Python 代码
2.  编译:源代码被转换成字节码
3.  字节码:中间代码还是低级代码
4.  虚拟机:在这里，代码在库模块的支持下执行

字节码指令不是在 CPU 上执行，而是在虚拟机上执行。

# 关键字

Python 中的关键字是保留字，用作标识符、函数名或变量名。它们帮助定义语言的结构和语法。

True、False、and、or、None、return、not、assert、break、continue、class、def、if、else、elif、del

这些名称不能用于给变量赋值。

**名称空间:**python 中的名称空间是一个系统，它确保每个对象都有唯一的名称，以避免任何错误

# 基本数据类型

整数、布尔、浮点、字符串

数据以以下类型之一存储:整数(1，2，3)、浮点(1.0，2.5)、字符串(“Hello”)(所有字符串必须写在“-”内)、布尔(True，False)。

# 变量

变量是用来存储值的保留内存位置。

变量由 x=1，y=2 赋值，其中 x 是存储值 1 的变量名。

type(var)给出该变量的类型 type(x) →Integer

变量名不能以数字(2x)或空格 x y，x-y(接受 x_y)开头

Python 允许在一行中为多个变量赋值

x，y=1，2

# 输入和输出

可以使用语法 Input()从用户处获取输入。

输入("名称:")

可以使用 print()语法给出输出

```
print("a",end="\n")
print("b")
a
bprint("a",end=" ")
print("b")
a bprint("a","b","c")
a b cprint("a","b","c",sep="@")
a@b@c
```

## 字符串格式

运行时格式化字符串

1.  。格式()
2.  f 弦

**。格式()**

打印(“我叫{}”。format())在 format()中键入的任何内容都打印在{}内

```
print(“ My name is {} {}”.format("M","A")) 
→ My name is M A
print(“ My name is {fname} {lname}”.format(lname="A",fname="M"))
→ My name is M A
```

**f 弦**

```
fname='M'
lname='A'
print(f" My name is {fname} {lname}")
→ My name is M Aprint(" My name is %s %s" %("M","A"))    #%s for strings
→ My name is M Aprint("%d" %(1))
1
print("%.3f" %(1))
1.000print(f" {3.14159265:.4f}")             #: for additional formatting 
3.1416
# :.nf  n indicate number of digit after the .
```

# 铸造

x=1 python 会自动把 x 赋值为整数。您可以使用 float()语法将其转换为浮点型。float(x)=1.0。同理 int()，float()，str()。

# 算术运算符

运算符前后的数据类型相同

+，-，/，*，// (floor div return int(result))，% (mod return remainder)

# 比较运算符

==(检查左=右)，！=(≦)，< =, > =，>，<

**逻辑顺序**

1.  不
2.  和
3.  或者

# 方法

方法是属于一个对象的函数。python 中的一切都是对象 x =“hello”是 string 类型的对象。ObjectName 调用一个方法。方法名()

**字符串方法**

。upper()(假设 x= "hello "那么 x.upper()会给 x= "HELLO ")

。下部()

。大写()

。count()(计算字符串中的字母数)

。replace(x，y)(用字母 y 替换字母 x)

。strip()(删除字符串前后的空格)

# 基本数据结构

# 目录

该列表用于在单个变量中存储多个项目。
X=1，2 会给出一个变量只能存储一个值的错误，但是对于一个列表，可以存储多个值

X=[1，2，3，4]列表由[]表示

List 可以存储任何数据类型 z=[1，1.0，" 1"]它也可以存储另一个 list 也是 z=[1，2，[3，4]](这里[3，4]是作为类型 list 的 1 个元素)

**索引:**是一个值在列表中的位置。索引从 0 开始。列表中的值可以通过 ListName[Position]，X[0]=1 来访问

索引也可以从后-1，-2 开始...

```
X[-1]=4 or X[3]=4X[-2]= 3 or X[2]=3
```

## 列表法

。insert(位置，值)(X.insert(0，0)→X=[0，1，2，3，4]

。append(value)(在列表末尾插入值)

。extend(value)(如果 X.append([5，5])，那么 X=[1，2，3，4，[5，5])

。remove (Value)(从列表中删除值)

。流行指数。remove()删除值。pop()移除指定索引处的值

。clear()(清除整个列表)

。sort()(按顺序排列列表)

。reverse()(反转列表 X.reverse() X=[4，3，2，1])

。copy()( y.copy(X) y 将具有与 X 相同的值)

。index()(给出列表 X 中元素的索引。index(3)将给出 2(3 的位置)，X[2]= 3

## 列表切片

```
x=[1,2,3,4]X[1] gives 2 which is the value at 1X[1:3] gives 2,3 X[start:end-1], so X[1:3] → X[1],X[2]X[3] will not be included as it is end-1 Similarly X[-4:-2] → X[-4], X[-3] = 1,2X[-2] will not be included X[start:stop:step]X[1,2,3,4,5,6]X[:3] 1,2X[3:] 3,4,5,6X[0:6:2] 1,3,5
```

# 元组

有索引且不可改变的集合

索引意味着元素可以被索引(可以使用索引 x[1]引用元素)

一个元组由()发起 X=(1，2，3)是一个元组，X[1]=2

如果需要进行改变，则不能从现有元组中添加或移除元素，然后创建整个新元组，可以组合元组 Y=(4，5)然后 Z=X+Y Z=(1，2，3，4，5)

元组只有两个方法 count 和 index。

元组比列表快。这是因为元组的内存很小，并且存储在单个内存块中。

列表消耗内存。该列表存储在两个内存块中(固定大小和可变大小用于存储数据)。因此，创建一个列表会比较慢，因为需要访问两个内存块。

# 词典

字典用于存储(键:值)对中的值，它是索引的，可变的
字典是用{}创建的

```
Dict={’k1’:0,’k2’:1,’k3’:2}
Dict[k2]=1
```

## 字典方法

。清除()

。from keys()(遍历所有的 Dict.fromkeys(x，y) X=k1，k2，k3，y=1，2，3

。get(key)(与 dict[key]相同)

。items()(以元组形式返回一个包含键、值对的列表)

。钥匙(列出所有钥匙)

。爆音(键)

。popitem()(删除最后一个键，值对)

# 一组

集合是一组无序的没有重复的数据。

不存储重复值

创建{}或设置()

集合可以比列表更快地被迭代，因为集合实现了散列数据结构

## 永久变形测定法

。添加(X)

。清除()

。复制()

。difference()(返回包含差值的集合)

。difference_update()(不返回新的集合，而是更新原来的集合)

。丢弃(x)

。交集(X，y)

。交叉点.更新()

。isdisjoint()

。issubset()

。issuperset()

Symmetric _ diffrence()(并集交集)

。联合()

# 排列

python 没有内置数组 can，但是可以通过 python 包 NumPy(数值 Python)实现

数组和列表的区别

1.  列表和数组都可以被索引和迭代。主要区别在于对它们执行的操作，例如，Array=[2，4，6]/2 →[1，2，3 ], List =[2，4，6]/2 → error(数组可以直接处理算术运算)
2.  数组需要导入库进行声明。然而，该列表不需要库来声明，因为它是 python 语法的一部分。
3.  数组存储相同的数据类型，而列表可以存储不同的数据类型
4.  对于较长的数据项序列，数组是首选的
5.  该数组在添加或删除时速度较慢(删除一个元素时，该元素左侧的整个元素需要右移)(添加速度也较慢，因为当数组达到最大容量(分配的内存)时，会创建一个新数组并复制旧值)
6.  需要执行一个循环来打印一个数组中的所有元素，而一个列表不需要循环来打印所有元素
7.  如果元素被频繁访问，则首选数组
8.  数组在内存大小上更紧凑，而列表消耗更大的内存来更容易添加元素。

# 条件语句

条件语句根据特定布尔约束(条件)的值是真还是假来执行不同的计算或操作

三个条件语句 if、for、while

## 如果

if 语句用于决策操作。它包含一段代码，只有当 if 语句中给定的条件为真时才会运行。如果条件为假，则运行可选的 else 语句，其中包含 else 语句的一些代码。

```
if (if the conditional statment 1 is correct):
  Run this block of code
elif (or if the conditional statment 2 is correct):
  Run this block of code
**else**:
  Run this block of code if none of the statemet is correctNested Statement: multiple if statement can be looped.
if (  ):
  if (  ):
    code
  elif (  ):
    code
elif ( ):
  if (  ):
    code
  elif (  ):
    codeif (1>2):
  print("Yes")
else:
  print("No")Output→No
```

## 在…期间

While Loops 用于重复执行一段代码，直到满足给定的条件。当条件失败时，循环退出。

```
while (condition is true):
  Run this codeNested Statement: multiple while statement can be looped.
while (condition is true):
  Run this code
  while (condition is true):
    Run this codei=0
while(i<5):
  print("i")Output→ i i i i i
after every interation i gets incremeted after first iteration i=1, next i=2 till i=5 where the codition is not satisfied hence doesnt go into the code block
```

## 为

for 循环用于迭代序列，例如列表、字符串或数组。

```
for x in [1,2,3,4]:
  print(x+1)2
3
4
5for i in range(len(list))  if len of list 4 then range(0,3(n-1))so i=0,1,2,3
```

## 功能

只有被调用时才运行的代码块

```
def function_name(parameters/Argument):
  block of code  ( this block will not run unless called)function_name(parameters) (calling the function)Output execcutes the block of code under the functiondef add1(x):
  print(x+1)  the parameter x is like place holder it will take the       
              value given
add1(5)      calling, x=5
op=6
```

位置参数:它们在函数定义中的位置可以调用的参数

关键字参数:可以通过名称或关键字调用的参数

```
positional argument:
def add(x,y):     add(4,5) x=4,y=5
  print(x+y)      add(5,4) x=5,y=4
argument called based on their positionKeyword argument: 
def add(x,y):     add(x=4,y=5) x=4,y=5
  print(x+y)      add(y=5,x=4) x=4,y=5
based on the keyword the arguments are assigned 
```

当不知道要传递的参数或关键字参数的确切数量时，则使用*和**。*arg 的数据类型是元组，**kwargs 的数据类型是字典

```
def func(*args,**kwargs)
this way any no of argument and keyword argument cant be sent to the function
```

## 解除…的负担

```
* , ** is also used to unpack
* for list
** for dictionary
print([1,2,3]) → [1,2,3]
print(*[1,2,3]) → 1,2,3{"x":1,"y":2} 
def add(x,y):
  print(x+y)add({"x":1,"y":2}) erroradd(**{"x":1,"y":2})  unpacks x=1,y=2 then add(x=1,y=2)
```

# λ函数

lambda 函数是一个匿名函数。仅对于单行表达式，编写代码比普通函数更快

```
x=labda x:x+5
print(x(10))    op=15even=labda x: x%2==0
print(even(12)) op=Truex= lambda x,y : x+y
print(x(2,4))   op=6x=lamda x, y, z:x+y+z
print(x(1,2,3)) op=6
```

# 地图

将函数映射到 iterable 中的所有元素

映射(函数，可迭代)

```
a=[1,2,3,4,5]
l=list(map(lambda x:x+1, a))
op=[2, 3, 4, 5, 6]for i in a:
print(i+1)
op=
2
3
4
5
6
```

# 过滤器

与 map 函数相同，但它不显示所有输出，而是只显示返回 true 的输出。

过滤器(函数，可迭代)

# 列表理解

它提供了一种创建列表的简洁方法

```
x=[x for x in range(5)]  →[0, 1, 2, 3, 4]
x=[x+1 for x in range(5)]→[1, 2, 3, 4, 5]
x=[x*1 for x in range(5)]→[0, 2, 4, 6, 8]
x=[x for x in range(5) for x in range(3)]→
[0, 1, 2, 0, 1, 2, 0, 1, 2, 0, 1, 2, 0, 1, 2]x=[x for x in range(2) for x in range(3)]→[0, 1, 2, 0, 1, 2]
x=[x for x in range(100) if x%5==0]→
[0, 5, 10, 15, 20, 25, 30, 35, 40, 45, 50, 55, 60, 65, 70, 75, 80, 85, 90, 95]
```

# 词典理解

```
a=[1,2,3,4,5]
b=[1,4,9,16,25]Dict={}
for a,b in zip(a,b):
  Dict[a]=b{1: 1, 2: 4, 3: 9, 4: 16, 5: 25}
```

# 可迭代程序、迭代器和生成器

可迭代对象是可以迭代的对象，例如，当列表被初始化时。所有的元素都存储在存储器中

迭代器是一种对象，用于使用 next()对可迭代对象进行迭代，

Iter()将 iterable 转换为迭代器

```
for i in ["a","b","c"]:
  print(i)output
a
b
c
--------------------------------------------------------------------iter_alp = iter(["a","b","c"])
print(next(iter_alp))
print(next(iter_alp))
print(next(iter_alp))output
a
b
c
--------------------------------------------------------------------x=["a","b","c"]
print(x)output
["a","b","c"]
```

当调用 for loop 时，它在对象上应用 iter()，然后调用 next()方法在对象上循环。当一个列表被初始化时，所有的元素都被存储在内存中，而对于迭代器，只有当 next()被调用时，下一个元素才在内存中被初始化。

```
square number
def square_number(nums):
  result=[]
  for i in nums:
    result.append(i*i)
  return resultoutput=[4,9,16,25]
--------------------------------------------------------------------**Generator**
x=def square_number(nums):
   for i in nums:
    yield(i*i)print(next(x))
4
print(next(x))
9
```

生成器用于创建迭代器函数。它是一种特殊类型的函数，不返回值，但返回一个迭代器。这里，项目在需要时被初始化，因此与正常功能相比节省了内存

return 语句终止程序并返回最终值。Yield 可以获得局部变量，只要调用 next()就不会在内存中启动

1.  迭代器使用 iter()语法不需要函数，生成器使用 yield 函数
2.  构建一个迭代器需要做很多工作。(用 iter()和 next()实现一个类，跟踪内部状态，并引发 stop)

生成器是创建迭代器的一种简单方法。上述所有问题都是自动处理的

简而言之，生成器是一个返回对象的函数，它是一个迭代器，可以遍历对象

生成器表达式类似于列表理解

```
generator = (num ** 2 for num in range(10))
next(generator)
0
next(generator)
1
next(generator)
4generator = [num ** 2 for num in range(10)]
generator
[0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
```

感谢您的阅读