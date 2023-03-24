# 测试 REST API 在 Go 中的验证和模拟

> 原文：<https://medium.com/nerd-for-tech/testing-rest-api-in-go-with-testify-and-mockery-c31ea2cc88f9?source=collection_archive---------2----------------------->

![](img/95c5a5037a7aef4d33f2ce20234a9869.png)

由[miko aj](https://unsplash.com/@qmikola?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

> "当低价的甜蜜被遗忘后，劣质的苦涩依然久久不散." **—本杰明·富兰克林**

让我们以一句引语开始这篇文章，当你读到这句引语时，你想到了什么？

对我来说，顾名思义，和软件测试有关。

有时，当我们写一些代码时，焦点只是尽快创建一个工作代码，而不考虑可读性、清晰性、有效性和可维护性。通过忘记这些顾虑，我们可能会有一个功能代码。我们的代码工作一段时间，直到有一些需要改变或其他开发人员介入。因为我们的代码首先是不可维护的，任何未来的改变都将是困难的。然后，如果其他开发人员介入并需要在我们的代码中更改或扩展一些功能，但我们的代码非常脏，那么我保证那个开发人员会因为这样脏的工作而对你生气。

这种灾难将持续一段时间，直到发生彻底的变化或者应用程序崩溃。我们可以防止这种灾难的许多方法之一是编写单元测试。

就像鲍勃大叔在 *Clean Code 里说的“测试代码和生产代码一样重要”。我们的测试代码需要思考、设计和细心。它必须像产品代码一样保持干净。如果我们的生产代码可以保持干净，那么同样的事情也必须应用到测试代码中。要做到这一点，在围棋中使用证明和嘲弄来学习单元测试是有好处的。*

# 介绍

![](img/2463b1352f7ed3bc618f73e1917d1a31.png)

按作者

在本文中，我们将为遵循这种架构的应用程序编写单元测试。有三个主要组件，处理程序、用例以及存储库。它们每个都有自己的任务，Handler 负责处理传入和传出的数据，Usecase 用于在与数据库联系之前和之后处理数据，Repository 是我们用来与数据库通信的一层。简单地说，处理程序依赖于用例，用例依赖于存储库来工作。因此，因为我们想写单元测试而不是集成测试，所以我们需要嘲讽，我们将在这里学习。

免责声明:本文将涵盖概念，但为了便于阅读，忽略了一些细节。这里可以看到完整的代码[https://github . com/agusrichard/go-workbook/tree/master/restapi-test-app](https://github.com/agusrichard/go-workbook/tree/master/restapi-test-app)。

有三个工具我经常使用，它们是 Assert、Suite 和 Mock。先说 Assert。

# 维护

Assert 将是我们可以用于单元测试的最小的工具。如果你有一些使用 Go 内置包`testing`的经验，很多时候，我们要写 If 语句来断言我们的实际输出是否正确。但是借助于 testify，我们可以直接断言实际的输出，而不需要编写 if 语句。

使用 assert 的一般模式如下:

```
func TestSomething(t *testing.T) {
   // define expected and actual
   assert.Equal(t, expected, actual, "some message about this test")
}
```

或者，如果您不想在每次使用时都将`t`置为 assert，您可以这样编写测试。

```
func TestSomething(t *testing.T) {
   // define expected and actual
   assert := assert.New(t)
   assert.Equal(expected, actual, "some message about this test")
}
```

请注意，还有其他可用的断言，如 Error、False、NotEqual、NoError、Nil 等。你可以在 https://pkg.go.dev/github.com/stretchr/testify/assert[的官方文件中得到完整的名单。](https://pkg.go.dev/github.com/stretchr/testify/assert)

# 套房

大多数时候，当运行测试时，我们需要一些设置和拆卸功能。例如，当我们需要在运行所有测试用例之前建立一个连接，并需要清除数据库中所有注入的数据以开始一个新的会话时，为此，evidence 为我们提供了一个套件。使用 Suite，我们可以对相关的测试用例进行分组，在所有测试或单个测试之前进行一些设置，在所有测试或单个测试之后进行一些拆卸。

使用 Suite 的一般模式如下。

```
type MySuite struct {
   suite.Suite
}

func (m *MySuite) SetupSuite() {
   //... do some setup
   // this function runs only once before all tests in a suite
}

func (m *MySuite) TearDownSuite() {
   //... do some teardown
   // this function runs only once after all tests in a suite
}

func (m *MySuite) SetupTest() {
   //... do some setup
   // this function runs before each test
}

func (m *MySuite) TearDownTest() {
   //... do some teardown
   // this function runs after each test
}func (m *MySuite) TestOne() {
   // we can write our test here
}

func TestMySuite(t *testing.T) {
   /// we still need this to run all tests in our suite
   suite.Run(t, new(MySuite))
}
```

# 模拟的

第三个是 Mock。有时候，我们希望孤立地测试我们的功能。但是为了让我们的功能发挥作用，我们需要一些外部依赖。例如，在某个函数中，有一个对外部资源的 API 调用，如果我们在测试我们的函数时不想进行 API 调用，该怎么办？那么，我们如何独立地测试我们的功能，而不依赖于其他外部依赖呢？这就是我们需要嘲笑的时候。

这是用法示例:

```
// main.gotype MyStruct struct {
   // some properties if needed 
}

type MyStructInterface struct {
   DoAmazingStuff(input int) (bool, error)
}func (m *MyStruct) DoAmazingStuff(input int) (bool, error) {
   // do something amazing in here    
}func AmazingFunction(m MyStructInterface) {
   m.DoAmazingStuff(123)
} // main_test.gotype MyStructMocked struct {
   mock.Mock
}

func (m *MyStructMocked) DoAmazingStuff(input int) (bool, error) {
   // this is a mocked process
   args := m.Called(input)

   // return the values based on the specified types   
   return args.Bool(0), args.Error(1)
}func TestAmazingFunction(t *testing.T) {
   // create a mocked instance
   testObj := new(MyStructMocked)

   // DoAmazingStuff will be called when AmazingFunction runs
   // therefore you need to specify the arguments (the second 
   // position of method On) and its return values
   testObj.On("DoAmazingStuff", 123).Return(true, nil)

   AmazingFunction(testObj)
   testObj.AssertExpectations(t)
}
```

在这里，我们定义了我的结构。这是我们使用 AmazingFunction 需要的依赖项，因为我们必须向它传递 MyStruct 的一个实例。然后，我们定义了 MyStruct 的模拟版本，即 MyStructMocked。请注意，要使用 AmazingFunction，我们需要传递一个实现 MyStructInterface 的结构，因此，如果我们想将 MyStructMocked 的一个实例传递给 AmazingFunction，MyStructMocked 需要实现该接口。幸运的是，在 Go 中，实现一个接口我们不需要显式指定，因为在 Go 中接口是隐式实现的，而不是显式实现的。我们要做的只是创建与指定接口具有相同签名的相同方法。

# 应用程序的实际使用情况

好的，之前我们已经学习了作证的基本用法。现在，我们如何在现实生活中使用它？

让我们从存储库开始。在这种情况下，如果想要获取、插入、更新或删除记录，存储库需要一个数据库连接。我们不会模拟数据库连接，我们将使用真实的数据库。为了简单起见，我将给出一个创建事务的例子(并忽略一些细节)。你可以在这里寻找完整的代码[https://github . com/agusrichard/go-workbook/tree/master/restapi-test-app](https://github.com/agusrichard/go-workbook/tree/master/restapi-test-app)。

## 贮藏室ˌ仓库

仓库的主文件

仓库的测试文件

正如我在上一节中解释的那样，当我们想要在套件中运行任何测试之前做一些设置时，使用 SetupSuite 如果我们想要在每次测试之后做一些事情，例如，清理我们使用过的任何表，则使用 TearDownTest。

## 用例

对于用例，因为我们需要存储库才能正常工作，但是我们不想实际使用存储库并调用数据库。这意味着我们需要模仿存储库。现在，嘲弄会给我们做这种嘲弄的东西很好的帮助。

还记得在存储库中，我们有一个它所使用的所有操作的接口吗？现在，我们需要模拟版本，通过输入命令`mockery --name=TweetRepository`。这个命令将生成一个库的模拟版本。结果会是这样的。

在这之后，我们可以在用例中调用被嘲笑的版本。

用例的主文件

用例的测试文件

## 处理者

处理程序与用例有些类似，因为在用例中，我们模拟了存储库，那么对于处理程序，这意味着我们需要模拟用例。为了生成用例的模拟版本，您必须做与上一节相同的事情。不同之处在于赋予名称的值是 Usecase 接口。(`mockery --name=TweetUsecase`)。这个命令将生成模拟版本，然后它就可以被我们的处理程序使用了。

处理程序的主文件

处理程序的测试文件

就是这样，我们已经测试了所有(有点😄)作为独立单元的我们的应用程序的构建块。尽管我使用的模型架构由处理程序、用例以及存储库组成。我相信同样的概念可以适用于更一般的情况。

# 结论

如果我们想让我们的测试更简单，证明和嘲弄将是我们需要的便利工具。因此，学习如何使用它们，对我们来说肯定是一个很好的点。总的来说，我们可以使用 Assert 来验证我们的函数是否做了正确的事情，有很多 Assert 提供的断言可以使用。当我们想要分组一些测试，做一些设置和拆卸时，我们使用 Suite。最后，当我们的函数依赖于其他依赖项，但我们不想实际使用这些依赖项时，我们使用 Mock。

感谢阅读，你可以在这里寻找完整代码[https://github . com/agusrichard/go-workbook/tree/master/restapi-test-app](https://github.com/agusrichard/go-workbook/tree/master/restapi-test-app)。

如果这篇文章对你有帮助，请随意分享和鼓掌😁。