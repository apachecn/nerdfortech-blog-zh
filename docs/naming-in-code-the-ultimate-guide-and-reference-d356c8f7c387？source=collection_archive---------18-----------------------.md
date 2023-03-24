# 命名(用代码)——终极指南和参考

> 原文：<https://medium.com/nerd-for-tech/naming-in-code-the-ultimate-guide-and-reference-d356c8f7c387?source=collection_archive---------18----------------------->

![](img/006cb93addb2e41a9f221a814f173a0e.png)

[编程原则](https://programmingduck.com/articles/programming-principles)告诉我们代码中的命名很重要。本文旨在成为一个完整的代码命名学习指南。它还旨在成为一个关于命名的参考，如果你将来需要的话可以参考。

对于某些事情，比如命名方法，有不同的命名约定。我们将提到一些，以便您了解它们是如何工作的，以及您可以从中选择的选项。

总的来说，我们将检查:

1.  想要好名字的动机
2.  所有代码的一般命名提示
3.  为变量和类等特定事物建立命名约定

# 好名字的动机

好名字的动机来自于[干净的代码和编程原则](https://programmingduck.com/articles/programming-principles)。代码应该是:

*   显而易见，易于理解
*   容易改变

易于理解很有帮助，因为:

*   容易理解的代码理解起来会更快。这意味着你可以更快地工作。你将花更少的时间去理解旧代码，花更多的时间去写新代码。
*   代码将有可能被理解。相比之下，如果一些代码很难理解，即使花很长时间阅读，你也可能看不懂。经验不足的人可能运气更差。此外，你可能会误解代码是如何工作的，尤其是如果你那天没有全神贯注的话。误解代码非常容易产生 bug。

好名字对这两种情况都有帮助。

# 好名字如何让代码更容易理解

如果一件事名副其实，那么你不需要更进一步的细节来理解它。这可以节省您的时间。

例如，考虑:

*   名为`printHelloToTheScreen`的函数
*   名为`multiplyBy2`的函数
*   一个名叫`PI`的常数
*   名为`MAXIMUM_ALLOWED_LOGIN_ATTEMPTS`的常数
*   名为`Math`的静态类或模块
*   一个名为`circumference`的变量
*   一个名为`userInfo`的变量

在代码库中，你可以从名字中很好地了解它们的功能。

# 好名字如何让代码更容易理解

阅读代码时，您必须:

1.  读取(解析)它做了什么
2.  理解它为什么这样做，或者更确切地说，从概念上理解它试图做什么

例如，认为“这段代码删除了字符串中的初始空格，然后它发送了一个网络请求”是不够的。相反，你必须理解“这个代码格式化用户名，然后发送一个密码重置请求”。

下面是一个难以理解的代码示例:

```
function foo(a, b) {
  const c = PI * a ** 2 * 8 / b;
  return c > 1 ? c : 1;
}
```

那里的`8`是干什么用的？什么是`b`？为什么它返回最小的`1`？这个函数的目的是什么？

如果你想做一些事情，比如改变面积，你不知道这个函数是否相关。即使你怀疑它是，你也不知道它是做什么的，也不知道为什么。

类似这样的事情会好得多:

```
function calculateRemainingArea(radius, reductionFactor) {
  const area = PI * radius ** 2;
  const remainingArea = area * AREA_REDUCTION_RESISTANCE / reductionFactor;
  return Math.min(remainingArea, MINIMUM_AREA);
}
```

所以，好名字是有帮助的，因为**它们提供了意义**。它们帮助您理解代码做什么以及为什么要这样做。

为了得到好处，你所要做的就是给这个东西起一个好名字。

再比如，可能有人不理解`PI * a ** 2`。他们可能认为“`PI`、`a`、2 的力量...那是什么鬼东西？?"。为了使它更简单，您所要做的就是用`const circleArea = PI * radius ** 2`替换那一行。很有帮助。

![](img/80fe81d8fe19d7d9bfbad4bfc0c2bc75.png)

# 代码的一般命名提示

简而言之，一个好名字是能立刻告诉你某事是什么或做什么的东西。这并不奇怪。理解它并不需要努力。你不用想就明白了。

坏名字是你读到的东西，你想知道“那是什么”，或者“那有什么用？”。需要你多思考的事情。

当写名字时，他们应该能被以前从未见过这些代码的人理解。它们需要有意义，前后一致，并有足够的描述性。

这里有一些实现这一点的方法。

# 有明确的命名约定

你的代码库应该有清晰的约定。这大概是本文最重要的一点。如果你已经在遵循一个惯例，那么最好坚持下去，即使有一个替代方案可能更具描述性。

惯例适用于一致性。[编程原则](https://programmingduck.com/articles/programming-principles)告诉我们，一致性非常重要。

它能让你工作得更快。您可以对代码如何工作以及某些事情的含义做出某些假设。此外，在考虑某个事物的名称时，公约可能已经有了相应的规则。这使得事情变得更容易。

此外，不遵循惯例会导致错误。您可能会认为代码库中遵循了一个通用的约定。如果你错了，那么你可能对一些代码的工作原理有错误的理解。在这种情况下，很容易产生 bug。

理想情况下，您应该遵循您的编程语言或框架中已经存在的约定。这对公司的新开发人员来说更容易。一些例子是[。NET 命名指南](https://docs.microsoft.com/en-us/dotnet/standard/design-guidelines/naming-guidelines)和 [Airbnb JavaScript 风格指南](https://github.com/airbnb/javascript)。否则，您也可以为您的项目或公司创建自己的自定义约定。

# 偏好描述性名称

总是在考虑，下一个看这个名字的人会不会很容易理解？

## 使用完整的单词

一般全字比较好理解。例如`calculateArea`或`createWindow`。缩写词可能更难理解。一般来说，避免使用像`calcArea`或`cWin`这样的名字。

## 避免非常短的变量名

特别是，避免单个字母或非常短的变量，如`d`。它可能意味着任何事情:T5、T6、T7 或 T8。

例外情况是，当名称在非常小的范围内使用，并且它所代表的内容很明显时。在这种情况下，很容易向上看一两行，看看它是在哪里定义的。下面是一个使用变量`e`的代码示例。

```
function sendData() { /* sends a network request */ }
function handleFormSubmission(e) {
  const formElement = e.target;
  const formData = FormData(formElement);
  sendData(formData);
}
```

这里，变量`e`代表事件对象。我们知道这一点，因为名称`handleFormSubmission`和函数签名代表一个事件处理程序。`e`只在它被定义的地方下一行使用，所以仍然很容易使用。

不过，我个人还是会用`event`。我认为可读性比像这样节省一点点按键更重要。

另一个可接受的例子是在 for 循环等中使用`i`。那是每个人都明白的惯例。

## 首字母缩略词

如果一个缩略词很常见，那么就可以使用它。一些例子是缩写 HTML，UI，IO，OS，JSON，XML，HTTP。

如果首字母缩写不常见，那么最好使用全称。如果有疑问，你应该使用完整版。

# 简洁很重要

首先是具有充分描述性的名称。然而，这并不意味着你需要超长的名字。

超长的名字，比如一个名为`integerNumberOfEnginesInObject`的变量很难处理。`engineCount`就够了。

一般来说，如果你可以用一个更短的名字来表达同样的意思，那就用更短的名字。

# 考虑一下背景

周围上下文的名称可以为某事的目的提供有用的线索。这意味着，有时，您可以使用较短的名称。

例如，如果您有一个名为`Users`的类，那么创建新用户的方法可以称为`create`。它的用法类似于`users.create()`(其中`users`是`Users`的一个实例)。这已经足够说明问题了。这种情况下不需要调用方法`createUser`。

# 套

编程中流行的大小写(不包括 HTML 和 CSS)有 pascal 大小写、camel 大小写和 snake 大小写。您使用哪一种取决于您的编程语言或框架的约定。

## 每个外壳的一般格式

蛇形大写是小写的，使用下划线来分隔单词。比如`this_is_snake_case`。

在 pascal 大小写中，每个单词都以大写字母开头。比如`ThisIsPascalCase`。

Camel 大小写类似于 pascal 大小写，只是第一个单词以小写字母开头。比如`thisIsCamelCase`。

## 首字母缩写词的大小写

对于首字母缩写词，约定各不相同。

在前端，缩略词似乎总是完全大写，不管长度如何。一些例子是`performance.toJSON()`、`XMLDocument`、`HTMLCollection`和`DOMString`。

但是，在其他一些语言中，如。NET 语言，约定是:

*   如果首字母缩略词只有两个字母，第二个字母的大小写应该与第一个字母相同(大写或小写)。比如`UIInput`(帕斯卡案)`uiInput`(骆驼案)`fooUIInput`。
*   如果首字母缩略词是三个字母或更长，必要时只需大写第一个字母。比如`JsonFoo`(帕斯卡案)`jsonFoo`(骆驼案)。

## 复合词的大写

从大写的角度来看，将复合词视为一个词是一种惯例。比如用`callback`和`endpoint`代替`callBack`和`endPoint`。您可以在[上找到编程中使用的常见复合词的完整列表。资本化的网络命名指南](https://docs.microsoft.com/en-us/dotnet/standard/design-guidelines/capitalization-conventions)。

# 偏好语义命名

语义命名是指以事物的目的或意义来命名事物。按照优先顺序，这意味着:

1.  它的目的是什么，或者它有什么作用
2.  它是如何做到的

作为一个额外的好处，它也导致代码在未来不太可能改变。

给事物命名时，考虑:你要命名的事物最重要的是什么？最适合别人从概念上理解的名字是什么？

通常，用户更关心某个东西在做什么，而不是它是如何做的。虽然，有时候,“如何”才是最重要的。

这里有一些语义命名的例子。

## 带有变量名的示例

如果您有一个保存用户集合的变量，重要的是它包含用户。是列表还是地图就不那么重要了。更不用说您的 IDE 和类型声明提供了这些信息。

因此，名曰:

*   `users`会比较合适
*   `userList`不太可取。“列表”部分没有`users`部分重要。此外，如果您在将来更改数据结构，您将不得不更新变量名。
*   `userCollection`也不太可取，因为“收藏”部分不如“用户”部分重要。但是，如果您将来更改数据结构，至少您不必更新变量名。

## 接口和实现示例

在 OOP 代码中，你倾向于拥有可能有多个实现的接口。对于一个排序算法，一个接口可能被称为语义和通用的东西，比如`Sorter`。这就是它的重要之处，它可以排序。如何在接口层面上并不重要。

不同的实现可以根据它们实现的排序算法来调用。这是他们的重要之处，也是他们之间唯一的区别。这是特定类别的用户想要的信息。比如`BubbleSort`、`MergeSort`、`Quicksort`。

## 排序方法示例

假设您有一个包含某个集合的类。你也有一个排序的方法，比如说，按字母顺序。

如果你只有一种排序方法，那么最好将其命名为`sort`。像`sortWithQuicksort`或`quicksort`这样的名字是用户不关心的不必要的信息。你想想，代码的调用者想要排序。他们对你的类使用的特定算法并不特别感兴趣。(唯一的例外是，如果您的类确实是性能瓶颈之类的，但那是另一个话题。)

此外，将来您可以更改该类用于合并排序的排序算法。那样的话，`sortWithQuicksort`这个名字就没有意义了。

解决方案是将你的公共方法命名为`sort`。

你使用快速排序的事实仍然很重要。致力于实现该类的开发人员会想知道这一点。一些选项是拥有一个名为`quicksort`的私有方法，或者从代码库中的其他地方导入并使用一个`quicksort`函数。

例如:

```
// pseudocode

class Users
{
  // the public method with a name that's useful to callers
  public User[] Sort()
  {
    return _quicksort(this.users);
  }

  // the private method with a name that's userful to someone working on this class
  private T[] _quicksort<T>(T[] items)
  {
    /* implementation of quicksort */
  }
}
```

## 前端 CSS 示例

在前端 CSS 中，有几种方法。您选择的方法决定了类名的重要性。

使用 BEM 命名约定时，重要的是元素的用途，而不是样式。例如，你可以使用 CSS 类`page-heading`而不是`large-heading-red`。这是因为，明天，页面标题的样式可能会改变。此时，非语义名称不再有意义，但是语义名称有意义。

如果您使用的是 UI 组件库，那么组件的样式比组件的用途更重要。例如用`button-primary`代替`add-to-cart-button`。这是因为像`button-primary`这样的类是你将在整个代码中使用的主要类。

# 鲍勃叔叔的其他建议

鲍勃叔叔的书《干净的代码》中提到的一些技巧是:

*   进行有意义的区分。避免看起来相似或相同的变量，如`accountInfo`或`accountData`。使用该代码的用户将无法分辨其中的区别。
*   使用容易发音的名字。名字应该易于大声读出。一些要避免的例子是像`genymdhms`和`modymdhms`这样的名字。更好的名字应该是`generationTimestamp`和`modificationTimestamp`。
*   使用可搜索的名称。名称应该易于使用代码编辑器进行搜索。本质上，避免一两个字母长的名字，因为搜索它们会返回太多匹配。
*   避免使用可爱/攻击性的词语。避免使用`whack()`和`seeYouLaterFiles()`这样的名字。要专业，要一致。
*   为每个概念选择一个单词。例如，不要在你的代码库中混合使用“获取”、“获取”和“检索”这三个词来表示同一种操作。选择其中一个，坚持使用。
*   避免双关语。避免用同一个词表达不同的概念。例如，如果在一个类中`add()`添加两个数字，在另一个类中`add()`不应该插入到一个列表中。

# 的其他提示。网络命名约定

。NET 提出了一些额外的建议。有些是专门针对。网络语言。然而，无论如何你都要记住它们。

建议如下:

*   更喜欢自然易读的名字。比如用`HorizontalAlignment`代替`AlignmentHorizontal`。
*   避免使用与其他编程语言中常见关键字冲突的名称
*   更喜欢语义名称，而不是特定语言的名称。比如用`GetLength`代替`GetInt`。
*   首选通用 CLR 类型名称，而不是特定于语言的名称。例如命名一个方法`ToInt64`而不是`ToLong`。这是因为像这样的方法可能在其他 CLR 兼容语言中使用，而数据类型`long`并不存在。因此，该名称在那些语言中没有意义。然而，`Int64`是存在的，对所有 CLR 语言都有意义。

![](img/a8e4b84a236d24d5d588cbc220ce0893.png)

# 特定用例的命名约定

以下是一些常见的命名惯例，如变量、函数、类等。

# 变量

变量只是值和对象的名称或标签。它们的一些通用惯例是:

对于保存布尔值的常量和变量，规则稍有改变。

## 常数

一些编程语言写常量完全大写，并带有蛇的大小写。这包括 JavaScript、Python 和 Java。一些示例常数是:

在这里，“常数”指的是特殊值。这些值不依赖于运行时。它们可以很容易地放在配置文件中，远离您的应用程序代码。

“常量”不是指一个普通的局部变量，只是碰巧是不可变的。这些变量遵循与正态变量相同的规则。

以下是一些“常量”和“恰好是不可变的正常变量”的例子:

*   `final MAX_ATTEMPS_BEFORE_LOCKOUT = 5`(这可能在配置文件中，它是一个特殊的常量)
*   `final float result = SomeClass.factorial(someNumber)`(这只是一个碰巧不可变的局部变量。它根据`someNumber`在运行时变化。它不能放在配置文件中)
*   `const URL_ENDPOINT = '/ajax/foo'`(这可能在配置文件中，它是一个特殊的常量)
*   `const filteredArray = array.filter(a => !a)`(这只是一个不可变的局部变量。它在运行时根据`array`而变化。它不能放在配置文件中)

## 布尔运算

对于具有布尔值的变量，约定是将它们表述为一个问题。以谓语开始，如“是”、“已经”、“曾经”和“可以”。这清楚地表明该变量包含一个布尔值。

布尔变量的一些例子是:

除了可以独立阅读之外，它们在条件语句中也很容易阅读:

```
if (hasLoaded) {
  doSomething();
} else {
  doSomethingElse();
}
```

相比之下，如果你不使用谓词，你可以使用一个名字，比如`complete`。这既是动词又是形容词。它可以表示许多事情。它可以是一个运行来完成某件事情的函数，或者是一个保存已经完成的事情的变量，或者是一个表示某件事情是否已经完成的布尔值，或者是一个事件名称。它所代表的含义更加模糊，所以更倾向于使用谓词。

像`completing`这样的动词稍微好一点。它不能表示功能，因为“完成”中的“ing”意味着某事已经发生。它不是你现在就可以开始运行的东西(比如函数调用)。然而，它仍然可以是任何其他选项。总的来说，使用谓词仍然更好。

# 功能

函数是做某些事情的代码单元。函数名的约定是:

*   它们应该是动词
*   他们通常使用骆驼大小写或蛇大小写，这取决于你的编程语言

一些示例函数名称如下:

对于返回布尔值的函数，常见的约定是从谓词开始。这类似于包含布尔值的变量。

返回布尔值的函数的一些示例名称如下:

我见过的另一个约定是“变压器”或“转换器”功能。这些是从一件事物转换到另一件事物的功能。它们通常以“to”开头，后面是它们要转换成的类型。

转换器函数名称的一些示例如下:

# 班级

类是包含方法和属性的代码单元。

类的一些约定是:

*   他们使用帕斯卡格
*   它们应该是名词(或名词短语)

一些示例类名如下:

## 常规属性名称

一般来说，属性的命名类似于变量。方法的命名类似于函数。

然而，如前所述，类名提供了一些上下文。

这意味着你可以为一个方法/属性使用一个比等价的函数/变量更简单的名字。

例如，在一个类`Users`中，你可能有一个名为`create`的方法。它的用法类似于`users.create()`(其中`users`是`Users`的一个实例)，这就足够说明问题了。

## 属性的套管和前缀

在大小写和前缀方面，不同的语言和框架有不同的约定。

例如， [Java 命名约定](https://www.oracle.com/java/technologies/javase/codeconventions-namingconventions.html)提到方法和属性应该是骆驼大小写的。

[PEP 8 Python 命名约定](https://www.python.org/dev/peps/pep-0008/)也提到了骆驼大小写。但是，它补充说私有属性应该以下划线为前缀。比如(`_privateProperty`)。

C#编码惯例似乎是最严格的。他们指出:

*   公共属性应该是 pascal 大小写的(例如`PublicMethod`或`PublicVariable`)
*   私有或内部属性应该以下划线为前缀并区分大小写(例如`private void _privateMethod`)
*   私有或内部静态属性应该以`s_`为前缀，并采用骆驼格式(例如`private static foo s_workerQueue`)
*   私有或内部线程属性应该以`t_`为前缀，并采用骆驼格式(例如`private static foo t_timeSpan`

无论出于什么原因，如果你正在创建自己的惯例，我个人建议:

*   方法使用 pascal 大小写，属性使用 camel 大小写(这是 Unity 游戏引擎中使用的约定)。这样做的原因是为了区分布尔属性(如`isValid`)和返回布尔的方法(如`IsValid(data)`)。作为第二种选择，我将使用 camel case 作为方法。
*   非公共属性的下划线前缀
*   可能是(还不确定)来自 C#的`s`和`t`前缀。正如 Python 的 [Zen 所说的“名称空间是一个非常棒的想法——让我们做更多这样的事情吧！”(我认为前缀也有类似的效果)](https://www.python.org/dev/peps/pep-0020/)

# 接口

接口的名称类似于类名。它们使用帕斯卡格，通常是名词。有时它们可以用一个形容词来命名，例如“可读的”。

在《清洁代码》一书中，鲍勃大叔建议避免使用`I`前缀作为接口。Java 命名约定[推荐相同的方式。比如`interface Foo`。](https://www.oracle.com/java/technologies/javase/codeconventions-namingconventions.html)

C#编码惯例建议在接口前加上 I .前缀，例如`interface IFoo`。

就我个人而言，我倾向于避免使用前缀。这是因为，作为代码的用户，我对我使用的是接口还是类并不特别感兴趣。

# 枚举

Java 惯例是将枚举视为具有常数的类。这意味着用 pascal 大小写和全部大写的字段来命名枚举类型。

C#也把它们和类一样对待。这意味着使用 pascal 大小写来命名枚举类型和字段。

就个人而言，我更喜欢 Java 约定，因为它区分了常量和其他值。

# 事件、事件处理程序、消息传递系统和命令

关于事件及其相关功能，需要考虑一些事情。

## 事件名称

事件名称:

*   提及一项行动
*   通常用动词来命名。例如“下载”、“加载”、“删除”、“损坏”、“移动”。
*   通常用现在时来描述在动作开始前发生的事件。例如，在提交密码重置请求之前，您可以触发一个名为“passwordResetRequestSubmitting”的事件。
*   通常使用过去式来描述动作完成后触发的事件。例如，在提交密码重置请求后，您可能会触发一个名为“passwordResetRequestSubmitted”的事件。

根据我的经验，这些约定在许多语言中都很常见。然而，没有太多关于它们的官方文档。一个例外是[。在那里他们正式陈述了这些指导方针](https://docs.microsoft.com/en-us/dotnet/standard/design-guidelines/names-of-type-members)。

对于事件的实际名称，您可以使用任何对您有意义的名称。如果您的事件是在一个类中创建的，那么类名可以提供足够的上下文。例如，如果您有一个类`Shape`，您可能有一个名为`areaChanged`的事件。用法类似于`myShape.areaChanged += someHandler`或`myShape.areaChanged.subscribe(someHandler)`。在这种情况下，事件名“areaChanged”就足够说明问题了。

就大小写而言，遵循编程语言中的约定。C#对公共成员(包括事件)使用 pascal 大小写。大多数其他语言使用骆驼大小写。

一些事件名称示例如下:

*   文件下载/文件下载
*   用户注册/用户注册
*   uielementxupdate/uiElementXUpdated

## 事件处理程序

对于事件处理函数，有一些约定。在前端， [React 倾向于使用前缀“handle”](https://reactjs.org/docs/handling-events.html)。C#推荐后缀“EventHandler”。其他语言可能有其他约定。

事件处理函数的一些示例名称如下:

*   handle user subscribed/usersubscribed handler/UserSubscribedEventHandler
*   handle file downloaded/file downloaded handler/file downloaded eventhandler

我个人倾向于使用“handle”前缀。这使函数保持动词形式，这是函数的惯例。

## “开”功能

仅仅用来引发事件的函数往往被命名为`on<EventName>`。比如“onAreaChanged”。

使用它们的一个例子是在可能被派生的类中。

下面是一个 C#示例:

```
class Foo
{
  public event EventHandler FileDownloaded;

  protected virtual void OnFileDownloaded(EventArgs e)
  {
    EventHandler handler = FileDownloaded;
    if (handler != null) {
      FileDownloaded(this, e);
    }
  }

  public void Something()
  {
    OnFileDownloaded(EventArgs.Empty);
  }
}
```

这里，我们有一个名为`OnFileDownloaded`的方法，它的唯一目的是引发事件。

这种约定在前端可能有点不同，但它仍然遵循一般的思想。例如，在 React 中，可以将道具命名为“onClick”或“onSomeEvent”。在组件内部定义的事件处理函数可以使用“handle”前缀。

下面是一个 React 示例:

```
import React from 'react';

const ParentComponent = () => {
  function handleButtonClicked() {/* do something */}
  return <ChildComponent onClick={handleButtonClicked} />;
};

const ChildComponent = ({ onClick }) => {
  const handleClick = (e) => {
    console.log("clicked");
    onClick("foo");
  };
  return <button onClick={handleClick}>Click me</button>;
};
```

这里，每个组件都创建以“handle”为前缀的事件处理函数。子组件有一个名为“onClick”的属性。在子组件中，`handleClick`函数调用`onClick`属性。

## 更多全局消息名称

PubSub(消息总线)和分析与本地事件有相似之处，但它们更具全球性。它们跨越了更大范围的代码库。甚至可以跨越多个应用程序(分析可能就是这种情况)。

有了这些，使用更具体的名称就很重要了，否则你就不知道他们指的是什么了。

一个好的方法是使用名称空间和前缀，以及指定的分隔符。

例如，使用 PubSub 事件名称，您可以为代码库的相关区域指定一个名称空间。事件名称的格式可以是`<areaOfCodebase>/<eventName>`。例如:

*   ui/主题已更改
*   ui/font size 已更改
*   播放器/损坏
*   播放器/电源关闭
*   用户/注册
*   用户/注册
*   用户/已删除

正如大卫·威尔斯在[的《干净的分析》中所解释的那样](https://davidwells.io/blog/clean-analytics)，对于分析，你可以使用像`<Source>:<object>_<actionName>`这样的格式。例如:

*   站点:时事通讯 _ 已订阅
*   app:site_deployed
*   cli:用户登录
*   api:站点 _ 已创建

这些只是例子。在您自己的代码库中，您可以使用任意多的名称空间。名称空间可以是任何东西。

同样，分隔符可以是您想要的任何东西。例如“/”、“::”、“:”、“_”，甚至没有分隔符。

## 命令

命令的编写类似于函数。它们是祈使语气的动词。它们也在 PubSub 中使用。上面提到的关于名称空间和分隔符的注释适用于它们。

命令通常也需要响应。换句话说，像“CreateUser”这样的命令会有“CreateUserResult”、“CreateUserSucceeded”甚至“UserCreatedNotification”这样的响应消息。总的来说，我不知道这些有什么强约定，所以你可以使用你喜欢的任何东西。

我个人对响应名称的偏好来自于 Jimmy Bogard 关于消息命名约定的文章。我通常在原来的命令名后面加上“Result”、“Reply”或“Response”。

命令名及其名称空间的一些示例格式是`<Verb><Subject>`和`<subject>/<verb>`。例如:

*   可能的命令是“注册用户”或“用户/注册”。可能的响应是“注册用户响应”或“用户/注册结果”
*   可能的命令有“损坏播放器”、“播放器/损坏”。可能的响应是“损坏玩家响应”、“玩家/损坏 _ 结果”

# 文件名

对于文件名，您需要考虑大小写的约定，以及根据文件包含的代码来命名文件。

## 文件名大小写约定和分隔符

对于文件命名，根据您遵循的语言、框架和风格指南，有不同的约定。

许多惯例建议文件名都是小写字母。单词可以用连字符(-)或下划线(_)分隔。例如:

*   在 HTML 中，约定都是小写，用连字符作为分隔符。下划线很少出现。其中一个原因是 HTML 文件名可能会反映在 URL 中(尤其是对于较旧的服务器)。URL 中的下划线远不如连字符常见。
*   在 CSS 中，约定都是小写，用连字符或下划线作为分隔符
*   在 Python 中，PEP 8 建议文件名全部小写，用下划线作为分隔符
*   Google JavaScript 风格指南建议文件名全部小写，用下划线或连字符作为分隔符

在连字符和下划线之间，你可以使用任何一个。两者都可以接受。一般来说，我更喜欢使用连字符来与我的 CSS 类保持一致(CSS 类通常使用连字符),这也是 HTML 中提到的原因。但是，如果您在编程语言中经常使用 snake case，或者如果您不编写 HTML 和 CSS，那么使用下划线而不是连字符可能会更一致。

一些文件名示例如下:

*   index.html
*   第三方分析网站
*   敌人-移动者. js

除此之外，还有其他约定推荐您的文件使用 camel 或 pascal case。例如:

*   C#和 Java 建议将你的文件命名为与你的文件中的 main 一样。这意味着使用 pascal 大小写，就像你的类和接口一样。
*   AirBnB JavaScript 风格指南建议将文件命名为与默认导出相同的名称。同样，这意味着使用 camel 或 pascal case，至少对于您的 JavaScript 文件是这样。

[React 更进一步，建议在每个文件夹中用一致的名称命名所有文件](https://reactjs.org/docs/faq-structure.html)。例如:

*   MyComponent.js
*   MyComponent.css
*   MyComponent.test.js(测试文件通常有特殊的扩展名. test.js)

那么应该选择哪个呢？首先，考虑对于您的编程语言或框架来说，是否有一种约定比其他约定更常见。那是自然的选择。否则，你可以为所欲为。我个人的建议是选择与文件中的代码最匹配的命名约定。例如，在前端工作时，惯例是 HTML 和 CSS 中的所有内容都是小写，用连字符作为分隔符。因此，您可能希望使用它作为命名约定。

作为另一个例子，当处理使用 CSS 模块的 React 应用程序时，您可能更喜欢使用 pascal 大小写和下划线来编写 CSS。这使得在 JavaScript 中使用 CSS 更加容易，例如`styles.Foo_bar`(其中`Foo_bar`是 CSS 类)，因为 JavaScript 中不允许使用连字符。在这种情况下，使用 pascal case 命名文件可能会感觉更自然。

## 根据文件名包含的代码选择文件名

通常，您希望根据文件的用途来命名文件。

这受到 C#命名约定和 Java 命名约定的支持。它们规定你的文件名应该和你在文件中定义的主要内容相同。例如，如果你定义了一个 Foo 类，你的文件名应该是 Foo.cs 或者 Foo.java。

AirBnB JavaScript 风格指南也同意这一点。它规定您的文件应该以您的默认导出命名。是否使用默认导出是另一回事。但重点是一样的。意思是以目的命名，或者以你文件中最重要的东西命名。通常(但不总是)，如果你使用导出默认值，你会这么做。

一个例外是，如果您的默认导出被命名为“main”、“setup”或类似名称。将一个文件称为“main”是没有意义的，尤其是如果许多文件都有类似的默认导出。在这种情况下，请考虑该文件的用途。另一种方法是考虑等价的 OOP 代码是什么。

例如，假设您有一个处理转盘功能的类。你的 OOP 代码可能是一个名为“Carousel”的类。相比之下，如果您使用函数编写它，它可能看起来像这样:

```
function handleChangeToNextSlide() {
  // code to change to next slide when user clicks the relevant button
}

function main() {
  // find DOM elements that are supposed to be carousels
  // set up event listeners on those elements
}

export default main;
```

在等价的 OOP 代码中，`main`中的代码应该在`Carousel`类的构造函数中。在这种情况下，我建议将文件命名为`carousel`。这才是它的真正目的。或者，你也可以把`main`改成`setupCarousel`或者别的什么，然后给这个文件命名。

至于其他一些情况:

*   如果你的文件定义了多个类:好吧，大多数风格指南会告诉你避免包含多个类的文件。考虑是否可以将每个类分离到自己的文件中。虽然，如果你只有一个公共类，那也没问题。以一个公共类命名文件，而不是私有类。
*   如果使用函数而不是类，可能会出现没有一个函数适合导出默认值的情况。同样，您需要考虑文件的用途。例如，如果您的测试文件中使用了一些函数，那么“test-utilities”可能是一个好名字。或者，您可以考虑等价的 OOP 代码是什么。你可能会有一个包含静态方法的静态类。用静态类的名字命名你的文件。

# 包名和名称空间

实际上，许多包根本不遵循命名约定。这是因为开发人员可以上传他们想要的任何名称的包。通常不会有很多支票。

但是，包和不同的包存储库有命名约定。

Java 和 [Maven](https://search.maven.org/) 使用`<groupID><artifactID>`的格式。组 ID 部分通常是反向域名。例如，如果您的域是 example.com，组 ID 将是“com.example”。它可以有子组(附加的名称空间)。例如“com.example.plugins”。artifactID 是 jar 的名称。例如“junit”或“spring-web”。一般来说，工件 id 倾向于完全小写，用连字符来分隔单词。

[NPM(节点包管理器)](https://www.npmjs.com/)倾向于使用直接的包名，比如“react-dom”，或者以名称空间或公司名称为前缀，比如“@babel/preset-env”。它们倾向于完全小写，用连字符来分隔单词。

用 [NuGet](https://www.nuget.org/) (包存储库为。NET)，名称空间的数量是变化的，就像 Maven 和子组一样。有很多套餐只有一个名字，比如“Moq”。还有很多格式`<CompanyName>.<Package>`的包，比如“AWSSDK。核心”。也有带有许多名称空间的包，比如“Microsoft。extensions . file providers . abstracts”。如果你想效仿微软发布的软件包，那么就使用 pascal case，可以选择一个公司前缀和你需要的名称空间。

名称空间(在代码中)似乎遵循类似于包的概念和约定。

# 最终提示

记住命名是困难的。

> *计算机科学只有两个硬东西:缓存失效和事物命名。—菲尔·卡尔顿*

然而，花些时间想出一个合理的名字通常是值得的。

还有，一如既往的务实。偶尔打破这些惯例也没什么。他们只是在这里帮助你在大多数时候做出好的和一致的决定。比如你觉得在名字里加上数据结构会有帮助，比如`userList`，那也没关系。由你来决定什么对你的项目最有利。

此外，你可能不能花不合理的时间想出好名字。所以，有时候，如果你已经花了太长时间，你可能需要用你目前想到的最好的名字，然后继续前进。

总的来说，从这篇文章中最重要的是你理解了命名背后的原则。特别是，这些名称应该使代码易于理解。如果你明白这一点，那么你会没事的。即使在不熟悉的情况下，你也能想出自己的解决方案和惯例。

# 最终注释

这篇文章到此为止。我希望你觉得有用。

如果你在你工作过的代码中遇到任何糟糕的名字，请留下评论！

此外，如果你想讨论什么，不同意什么，或有任何反馈，请在下面留下评论。

好吧，下次见🙂

# 图像制作者名单

*   封面照片(修改)——由 Paul Stollery 在 Unsplash 上拍摄
*   白色表面上白色记事本旁边的透明灯泡—照片由 Burak Kebapci 在 Pexels 上拍摄
*   带有白色卡片的绿叶——照片由 Helena Hertz 在 Unsplash 上拍摄

*原载于 2021 年 6 月 15 日*[*【https://programmingduck.com】*](https://programmingduck.com/articles/naming)*。*