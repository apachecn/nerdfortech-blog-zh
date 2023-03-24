# 用 yupjs 验证引用数据

> 原文：<https://medium.com/nerd-for-tech/referential-data-validations-with-yupjs-98639bc04dd3?source=collection_archive---------27----------------------->

![](img/c59acfb42afd681f789ff62a462ae633.png)

班加罗尔乌尔索尔湖。绍拉夫·萨胡在 Unsplash 上拍摄的照片

注意:这篇博客最初发布在我的个人网站上:[用 yupjs (mrsauravsahu.tech)](https://mrsauravsahu.tech/posts/45/) 验证参考数据

# 什么是数据验证？

数据验证是根据业务需求检查给定值是否符合特定标准的过程。

对于任何输入——UI 输入字段或 API 输入主体，数据验证都是至关重要的。任何随意的输入都不应该被信任。数据验证在确保这些输入在我们的应用程序中产生意想不到的副作用之前严格地通过正确的管道中发挥着至关重要的作用。

# JavaScript 世界中的数据验证

在 JavaScript 项目中，浏览器和 node.js，也就是说，有几个 npm 包可用于进行数据验证。我个人用过 joi 和 yupjs。

joi 是我长期以来进行数据验证的首选。它与 hapijs 合作得非常好，并且有一个很好的社区围绕着它。相信我，我对 joi 没有任何意见，只是我觉得和 yupjs 一起工作更容易。

yupjs 也是一个数据验证库，它从 joi 获得了很多特性，但更侧重于客户端验证，并且可以很容易地扩展。

# 数据验证的一个例子

对传入的“数据传输对象”的每个属性进行数据验证。只是一种奇特的方式🎓也就是说，从原始输入中创建一个对象，并在实际存储或用于应用程序的其他地方之前进行清理和处理。

让我们以一个简单的注册页面为例。我们将有两个输入，DTO 将具有如下所示的形状:

```
type SignUpDto = {
  userName: string | undefined,
  password: string | undefined
}
```

这里的简单数据验证是:

*   用户名和密码字段是必需的
*   用户名的最大长度应为 12
*   密码的最小长度应为 8

等等。

# 输入 yupjs

为了实现这一点，yupjs 使用了一个称为模式的概念来进行验证。我相信你会发现 yupjs 库和 joi 非常相似，所以让我们来看看。用户名和密码的简单验证可以写成如下所示:

```
import * as yup from 'yup'type SignUpDto = {
  userName: string | undefined,
  password: string | undefined
}const signUpSchema = yup.object({
  userName: yup
    .string()
    .required('please enter a username')
    .max(12),
  password: yup
    .string()
    .required('please enter a password')
    .min(8)
})
```

如您所见，您还可以为每个验证定义定制错误消息。现在这个`signUpSchema`实际上可以用来对数据进行验证，如下所示:

```
const signUp: SignUpDto = {
  userName: 'sample',
  password: undefined
}signUpSchema.validate(signUp, { abortEarly: false })
  .then(console.log)
  .catch(console.error)>
ValidationError: please enter a password
    at finishTestRun (.../node_modules/yup/lib/util/runTests.js:63:20)
    at .../node_modules/yup/lib/util/runTests.js:17:5
    at finishTestRun (.../node_modules/yup/lib/util/runTests.js:67:9)
    at .../node_modules/yup/lib/util/createValidation.js:72:127 {
  value: { userName: 'mrsauravsahu', password: undefined },
  path: undefined,
  type: undefined,
  errors: [ 'please enter a password' ],
  inner: [
    ValidationError: please enter a password
        at createError (/Users/sauravsahu/Documents/personal/code/yuppers/node_modules/yup/lib/util/createValidation.js:54:21)
        at /Users/sauravsahu/Documents/personal/code/yuppers/node_modules/yup/lib/util/createValidation.js:72:107 {
      value: undefined,
      path: 'password',
      type: 'required',
      errors: [Array],
      inner: [],
      params: [Object]
    }
  ]
}
```

正如我们所看到的，我们得到了关于为什么在`inner`属性中验证失败的详细解释。我映射了`inner`属性，只保留了路径和值字段——这足以让我的前端应用程序理解要显示什么本地化的消息。

```
signUpSchema.validate(signUp, { abortEarly: false })
  .then(console.log)
  .catch(err => {
    var validationErrors = err.inner.map((error: any) => ({ type: error.type, path: error.path }))
    console.error(JSON.stringify(validationErrors, undefined, 2))
  })>
[
  {
    "type": "required",
    "path": "password"
  }
]
```

这很好，yupjs 支持许多不同类型的开箱即用的验证，这里列出了— [yupjs API](https://github.com/jquense/yup#api)

# 什么是引用验证？

与某些仅依赖于属性的一个键的验证规则不同，更复杂的验证也可以引用其他属性。yupjs 允许我们用`test`方法引用 DTO 的其他属性。

在我们的例子中，假设我们想要确保密码不包含字符串形式的用户名，也就是说，如果用户名是`sample`，密码就不能是`123saMplE456`，因为单词`sample`出现在密码中。

为了验证这个密码，我们还需要参考用户名字段。我们可以用 yupjs 的`test`方法来写这个。让我们修改我们的模式，如下所示。

```
const signUpSchema = yup.object({
   userName: yup
     .string()
     .required('please enter a username')
     .max(12),
   password: yup
     .string()
     .required('please enter a password')
     .min(8)
+    .test('contains-username', (password, context) => {
+      const { userName } = context.parent;
+      const userNameString = userName ?? '';
+      const containsUserName = (password ?? '').toLowerCase().includes(userNameString.toLowerCase())
+
+      return !containsUserName
+    })
 })
```

正如你所看到的，我添加了缺省值和零合并操作符，比如`userName`和`password`可能是 falsey。

现在，如果我们尝试验证我们的示例用户，我们会得到这个验证错误，这正是我们想要的。

```
[
  {
    "type": "contains-username",
    "path": "password"
 }
]
```

对于`test`方法，第一个参数是验证的名称，对我们来说，它是`contains-username`，第二个方法是实际的测试函数，它获得当前值和验证的上下文，我们可以用这个上下文选择`userName`。

# 结论

yupjs 是一个非常通用的数据验证库。它既可以在浏览器中使用，也可以在 node.js 中使用。引用验证轻而易举，这些方法也可以很容易地进行单元测试。

yupjs 还包含将对象从一种形状转换为另一种形状的转换方法。我目前正在[每日语音应用](https://github.com/daily-vocab/daily-vocab)上享受睡衣

祝你愉快！继续编码。

- [mrsauravsahu](https://poly.work/mrsauravsahu)