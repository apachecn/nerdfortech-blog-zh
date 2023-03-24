# 使用 GraphQL、Express.js 和 React 的 Todo 应用程序—第 1 集

> 原文：<https://medium.com/nerd-for-tech/todo-app-with-graphql-express-js-redux-episode-1-ccbe54b40c89?source=collection_archive---------4----------------------->

![](img/aee247e71af8002c5249e1af3082ea89.png)

# 构建基本 API

欢迎光临。进行 API 调用并在 web 页面上呈现响应非常有趣。但是，创建一个 API 并查询它是很棒的！此外，创建 API 可以让我们更深入地了解 API 是如何工作的，它们背后的逻辑是什么，以及当收到 API 调用时后端会发生什么。因此，如果您觉得到目前为止您已经在项目中做了足够多的 API 调用，并且您决定深入了解任何应用程序的整个过程，我可以说这篇文章可能会对您有所帮助。

我们将在这个项目中构建一个 todo 应用程序。我们将使用这些技术:

*   快递. js
*   GraphQL
*   阿波罗
*   蒙戈
*   反应

我们的项目结构如下图所示:

![](img/953f0c69f967f2fd7702e97a8aa080f3.png)

这一系列文章计划由 3 集组成。这些剧集将包含以下标题:

*   构建基本 API
*   构建用户身份验证
*   在前端呈现和操作数据

在这一集里，我们将构建一个基本的 API。它将包含一个获取 Todo 项的查询和三个创建、修改和删除项的突变。

