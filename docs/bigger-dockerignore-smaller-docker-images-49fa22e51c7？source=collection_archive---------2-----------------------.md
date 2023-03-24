# 更大。dockerignore，较小的 Docker 图像

> 原文：<https://medium.com/nerd-for-tech/bigger-dockerignore-smaller-docker-images-49fa22e51c7?source=collection_archive---------2----------------------->

*这是四篇系列文章中的第一篇，尽量减少&保护 Docker 图像。查看该系列的其他文章:
1。* [*变大。更小的 Docker 图片*](/nerd-for-tech/bigger-dockerignore-smaller-docker-images-49fa22e51c7) *2。* [*看 Docker，看 Distro*](/nerd-for-tech/look-docker-no-distro-5dc87d4deb00)

每个人都想要更快的构建时间，更少的垃圾应用。通过增强我们的`.dockerignore`我们可以制作更小的 Docker 图像。更小的映像的好处不会停留在更快的构建时间上。较小的图像占用较少的磁盘空间，当应用程序扩展时，这确实开始显示出一些好处，可能是在自动扩展的 Kubernetes 集群中。最后，较小的 Docker 图像具有较小的攻击面。

![](img/f05be756bc8cf56f58faed2540847d76.png)

这是关于优化 Docker 图像大小和安全性的四部分系列的第一部分。本文有一个相应的存储库，用来展示一个真实的例子。回购可以在[这里](https://github.com/starlightromero/dockerignore-example)找到。每个分支都将建立在前面的基础上，展示我们将在文章中经历的不同阶段。

# 🔤回到基础

就像`.gitignore`文件定义了我们希望 git 忽略的文件一样，`.dockerignore`文件定义了我们希望 Docker 忽略的文件。但是为什么我们希望 Docker 忽略某些文件呢？回到想要更小的映像，当我们运行`docker run`或`docker-compose up`时，更小的映像意味着更快的构建时间。如果您的应用程序运行不需要某个文件，请将其放入`.dockerignore`。现在这个文件在`.dockerignore`中，它不会包含在 Docker 图像中，从而减小了图像的大小。Docker CLI 在您的应用程序的根目录中寻找`.dockerignore`。如果它不在根文件夹中，就不会被读取。

放入您的`.gitignore`中的一些文件示例如下:

*   `.git`
*   `.vscode`
*   `.gitignore`
*   `build`
*   `dist`
*   `node_modules`
*   `Makefile`
*   `README.md`

分支`01-Basics`是该部分的对应分支。克隆并运行应用程序会在端口`8080`启动一个服务器。导航到`localhost:8080`，我们可以看到文本，`“Hello, World! The secret is 1234”`。

深入到`.dockerignore`文件，我们可以看到我们忽略的文件:

在`Dockerfile`的第 8/9 步，我们运行一个`ls -la`命令。我们可以看到下面的输出:

这很好，但是我们还能去掉什么呢？

# 🐳码头工人的东西

我们可以把`Dockerfile`或者`docker-compose.yml`放在`.dockerignore`文件里吗？是啊！把这些也扔进去。如果我们的应用不直接需要文件来运行，它属于`.dockerignore`。我们甚至可以将`.dockerignore`本身放入`.dockerignore`文件中！这是为什么呢？

是的，`Dockerfile`、`docker-compose.yml`和`.dockerignore`都是用来构建映像和旋转容器的，然而，这就是这些文件的目的所在。我们的应用程序中不使用它们。*如果我们不需要文件来运行没有 Docker 的应用程序，我们也不需要文件来运行有 Docker 的应用程序。我们需要文件来构建 Docker 映像，也需要文件来运行 Docker 容器。然而，Docker 容器中的应用程序不需要这些文件。*

# 🔒环境变量

在 Docker 的上下文中，环境文件总是让我感到困惑。与`Dockerfile`不同，无论是使用 Docker 还是不使用 Docker，应用程序都需要`.env`才能正常运行。然而，如果我们正在使用一个`docker-compose.yml`文件，就像我们在这个应用程序中一样，并且在 yml 文件中我们定义了一个`env_file`，那么`.env`文件可以被忽略。

在`docker-compose.yml`中定义一个`env_file`就是`docker-compose --env-file ./.env`的声明版本。这两种方法都定义了包含秘密变量的环境文件的路径，允许 docker-compose 访问变量并将其传递给容器内运行的应用程序。通过将环境文件传递给 docker-compose，docker 本身不需要知道环境文件。这是因为 docker-compose 的作用类似于`Dockerfile`的包装器。Docker-compose 是`docker run …`的声明版本。Docker-compose 启动`Dockerfile`构建，并且能够将环境文件传递给 Docker 构建，而不需要`Dockerfile`直接需要`.env`。

# 🧪路线和单元测试

运行应用程序不需要测试，但测试应用程序需要测试。一种选择是在 Docker 容器之外测试应用程序。在这种情况下，我们可以忽略测试文件。这种方法有 Docker 之外的任何应用程序的缺点，应用程序运行的环境不能保证有所有正确的包、包版本、配置等。

在 Docker 容器中测试您的应用程序在一个标准化的环境中提供了更多的一致性。当测试文件被忽略时，我们如何测试应用程序？我们将在下面的热重装部分回到这个问题。然而，现在，我们将忽略测试文件。

# 🐗通配符选择器

在本节中，我们不会向`.dockerignore`添加任何额外的文件。我们将通过使用通配符选择器来压缩`.dockerignore`中的行数。`*`允许我们填充不确定数量的字符。在第 5 行，我们有`docker-compose*.yml`。这将忽略`docker-compose.yml`、`docker-compose.dev.yml`和`docker-composeyou-can-put-anything-here.yml`。我们在第 3 行使用了类似的语法来忽略`.dockerignore`和`.gitignore`。第 6 行和第 9 行遵循相同的逻辑。

第 7 行遵循不同的模式，`**`。双通配符允许我们匹配任意数量的目录(包括零)。因此`**/*_test.go`将匹配任何目录中的任何测试文件。这条小小的、强有力的线具有下面定义的所有能力(以及更多):

*   ✅ `./main_test.go`
*   ✅ `/tests/main_test.go`
*   ✅ `/tests/auth_test.go`
*   ✅ `/test/auth/main_test.go`
*   ✅ `/test/auth/login_test.go`
*   ❌ `./mainTest.go`
*   ❌ `/tests/authTest.go`

我们的`Dockerfile`中的`ls -la`命令产生与前面部分相同的输出:

一个很好的命令是`!`，但是我从来没有发现需要使用它。通过在`.dockerignore`中的任何一行前面放一个`!`，Docker 会忽略忽略它。多拗口啊！基本上，Docker 会确保将文件包含在映像中。这里有一个不实际的例子:

```
*.md
!README.md
```

我们将忽略除`README.md`之外的所有降价文件。

# 🔃热重装的利与弊

绑定挂载是 docker-compose 允许我们使用的一种非常有用的卷类型。它们实质上是将 Docker 容器外部的文件“绑定”到容器内部的文件。这允许热重装。但是，我经常看到它们被误用。说实话，我也不知道使用它们的最佳方式。起初，我会用 volume `.:/app`或`.:/usr/src/app`将容器外的所有内容绑定到容器内的所有内容。

如果我们理解操作的顺序，就可以更好地理解这种方法的问题。当我们运行`docker-compose up`或该命令的任何变体时，首先 docker-compose 命令被转换为`docker`命令。接下来，读取`.dockerignore`并忽略其中的任何文件。之后，读取`Dockerfile`。当然，当我们这样做时，我们只复制 Docker 没有忽略的文件。然后，应用任何卷，包括绑定装载。卷不符合`.dockerignore`。最后，如果`docker-compose.yml`中有指定的命令，则运行该命令。

通过绑定挂载`.:/app`，我们完全覆盖了`.dockerignore`。因为绑定挂载是在映像构建之后附加的，所以即使是被忽略的文件现在也被挂载到容器中。使用绑定挂载的一个更好的方法是映射特定的文件或文件夹。下面是一个开发`docker-compose.dev.yml`的例子，我们只在`main.go`文件上设置了一个绑定挂载，并将其映射到位于`WORKDIR`位置`/app/main.go`的容器中的文件。

回到测试，我们可以应用我们的新知识。由于绑定挂载是在映像构建之后应用的，所以即使我们忽略测试，我们也可以通过添加一个卷`./main_test.go:/app/main_test.go`使它们在容器中再次可访问。在这个 docker-compose 中，我们可以看到`go test`命令正在运行。由于 docker-compose 命令是在卷被映射后执行的，所以我们可以确信在运行`go test`时`main_test.go`文件将在容器内可用。

# 📦打包好

较小的图像大小不仅减少了构建时间和磁盘空间，还减少了我们的攻击面。`.dockerignore`文件只是优化 Docker 图像大小的第一步。现在你应该对`.dockerignore`有了很好的理解，知道如何使用它，以及它如何与其他 Docker 组件相关联。现在，在你的下一个项目中尝试这些策略，看看你能减少多少图片尺寸。