# 与 PostgreSQL 和 Flask 的一对多数据库关系(+CRUD)第 1 部分

> 原文：<https://medium.com/nerd-for-tech/one-to-many-database-relationship-crud-with-postgresql-and-flask-part-1-6f87fb574b7c?source=collection_archive---------5----------------------->

![](img/88667bf020bc1d95cbc9d83c869ca684.png)

一对多关系是您必须掌握的东西(多对多也是一样)。我将使用我自己的项目作为一个例子 [**博客应用**](https://github.com/Mozes721/Flask_Blog_App) 和一个关系数据库 **PostgreSQL** 和一个常见的实时暗示 **Flask** web 框架(最好你有 HTML/CSS 的知识和一些 Flask 经验)，在那里登录的用户可以发表自己的帖子并做

# 连接到 PosgreSQL 并创建模型

我们将从导入相应的包开始，连接到我们的 Flask 实例，连接到我们的 PosgreSQL 数据库，创建将连接到特定的 **__tablename__** 的模型，并创建特定的变量。

显而易见，首先我们连接到 Flask 应用程序实例，并通过提供 PosgreSQL URI 来配置 Flask。

然后，我们用 **SQLAlchemy** 创建一个 db 变量，以连接到您的数据库并使用模型创建表。

类通常有关系**,正如这里讨论的那样。**

> ***Flask-****SQLAlchemy****，relationship 方法中的****backref****参数允许你在指定的类下声明一个新的属性，这里将其指定为‘list’***
> 
> ****参数决定了通过关系查询时，相关对象如何加载。lazy = 'select '(或 True)****

## ***烧瓶形***

***这个 **PostForm(FlaskForm)** 类是作为一个附加来更新或修改一个用户已经发表的文章的。简而言之就是一个 CRUD 操作。你可以在这里阅读更多关于它的[。](https://hackersandslackers.com/flask-wtforms-forms/)***

# ***注册/登录/注销逻辑***

***这一部分非常简单，与我之前的文章相似:***

***[](/codex/simple-registration-login-system-with-flask-mongodb-and-bootstrap-8872b16ef915) [## 带有 Flask、MongoDB 和 BootStrap 的简单注册/登录系统

### Flask，Bootstrap，MongoDB 简介

medium.com](/codex/simple-registration-login-system-with-flask-mongodb-and-bootstrap-8872b16ef915) 

主要区别是我们使用关系数据库(+CRUD 操作)并在前端添加了更多功能。

Bear with me index()是运行 **app.py** 时默认运行的，重定向到首页。

> *注意:大部分解释将在 gist 文件中提供。*

在这个项目的主页上已经列出了所有的用户帖子。

Flask **分页**的例子也实现了，这样一个页面就不会有超过 3 篇文章！更多信息请点击查看[。](https://pythonhosted.org/Flask-paginate/)

通过传递用户名、电子邮件和密码来注册用户(显然，我们为了安全起见对其进行了哈希处理！)*** 

# ***前端模板***

***既然逻辑部分已经解决了(至少目前❤是这样)，让我们把注意力放在 UI 上。***

> ******Sidenote*** *:由于我们的主要焦点是 CRUD 操作和数据库关系功能，因此我们不会深入设计前端。****

## ***基础***

***显然，由于我对前端/造型不够精通或没有足够的耐心(目前)，我使用 [Bootstrap](https://getbootstrap.com/) 进行快速定制和设计。***

*****base.html**是一个导航条，在所有其他模板中都有扩展***

## ***主页***

***我知道，我知道造型应该在 styles.css 告我！***

***![](img/2fd5c319965b575c71872e24ed255106.png)***

***搜索表单是所有用户**帖子**的 **CRUD** 操作的一部分，我将在本文的第 2 部分介绍这些用户**帖子**。***

***在 div“容器”中有***

```
*{% for post in posts.items %}*
```

***它将遍历所有用户传入的所有 posts.items。在其中，我们有“卡头”、“卡体”等等及其对应的 post 项目值，如***

```
*{{ post.title }}*
```

***![](img/e5bd7e13109714b0aea468650bdc3dc6.png)***

## ***注册模板***

***我想指出的一点是**get _ flash _ messages()**从会话中提取所有 flash 消息并返回它们。默认情况下，只返回消息，但是当 *with_categories* 设置为`True`时，返回值将是一个格式为`(category, message)`的元组列表。其余的应该像白天一样清楚🌞如果你对 **HTML** 和**表单**的工作原理有基本的了解。***

***![](img/98dd4af06a7268445deb450d185822d7.png)***

***此处用户已经登录，因此 Flash 消息显示为“您已经登录”***

## ***登录模板***

***![](img/bb93dce16737e10e9c56514b87b4e689.png)***

***一个我想让你注意的是在“**你的帖子**”、“**注销**”你可以看到文本颜色变暗了，就像我们之前在**base.html 中谈到的那样。*****

```
*<li class="nav-item">            
<a class="nav-link {{status}}" href="{{ url_for('members')}}" tabindex="-1" >Your Posts</a>
</li>    
<li class="nav-item">
<a class="nav-link {{status}}" href="{{ url_for('logout')}}" tabindex="-1" >Logout</a>
</li>*
```

***在 app . py**status = " disabled "**中，直到有人注册或登录(并且用户会话处于活动状态)！所以这些列表项将不能被点击，因为为什么未注册的用户可以注销或查看自己的帖子呢？***

## ***注销模板***

***没有注销模板！在这个小博客应用项目中，没有理由创建一个 logout.html 模板。在 **app.py** 注销路径*/注销*将被重定向到**登录**页面，显示您已注销，同时禁用“您的帖子”和“注销”。***

***![](img/71260c51664dc11dfe93f0b435a1c95a.png)***

# ***PosgreSQL 用户表单***

***如果您已经成功设置了一个 **PostgreSQL** 数据库和 **SQLAlchemy** ，并添加到您的 Flask 应用程序实例中。***

```
*app.config['SQLALCHEMY_DATABASE_URI'] = 'postgresql://postgres:Password@localhost/Database'*
```

***所以你应该在你的 PostgreSQL 服务器上看到类似的东西。***

***![](img/971095e09595043ca71cfaa08dfcafc8.png)***

***密码被“**哈希化**”，因此它已经变成了自身的一个乱码表示。***

***![](img/bb4d16e099ef138e9e1daa73f30a9ca3.png)***

***由于 **__tablename__** 是由“ **users** ”和“ **posts** ”的名称创建的，所以它也应该在数据库的 Tables 部分中。***

# ***结论***

***在这个 Flask 博客应用项目中，我们已经介绍了很多使用 **PostgreSQL** 作为存储用户的关系数据库的内容。***

***在下一部分中，我们将深入探讨如何在用户和帖子表之间建立一对多的数据库关系，并为用户帖子提供全功能的 CRUD 操作。您可以在下面找到它:***

***[](https://richard-taujenis.medium.com/one-to-many-database-relationship-crud-with-postgresql-and-flask-part-2-3e3e78255ef2) [## 与 PostgreSQL 和 Flask 的一对多数据库关系(+CRUD)第 2 部分

### 在第 2 部分中，我们将讨论数据库关系(一对多)。过账、编辑、删除和搜索(CRUD 操作)

richard-taujenis.medium.com](https://richard-taujenis.medium.com/one-to-many-database-relationship-crud-with-postgresql-and-flask-part-2-3e3e78255ef2) 

以下你可以找到 Github 回购❤:

[](https://github.com/Mozes721/Flask_Blog_App) [## Mozes721/Flask_Blog_App

### 此时您不能执行该操作。您已使用另一个标签页或窗口登录。您已在另一个选项卡中注销，或者…

github.com](https://github.com/Mozes721/Flask_Blog_App)***