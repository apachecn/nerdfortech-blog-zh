# 使用 gRPC、Python 和 Golang 构建微服务应用程序(第 3 部分)

> 原文：<https://medium.com/nerd-for-tech/build-a-microservice-app-using-grpc-python-and-golang-part-3-819388a16717?source=collection_archive---------12----------------------->

![](img/11f3aa8c15b6453e31d68b1ace21e9cd.png)

[明库斯](https://unsplash.com/@minkus?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

在本文中，我们将继续构建我们的应用程序，并将重点放在 todo CRUD 操作上。因为实现与 auth 服务没有太大的不同，所以希望这篇文章会更短。让我们投入进去吧！

# Todo 服务

同样，对于 auth 服务，在继续编写原型文件之前，首先我们必须做一些设置。

创建一个名为 grpc_todo 的数据库和一个名为 todo 的表。您可以使用以下查询创建该表。

```
CREATE TABLE public.todos (id serial NOT NULL,title varchar(64) NOT NULL,description varchar(256) NULL,user_id int4 NULL,CONSTRAINT todos_pkey PRIMARY KEY (id));
```

做些准备。现在数据库。

这些配置与 auth 服务非常相似，不同之处在于数据库的名称。它被命名为`grpc_todo`而不是`grpc_auth`。接下来，定义 Todo 的模型。

Todo 模型有 4 个字段，ID、标题、描述和用户 ID。此外，我们为每个字段定义了标记。继续，定义存储库(现在只有模板)。

上面的代码没什么特别的，因为它只是一个`repository`文件的模板(咄！？).我们稍后将回来实现细节。现在，创建用例。

这仍然是常见的事情，我们在上一篇文章中已经这样做了。接下来，这是新的东西，定义 todo 原型文件。创建一个名为`todo.proto`的文件，放在`todo`工作目录(todo 服务的根目录)下。

之后，使用这个命令生成 gRPC 代码，我们将在`todo`文件夹中有两个 go 文件。

```
protoc --go_out=./todo --go_opt=paths=source_relative \--go-grpc_out=./todo --go-grpc_opt=paths=source_relative \./todo.proto
```

从上面的 proto 文件中，我们定义了四个过程，分别是`CreateTodo`、`GetTodos`、`UpdateTodo`和`DeleteTodo`；基本正常的 CRUD 操作。对于每个过程，我们还定义了它的`Request`和`Response`形式。例如，`CreateTodoRequest`有`userID`、`title`、`descriptions`字段。因此，当客户端想要远程调用过程时，他必须给出正确的请求形式，然后服务器将做出适当的响应(给我们`CreateTodoResponse`)。

在这之后，让我们定义服务器部分。

我们的 todo 服务器文件仍然是基本的，所以我们将稍后回来。最后是主文件。

此时，您可以运行`go run main.go`命令，但是它不能正常工作，因为我们还没有创建客户端并在服务器和客户端之间建立连接。没必要担心，让我们现在就定义它。

# 待办事项客户端

现在，确保您位于主服务的正确工作目录中。之后，将同样的`todo.proto`从`todo`服务复制到`main`服务，并使用这个命令在 Python 中生成 gRPC 代码。

```
python -m grpc_tools.protoc -I. --python_out=./todo --grpc_python_out=./todo ./todo.proto
```

运行这个命令后，我们有两个生成的文件。接下来，让我们定义客户端操作。

`TodoClient`有四个方法(不包括构造函数),我们将在 todo 视图函数中使用。其中四个有各自的职责，例如，当我们想创建一个 todo 时，`create_todo`方法会调用 todo 服务中的一个远程过程。

现在，让我们使用客户端并在视图文件中实现它们。

注意，每个视图函数的顶部都有一个装饰器。这个装饰器的工作，就像它的名字一样，是确保只有登录的用户才能访问这个函数。为此，我们必须定义一个中间件。现在，创建一个名为 middleware 的文件夹，并在其中创建一个名为`middleware.py`的文件。

我们的 todo 服务及其与主服务的连接几乎完成。最后一步，您可以使用 Postman 测试这个服务。一切正常吗？如果是，那么很好，我们在这里做得很好。

在下一篇文章中，我们将迁移到使用模板引擎，因此我们的应用程序不再使用 REST 框架。您可以在这里找到本文的代码[https://github . com/agusrichard/python-golang-grpc/tree/part 3](https://github.com/agusrichard/python-golang-grpc/tree/part3)。

如果您有任何问题或反馈，请随时留下评论或通过电子邮件联系我，agus.richard21@gmail.com。另外，如果你认为这篇文章对你有帮助，请不要犹豫，给这篇文章鼓掌。

感谢您的阅读，下一篇文章再见。