这里是[这一集](https://github.com/ardaorkin/todo-app-server)的最终代码。

准备好了吗？那我们走吧！

## 要求

*   NodeJS 14v+
*   NPM 还是纱线
*   NPX
*   React 开发人员工具
*   Apollo 客户端开发工具

## 建筑开发环境

C 创建一个名为 **todo-app** 的文件夹，并在 **todo-app** 文件夹中再创建两个文件夹，分别名为 **server** 和 **client** 。

在代码编辑器中打开 **todo-app/server** 文件夹，运行此命令构建 NodeJS 项目并安装依赖项:

```
npm init -y && npm i express apollo-server-express graphql mongoose dotenv cors && npm i --save-dev nodemon @babel/core @babel/node @babel/preset-env babel-loader
```

现在创建您的后端项目结构，如下所示:

![](img/5a0ae0a2d2a7cf8171bb73cc7ba64d1d.png)

安装项目依赖项时，将自动创建 package.json、package-lock.json 文件和 node_modules 目录。因此您不需要手动创建它们。

创建所有文件和目录后，打开。babelrc 文件，并将这几行写入其中:

```
{"presets": ["@babel/env"]}
```

我们在中进行的配置。babelrc 文件将提供我们在模块导入时使用 ES6。

在进一步讨论服务器配置之前，让我们创建一个 MongoDB 云帐户。

## MongoDB 配置

为了配置 MongoDB，让我们在 mongodb.com/cloud 的[创建一个账户。稍后，创建一个组织、组织中的一个项目和项目中的一个群。创建集群后，在集群所在的页面中，单击数据库访问。在那里单击添加新数据库用户选项，并创建一个数据库用户。稍后，单击左侧栏上的集群选项。在打开的页面上，单击连接按钮。在“设置连接安全性”步骤中，选择“允许任何地方”选项，然后单击“选择连接方法”。然后，让我们单击连接您的应用程序选项，并复制**下的连接信息，将您的连接字符串添加到您的应用程序代码**。让我们回到文本编辑器，在。env 文件，并将 MongoDB 连接信息赋给该变量。使用刚刚在数据库访问步骤中创建的密码进行更改，并将 **myFirstDatabase** 更改为 **todolist** :](https://www.mongodb.com/cloud)

```
MONGODB_URL="mongodb+srv://username:12345@cluster0.mjh9d.mongodb.net/todolist?retryWrites=true&w=majority"
```

现在，打开 **db/connection/index.js** 文件，写下这几行:

```
import dotenv from "dotenv";import mongoose from "mongoose";dotenv.config();const dbConnection = () => new Promise((resolve, reject) => { mongoose.connect(process.env.MONGODB_URL, { useNewUrlParser: true, useUnifiedTopology: true, useFindAndModify: false, }); const db = mongoose.connection; db.on("error", () => { console.error.bind(console, "connection error:"); reject( new Error( "Connection error has occurred when trying to connect to the database!" ) ); }); db.once("open", () => resolve("🚀 Successful database connection."));});export default dbConnection;
```

通过这个配置，我们定义了 MongoDB 的连接信息，并提供了一个 promise 函数，可以在项目的任何地方使用。在用 JavaScript 编写后端项目时，尽可能多地创建 Promise 函数并添加错误处理行是一个好主意。

稍后，打开 **db/models/TodoModel.js** 文件，用以下几行填充它:

```
import mongoose from "mongoose"; const todoSchema = new mongoose.Schema({ user: String, title: String, mission: String, isDone: Boolean,},{ timestamps: true });const Todo = mongoose.model("Todo", todoSchema);export default Todo;
```

现在，我们已经创建了集合的模式，该模式将放在数据库中，并从该模式中复制一个模型。让我们继续服务器配置，并运行服务器以连接到数据库。

## 服务器配置

下面我们已经创建了 MongoDB 配置。现在，让我们使用它！

打开 **server.js** 文件，将这些行写入其中:

```
import express from "express";import dbConnection from "./db/connection";const startServer = async () => { await dbConnection() .then((result) => console.log(result)) .catch((err) => console.log(err)); const app = express(); app.use("/", (req, res) => res.send("Welcome to Todo App"));app.listen(4000, () => console.log(`🚀 Server listening on port 4000`));};startServer();
```

现在打开 **package.json** 文件，将这一行写入脚本部分:

```
"start": "nodemon ./server --exec babel-node -e js",
```

编写完启动脚本后，您的 **package.json** 的脚本部分应该如下所示:

```
"scripts": { "start": "nodemon ./server --exec babel-node -e js", "test": "echo \"Error: no test specified\" && exit 1"},
```

打开控制台，使用以下命令运行服务器:

```
npm start
```

如果您在终端的输出上看到这两条消息，这意味着您成功地进行了配置:

```
🚀 Successful database connection.
🚀 Server listening on port 4000
```

是时候构建 GraphQL 模式并运行 Apollo 服务器了！

## 构建 GrapQL 模式和 Apollo 服务器

要构建一个 GraphQL 结构，从创建类型定义开始是一个好主意。为此，打开**schema/typeDefs/index . js**文件，并编写以下几行:

```
import { gql } from "apollo-server-express";const typeDefs = gql` type Todo { user: String title: String mission: String createdAt: String updatedAt: String isDone: Boolean } input TodoInput { user: String title: String mission: String isDone: Boolean } type Query { getTodoList: [Todo] } type Mutation { addTodo(todo: TodoInput): Todo }`; export default typeDefs;
```

这样，我们定义了一个与数据库集合模式并行的 Todo 类型，一个与 Todo 类型和 Todo 模式并行的输入类型。我们还定义了内置的 GraphQL 类型:查询和变异。我们将定义一个**解析器**对象，我们将为其中的查询和变异类型使用相同的属性名；此外，我们将把 MongoDB 查询函数分配给**解析器**对象中的属性。我们将把查询函数写入模型。稍后，我们将合并 Apollo 服务器配置中的类型定义、解析器和模型，这样，我们将建立**类型定义、****解析器、**和**模型**之间的关系。

现在，让我们创建模型。为此，打开 **schema/models/index.js** 文件，将以下几行写入其中:

```
import Todo from "../../db/models/TodoModel";const generateTodoModel = () => ({queries: { getAll: () => new Promise( async (resolve, reject) => await Todo.find({}, (err, todo) => err ? reject(err) : resolve(todo) )
    ), }, mutations: { addTodo: (todo) => new Promise((resolve, reject) => new Todo(todo).save((err, todo) => (err ? reject(err) : resolve(todo))) ), },});export default generateTodoModel;
```

这样，我们就创建了将在导入的 Todo 模型上执行的查询函数，并将这些函数分配给我们创建的 GraphQL 模式中的模型。稍后，在解析器对象中，我们将通过在 Apollo 服务器中定义的上下文对象调用这些函数，并将它们作为值分配给属性。现在，让我们创建 resolver 对象并将 console.log()函数分配给属性。为此，打开**schema/resolvers/index . js**并将这些行写入其中:

```
const resolvers = { Query: { getTodoList: async (parent, args, context) => console.log(JSON.parse(JSON.stringify(args.todo)), context) }, Mutation: { addTodo: async (parent, args, context) => console.log(JSON.parse(JSON.stringify(args.todo)), context) },};export default resolvers;
```

现在，我们已经创建了**类型定义**、**、**、**解析器**、**T22、**模型**。是时候创建 Apollo 服务器配置并与那些 GraphQL 元素建立关系了。要做到这一点，打开 **server.js 文件**并像这样修改它:**

```
import express from "express";import { ApolloServer } from "apollo-server-express";import typeDefs from "./schema/typeDefs";import resolvers from "./schema/resolvers";import generateTodoModel from "./schema/models";import dbConnection from "./db/connection";import cors from "cors" const startApolloServer = async () => { await dbConnection() .then((result) => console.log(result)) .catch((err) => console.log(err)); const app = express();
  app.use(cors()) const server = new ApolloServer({ typeDefs, resolvers, subscriptions: { path: "/subscriptions" }, context: ({ req }) => { return { models: { Todo: generateTodoModel(), }, }; }, }); await server.start(); server.applyMiddleware({ app }); app.use((req, res) => { res.status(200); res.send("Welcome Todo App"); res.end(); }); await new Promise((resolve) => app.listen({ port: 4000 }, resolve)); console.log(`🚀 Server ready at http://localhost:4000${server.graphqlPath}`); console.log(`🚀 Subscriptions ready at ws://localhost:4000${server.subscriptionsPath}` ); return { server, app };};startApolloServer();
```

如您所见，我们构建了 Apollo 服务器，并在其中创建了**类型定义**、**解析器**和**模型**之间的关系。现在，检查您之前运行`npm start`命令的终端。如果您在终端的输出上看到这些消息…

```
🚀 Successful database connection.
🚀 Server ready at [http://localhost:4000/graphql](http://localhost:4000/graphql)
🚀 Subscriptions ready at [ws://localhost:4000/subscriptions](http://localhost:4000/subscriptions)
```

…祝贺您，您已经成功运行了一台 Apollo 服务器！

你在 Apollo 服务器配置中看到了**上下文**功能吗？它将来自 **express 服务器**的 **req** 对象作为参数。然而，它现在不用它了。无论如何，更重要的是看看**上下文**函数返回什么——它返回我们在 GraphQL 模式中定义的模型！

现在，在解析器中，让我们调用模型中定义的函数。记住，到目前为止，我们只是将查询元素记录到控制台。让我们通过解析器运行一个真正的数据库查询，而不是仅仅将事情记录到控制台。为此，打开**schema/resolvers/index . js**并进行如下更改:

```
const resolvers = { Query: { getTodoList: async (parent, args, context) => await context.models.Todo.queries.getAll(), }, Mutation: { addTodo: async (parent, args, context) => await context.models.Todo.mutations.addTodo( JSON.parse(JSON.stringify(args.todo)) ), },};export default resolvers;
```

有了这个配置，我们就能够使用 GraphQL 查询语言运行 MongoDB 查询。要运行一个真正的查询，在浏览器中打开[http://localhost:4000/graph QL](http://localhost:4000/graphql)地址，然后运行一个将创建一个新 todo 的变异，如下所示:

![](img/ce0200501117f7c0adc7f362ade6962f.png)

如果您看到了**数据**对象，这意味着您已经完成了正确的配置，并且已经向 MongoDB 添加了一个 todo 对象。为了确保安全，请查看 MongoDB 集合。单击 MongoDB 云中集群页面中的 Collections 选项卡。在出现的页面上，单击名为 **todolist** 的数据库下名为 **todo** 的集合。

![](img/462abb43c754961511b69ac54d9df7b4.png)

到目前为止，我们已经添加了一个查询解析器和一个变异解析器。让我们再添加两个变异解析器来修改和删除 todo 项。为此，打开 **schema/models/index.js** 文件，并将这些函数写入**突变**对象:

```
modifyItem: (body) => new Promise( async (resolve, reject) => await Todo.findByIdAndUpdate(body.id, body.query, (err, todo) => err ? reject(err) : resolve(todo) ) ), deleteItem: (id) => new Promise( async (resolve, reject) => await Todo.findByIdAndDelete(id, (err, todo) => err ? reject(err) : resolve(todo) ) ),
```

然后转到**schema/resolvers/index . js**文件，将这些行写入**突变**:

```
modifyItem: async (parent, args, context) => await context.models.Todo.mutations.modifyItem( JSON.parse(JSON.stringify(args)) ),deleteItem: async (parent, args, context) => await context.models.Todo.mutations.deleteItem( JSON.parse(JSON.stringify(args.id)) ),
```

最后，转到**schema/typeDefs/index . js**文件，将这些行写入**突变**类型:

```
modifyItem(id: ID!, query: TodoInput): TododeleteItem(id: ID!): Todo
```

现在，在浏览器中打开[http://localhost:4000/graph QL](http://localhost:4000/graphql)。并运行 **GetTodos** 查询:

![](img/f9e36648fc96608796574af1f45dca33.png)

从返回值中复制 **_id** 属性，在 Apollo playground 上打开一个新标签，然后运行一个 **ModifyItem** 变异，如下所示:

![](img/56d38b19006a725506a204b696a7b108.png)

请参见 **updatedAt** 和 **isDone** 属性已更改。在 MongoDB 连接配置中，我们提供了 updateAt 属性以在每个更新操作中进行更改，并且我们已经通过在 Apollo 服务器上运行的查询更改了 **isDone** 。让我们运行一个删除查询。在 playground 中打开一个新的选项卡，然后像这样运行一个删除查询:

![](img/96bbc46051c45990b521cf303bc51c64.png)

就是这样！我们有一个可以运行 GraphQL 查询的 API，我们有一个 NoSQL 数据库，我们有一个与 express.js 集成的 Apollo 服务器。干得好！

下次我们将基于 API 调用复制 JSON Web Token，并在 Apollo 服务器上进行用户认证和授权配置！