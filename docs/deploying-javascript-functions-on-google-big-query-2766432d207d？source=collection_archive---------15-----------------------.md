# 在 Google Big Query 上部署 Javascript 函数

> 原文：<https://medium.com/nerd-for-tech/deploying-javascript-functions-on-google-big-query-2766432d207d?source=collection_archive---------15----------------------->

![](img/3d9288d2341e3f38f9c9ff40fad360e5.png)

照片由 [Aditya Chinchure](https://unsplash.com/@adityachinchure?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Google BigQuery 支持用 Javascript 和 SQL 编写的用户定义函数(UDF ),这开启了 UDF 可以提供的全新功能世界。

让我们从在 BigQuery 的 UDF 中使用 Javascript 的一些基础知识开始。这里的示例使用标准 SQL 模式，因为这是首选语法。

在下面的例子中，创建了临时 UDF，但这只是使测试和开发更容易。

# 返回类型

首先，一个简单的 Javascript UDF 返回一个布尔值:

```
CREATE TEMP FUNCTION booleanExample()
  RETURNS BOOLEAN
  LANGUAGE js AS r"""
    return true;
  """
;
SELECT booleanExample() AS result;
```

接下来是返回字符串数组的 UDF:

```
CREATE TEMP FUNCTION arrayOfStringsExample()
  RETURNS ARRAY<STRING>
  LANGUAGE js AS r"""
    return ['one', 'two', 'three'];
  """
;
SELECT arrayOfStringsExample() AS result;
```

现在，UDF 返回一个 Javascript 对象:

```
CREATE TEMP FUNCTION objectExample()
  RETURNS STRUCT<one INT64, two INT64, three INT64>
  LANGUAGE js AS r"""
    return {one: 1, two: 2, three: 3};
  """
;
SELECT objectExample() AS result;
```

如果您要在`STRUCT`中指定 Javascript 对象中不存在的附加字段，它们将被简单地设置为`null`。反过来也是如此，所以您可以从结果`STRUCT`中省略字段，Javascript 对象中的任何其他字段都不会被返回。

接下来，UDF 返回一个对象数组:

```
CREATE TEMP FUNCTION arrayOfObjectsExample()
  RETURNS ARRAY<STRUCT<one INT64, two INT64, three INT64>>
  LANGUAGE js AS r"""
    return [
      {one: 1, two: 2, three: 3},
      {one: 1, two: 2, three: 3}
    ];
  """
;
SELECT arrayOfObjectsExample() AS result;
```

最后，UDF 将一个字符串作为输入:

```
CREATE TEMP FUNCTION echo(word STRING)
  RETURNS STRING
  LANGUAGE js AS r"""
    return word;
  """
;
SELECT echo('echo') AS result;
```

使用上面的例子，你应该能够创建一个 Javascript 函数来返回你需要的数据类型。支持的数据类型的完整列表在[标准 SQL 用户定义函数文档](https://cloud.google.com/bigquery/docs/reference/standard-sql/user-defined-functions#supported-javascript-udf-data-types)中有详细说明。

# 捆绑资产

对于小型或简单的 UDF，使用内联 Javascript 很好，但是为了利用 Javascript 的各种开源模块，我们必须创建一个包含所有逻辑和代码的部署资产。

这个过程的一个常见选择是使用 [webpack](https://webpack.js.org/) ，就像您使用 web 应用程序一样。

所需的`webpack.config.js`非常简单，我们只需定义一个入口点和一个结果输出文件名:

```
module.exports = {
  entry: './src/index.js',
  mode: 'production',
  output: {
    filename: 'dist.js'
  }
}
```

# 入口点

我们将创建一个`index.js`文件，它将允许我们所有的功能在我们的 BigQuery UDFs 中出现并可用:

```
const typeExamples = require('./typeExamples')const index = module.exports = {}index.typeExamples = typeExamples
index.helloWorld = () => 'Hello World!'// Put functionality you need in the global scope for BigQuery usage Object.assign(global, index)
```

在上面的例子中，我们包含了另一个文件中的示例函数，并定义了一个简单的 hello world 函数。如果您有大量的功能，您可以使用 npm 模块，如`requireAll`。

您的 javascript 代码可以像任何其他 node.js 应用程序一样以通常的方式进行单元测试。

# 部署

要使用预构建的 Javascript 资产作为功能，我们需要首先将其部署到 Google 云存储(GCS)中。这里最简单的解决方案是手动将 webpack 创建的`dist.js`复制到 GCS bucket。

对于更加正式和协调的流程，我们可以创建一个 Javascript 脚本来将我们的资产部署到 GCS。下面的代码是一个简单的例子。

```
const { Storage } = require('@google-cloud/storage')const upload = async () => {
  const storage = new Storage()
  try {
    await storage.createBucket('YOUR_BUCKET')
  } catch (e) { // Don't worry if the bucket already exists
    if (e.message !== 'Sorry, that name is not available. Please try a different one.')
    throw e
  }
  return storage
    .bucket('YOUR_BUCKET')
    .upload('dist/dist.js', {
      destination: 'bigquery-js-udf-example/dist/dist.js'
    })
}upload()
  .catch((e) => {
    console.error(e.message)
    process.exit(1)
  })
  .then(() => {
    console.log('...Success')
  })
```

## 证明

所有使用 GCP 功能的脚本都要求您的终端已经通过 GCP 认证。

查阅 [Google 的文档](https://cloud.google.com/sdk/docs/quickstart)安装 SDK，然后可以使用以下命令来配置认证。

```
gcloud auth login
gcloud auth application-default login
```

安装 Google Cloud SDK 后，可以使用以下命令轻松检查身份验证:

```
gcloud auth list
```

请查阅 CI 提供商的文档，了解如何在部署时配置 GCP 身份验证，这通常是使用以前创建的服务帐户配置环境变量的情况。

# 测试

现在我们准备用 BigQuery 测试我们的新资产。最简单的手动方法是在 GCP BigQuery 控制台中调用 UDF，如下所示:

```
CREATE TEMP FUNCTION booleanExample()
  RETURNS BOOLEAN
  LANGUAGE js OPTIONS (
    library=["gs://YOUR_BUCKET/bigquery-js-udf-example/dist/dist.js"]
  )
  AS r"""
    return typeExamples.boolean();
  """
;
SELECT booleanExample() AS result;
```

我们可以使用编写单元测试的同一个框架来自动化测试。

```
const { BigQuery } = require('@google-cloud/bigquery')const runUdf = async (client, returnType, js) => {
  const query = `
    CREATE TEMP FUNCTION testFunction()
      RETURNS ${returnType}
      LANGUAGE js OPTIONS (
        library=["gs://YOUR_BUCKET/bigquery-js-udf-example/dist/dist.js"]
      )
      AS r"""
        ${js}
      """
    ;
    SELECT testFunction() AS result;`
  const [job] = await client.createQueryJob({
    query
  })
  const [rows] = await job.getQueryResults()
  return rows[0].result
} describe('BigQuery tests', () => {
  let bigquery beforeAll(() => {
    bigquery = new BigQuery({
      projectId: process.env.GCP_PROJECT_ID
    })
  }) it('typeExamples.boolean()', async () => {
    const result = await runUdf(bigquery, 'BOOLEAN', 'return typeExamples.boolean();')
    expect(result).toBe(true)
  })
})
```

# NPM 模块

现在我们已经有了用 webpack 构建的 JS 资产和将代码部署到 GCS 的方法，我们可以根据需要包含其他模块。这可以以与任何其他客户端项目相同的方式包含在内。然而，你可以使用的东西有一些[限制](https://cloud.google.com/bigquery/docs/reference/standard-sql/user-defined-functions#limitations)。

下面的例子使用了 [sillyname](https://www.npmjs.com/package/sillyname) NPM 模块，显示了包含第三方代码就像任何其他 node.js 应用程序一样:

```
const generateName = require('sillyname')
const index = module.exports = {}
index.sillyName = generateName
Object.assign(global, index)
```

现在可以在 Javascript UDFs 中访问`sillyName`函数，访问方式与前面的函数一样。

# 包扎

Javascript UDF 的允许你利用大量的开源模块进入你的 UDF。使用流行的 JS 测试框架，如 [Jest](https://jestjs.io/) 或 [Mocha](https://mochajs.org/) ，可以很容易地对这些功能进行单元和系统测试。

完整的工作示例、测试和部署脚本可以在随附的 GitHub 项目中找到，网址是:

[https://github.com/thedumbterminal/bigquery-js-udf-example](https://github.com/thedumbterminal/bigquery-js-udf-example)

*最初发表于*[*【https://www.thedumbterminal.co.uk】*](https://www.thedumbterminal.co.uk/posts/2021/03/deploying_javascript_functions_on_google_big_query.html)*。*