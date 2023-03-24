# 作为 JavaScript 开发人员，您应该知道的 JavaScript 库

> 原文：<https://medium.com/nerd-for-tech/the-javascript-libraries-you-should-know-as-a-javascript-developer-cdd9f17d088b?source=collection_archive---------4----------------------->

![](img/5c5dd52ff8eea5f72ba816670d7f11ab.png)

在过去的几年里，作为一名 JavaScript 开发人员，我发现了一些非常有用的库，我必须将它们包含在我的所有项目中，无论是前端还是后端。

# 拉姆达

安装:`yarn add ramda`

当谈到实用程序库时，有些人可能会想到`lodash`。但是`ramda`更好，原因如下:

1.  这是自动咖喱

```
// When the number of parameters supplied is not sufficient,
// It returns a new function which waits for more parameters.const add10 = R.add(10) // create a function to add 10 to inputconsole.log(add10(3)) // 13
console.log(add10(9)) // 19
```

2.它是不可改变的

```
const obj = {a: 1}const newObj = R.assoc('b', 2, obj)
console.log(newObj) // {a: 1, b: 2}// ramda functions never mutate input parameters
console.log(obj) // {a: 1}
```

3.您可以轻松地编写复杂(但可读)的函数

```
// I need to pick all `thumbnails[0].large.url` property from an array of object
// if the `url` exist, replace `http://` by `https://`,
// else returns `null`R.map(
  R.pipe(
    R.path(['thumbnails', 0, 'large', 'url']),
    R.ifElse(
      R.isNil,
      R.always(null),
      R.replace('http://', 'https://')
    )
  )
)(articles)
```

# Axios

安装:`yarn add axios`

你如何发出 HTTP 请求？由`jQuery.ajax`还是`window.fetch` API？Axios 是进行 HTTP 请求的最佳库！您可以通过不同的 HTTP 方法(GET、POST、DELETE 和其他方法)轻松地进行请求，代码可读性很强:

```
// fetch from `http://example.com/articles?limit=10`
axios.get('http://example.com/articles', {
  params: {
    limit: 10
  }
})// post data with custom header
axios.post('http://example.com/articles', {
  title: 'A very good title',
  content: 'Hello World!'
}, {
  headers: {
    Authorization: `Bearer ${accessToken}`
  }
})
```

您还可以使用基本 url、超时和拦截器来创建 API 客户端:

```
const apiClient = axios.create({
  baseURL: 'http://example.com/',
  timeout: 5000
})apiClient.interceptors.response.use(
  *response* => response.data,
  *error* => {
    if (error.response && error.response.data) {
      throw error.response.data
    } else {
      throw {
        code: 'UNKNOWN_SERVER_ERROR',
        message: 'Unknown server error.'
      }
    }
  }
)
```

# Dayjs

安装:`yarn add dayjs`

当你想操纵`DateTime`物体的时候，你可能会想到`moment`或者`date-fns`，但是我有更好的选择。

示例代码:

```
dayjs().startOf('month').add(1, 'day').format('YYYY-MM-DD')
```

它很小(~2kb)，不可变，可链接，并且支持 i18n。还需要我多说吗？

# 女士

安装:`yarn add ms`

当您需要设置特定的时间间隔时，例如每 5 分钟执行一个代码块，或者设置令牌的生命周期。你会这样做吗:

```
setInterval(() => {
  console.log('Code block executed.')
}, 300000) // 5 minutes
```

现在你可以写:

```
setInterval(() => {
  console.log('Code block executed.')
}, ms('5min'))
```

可读性更强吧？

# 结论

这里是我最常用的 JavaScript 库。试着在你的项目中使用它们，并告诉我你对它们的看法！