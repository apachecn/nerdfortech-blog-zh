# 去抖您的搜索|反应输入优化

> 原文：<https://medium.com/nerd-for-tech/debounce-your-search-react-input-optimization-fd270a8042b?source=collection_archive---------0----------------------->

![](img/8933e4fa3854e49902a096a7f2866351.png)

去抖是一种限制任务发生次数的优化技术。如果您曾经在 React 中实现过一个搜索特性，在用户输入每个字符时自动过滤列表或发送一个获取请求，这是一项可以大大提高应用程序效率的技术。

# 设置/安装

对于这个例子，我们将从 lodash 实用程序库中导入去抖功能，而不是创建我们自己的版本。为此，我们将把这一行代码添加到处理过滤操作的组件的顶部。

```
import debounce from 'lodash.debounce';
```

接下来，我们必须安装依赖 npm 或 yarn，这取决于您使用哪一个。

`npm install lodash.debounce`

或者

`yarn add lodash.debounce`

# 分解示例

在上面的 CodeSandbox 示例中，我已经创建了一个简单的应用程序，它显示了一个水果列表和一个搜索栏，当你输入时，搜索栏会过滤列表。

搜索栏是一个[控件形式](https://reactjs.org/docs/forms.html)。它显示的值取自状态，输入通过其`onChange`合成事件与状态链接。

```
const [searchTerm, setSearchTerm] = useState(""); const handleChange = (e) => { setSearchTerm(e.target.value);};
```

```
<input type="text" value={searchTerm} onChange={handleChange} />
```

每次输入一个字符或从输入中删除一个字符时，水果列表被过滤，最终用户看到的列表被更新。

```
let listToDisplay = fruits; if (searchTerm !== "") listToDisplay = fruits.filter((fruit) => { return fruit.includes(searchTerm); });}
```

# 问题是

虽然这个例子运行良好，但是您可以想象随着列表的增长，每次搜索花费的时间会越来越长。另一个场景可能是我们为每个被搜索的字符发送 api 请求。无论哪种方式，这种实现都可能开始降低应用程序的速度，并且您会不知道如何解决这个问题。进来了`debounce`。

# 去抖

```
_.debounce(func, [wait=0], [options={}])
```

虽然`debounce`有 3 个参数，我们将使用前两个，我们想要调用的函数和我们设置的等待时间。

我们还将使用内置在`debounce`中的`.cancel` 函数来帮助清理(稍后将详细介绍)。

有关`debounce`功能的更多信息，请参考[官方文档](https://lodash.com/docs/4.17.15#debounce)。

# 履行

第一步是与`useMemo`一起工作，从我们的`debounce`函数中记忆一个返回值。该返回值将在两次重新呈现之间保持不变。这一步是必不可少的，因为如果我们不在两次重新渲染之间保存这些数据，其他的`debounce`实现将会在每次重新渲染时出现，而我们将基本上拥有我们最初的例子；我们将在最后一次字符输入后的一段时间后过滤每个字符的列表。

```
const debouncedResults = useMemo(() => { return debouce(handleChange, 300);}, []);
```

接下来，我们将与`useEffect`合作，清理当我们的组件被卸载时来自`debounce`的任何副作用；当我们不在那个页面或视图上时，就没有理由进行搜索了。这就是我们将在内存返回值上调用`.cancel`函数的地方。

```
useEffect(() => { return () => { debouncedResults.cancel(); };});
```

最后，我们将撤销一些以前的工作。我们将通过删除表单的 value 属性并将`onChange`设置为调用`debouncedResults`来使表单不受控制。这将使表单在每次输入改变时被调用`debounce`。

```
<input type="text" onChange={debouncedResults} /
```

就这样，一个完整的去抖输入！

# 工作示例