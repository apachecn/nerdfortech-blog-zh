# 带有 Ruby Sinatra 和 RESTful 路线的应用程序，一个简单的故障

> 原文：<https://medium.com/nerd-for-tech/apps-with-ruby-sinatra-and-restful-routes-a-simple-breakdown-4ca1432dee3?source=collection_archive---------14----------------------->

![](img/d95554ce77a6f6b1792b1ae7782e4189.png)

我爱建筑！无论是纳米块，金属地球模型，还是程序，我就是喜欢建筑。最近有人向我介绍了针对 Ruby 的 Sinatra 库，这绝对令人兴奋。

对于任何初学编码的人(比如我自己)，库是由非常非常慷慨的编码者制作的一组代码。库让我们的生活变得更容易，因为我们不必担心重新定义和重复几乎每个应用程序中都存在的基本代码。Sinatra 是一个用于构建基本网站的库。

然而，仅仅被给予一个库绝对是一个冒险的举动。首先，我只知道西纳特拉图书馆如何运作的最基本的知识。我只是跟着代码走，所以我很害怕，我不知道如何利用这些知识来编写我自己的 Sinatra 网站。有什么比写一篇关于辛纳屈的文章更好的方法来了解辛纳屈呢？

# 装置

和任何 gem(Ruby 库的名字)一样，我们必须安装它！

在我们的终端中，我们运行`**gem install sinatra**`。

在我们需要使用 Sinatra 的任何文件中，我们只需在文件顶部添加`**require ‘sinatra’**`。

# **宁静的航线**

你能想象作为 X 公司的开发人员，你会像 companyx.com/codingproject/title/edit/user3/blah/bluh.一样写路线(用户会看到的 URL ),然后你不得不写另一个可能有类似功能的路线，看起来一点也不像第一个。呀。

现在，假设你要调到 Z 公司，因为你无法忍受 X 公司让你写路线的方式…却发现 Z 公司没有一种好的方式来写容易编码的路线。大呀。

2000 年，Roy Fielding 用他的 REST 概念改变了这一切。REST 代表代表性状态转移。这一切都可以归结为拥有实际代表服务器试图向用户显示的内容的路由。对我们来说，这听起来像是常识，但当互联网还是一个新现象时，情况并非如此。

有 7 条 RESTful 路径，它们对应于我们与数据交互的四种方式，CRUD(创建、读取、更新、删除)。

# 污垢的产生

你注册过网站吗？赌马怎么样？还有 Instagram？

当我们在网站上注册时，我们首先被带到一个页面，填写关于我们的信息。然后，我们将信息提交到网站，以便我们可以收到该网站提供的产品或服务。任何时候你在一个网站上提交信息，你都是在向那个网站提交表格！有了表单中的信息，网站可以将你保存到他们的数据库中。

CRUD 的 C 需要两条路线才能完全发挥作用。让我们在整个例子中使用神奇宝贝！在我的项目中，明星类是入门类、组织类和任务类，但是它们没有神奇宝贝有趣。我喜欢谈论神奇宝贝，所以让我们在下面的例子中继续使用神奇宝贝。

![](img/f4470a8ed49be4c2cc72f1d028bf6753.png)

迈克尔·里维拉·🇵🇭在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

让我们想象我们有一个存在于`**pokemon.rb**`中的神奇宝贝类。这个 Pokémon 类将继承自`**ActiveRecord::Base**`(另一个帮助我们定义与数据库交互的方法的库。

神奇宝贝类看起来会像这样。

```
**class Pokemon < ActiveRecord::Base
end**
```

通过从 ActiveRecord 继承，我们的神奇宝贝类将可以访问像`**.create**`这样的方法，它会自动将我们的实例保存到数据库中！但是本文不会集中讨论 ActiveRecord。

无论如何，回到 CRUD 的两条路线。它们是:

```
**get ‘/pokemon/new’ do
  erb :new
end**
```

和

```
**post ‘/pokemon’ do
  @pokemon = Pokemon.create(params)
  redirect to "/pokemon/#{@pokemon.id}"
end**
```

你可以想象你可以用任何你想要的职业来代替神奇宝贝。这就是休息的魔力。

`**get**`方法来自 Sinatra 库。每当看到 get 时，可以放心地假设您只是向用户显示一些东西。`**erb**`也是一种 Sinatra 方法，它接受一个符号作为输入。您通常希望用作输入的是您将显示的表单或显示该表单的页面。

`**params**`是 Sinatra 给我们的一个实例方法，它有一个 hash 的返回值。您可以把它想象成一个散列，存储来自用户填写的表单的信息。表单是另一个主题，很难在一篇文章中解释所有的内容。但是您可以想象 params 方法将检索用户输入的所有信息。根据这些输入，我们可以创建我们的神奇宝贝实例。

`**redirect to**`类似于`**erb**`，因为它是一个向用户显示响应的方法。一个很大的区别是`**erb**`不改变用户的 URL，而 redirect to 将改变 URL 以匹配输入的 URL。`**redirect to**`接受您定义的`**get**`路线。

我们在方法中使用实例变量，因为如果我们只使用`**redirect to “/pokemon/#{pokemon.id}”**`，`**“/pokemon/#{pokemon.id}”**`路线在技术上不知道神奇宝贝是什么，因为该路线在技术上是一个不同的方法，所以我们希望与单独的方法共享神奇宝贝信息。

# 污垢的读数

想想 Instagram。你可以在一个用户的主页上看到她所有的图片，或者你可以转到一个特定的图片，在它自己的单独页面上欣赏它的光辉。因此，CRUD 的其余部分也将采用 2 条路线，一条用于所有神奇宝贝实例，另一条仅用于一个特定的神奇宝贝实例。它将是:

```
**get ‘/pokemon’ do
  @pokemon = Pokemon.all
  erb :index
end**
```

和

```
**get ‘/pokemon/:id’ do
  @pokemon = Pokemon.find_by(id: params[:id})
  erb: show
end**
```

第一种方法是为神奇宝贝的所有实例设置一个实例变量，这个实例变量将被转移到`**index.erb**`文件中。`**index.erb**`文件将列出所有的实例。

第二种方法是显示神奇宝贝的一个具体实例。根据`**show.erb**`文件的布局，页面可能会显示具体的神奇宝贝的类型、动作和图片。

# CRUD 的更新

更新就是编辑。在应用程序可以进行任何编辑之前，应用程序必须知道要编辑什么。因此，首先我们必须向用户展示他可以用来编辑的表单。它看起来会像这样:

```
**get ‘/pokemon/:id/edit’ do
  @pokemon = Pokemon.find_by(id: params[:id])
  erb :edit
end**
```

在这个路径中，我们通过`**params**`方法从 URL 请求`**id**`值。一旦用`**find_by**`方法找到了神奇宝贝，我们就可以渲染`**edit.erb**`文件。

一旦用户进行编辑，信息将通过`**patch**`路径:

```
**patch ‘/pokemon/:id/edit’ do
  pokemon = Pokemon.find_by(:id params[:id])
  pokemon.update(params)
  redirect to '/pokemon/#{pokemon.id}'
end**
```

这条路线与前一条路线相似，它也将尝试通过 URL 中的`**params**`找到神奇宝贝。下一行，我们将使用 ActiveRecord 的`**update**`方法来元更新每个属性(就像在 Create route 中那样)。然后我们会将用户重定向到`**/pokemon/#{pokemon.id}**`路线。我们必须插入`**pokemon.id**`部分，因为`**redirect to**`方法的输入是一个字符串！我们使用`**redirect to**`是因为我们想向用户展示一个不同的 URL。如果我们只使用`**erb**`方法，URL 将保持为`**/pokemon/:id/edit**`。

此时，你可能会奇怪，“为什么我们每次都要找到神奇宝贝？”

当我们浏览互联网时，浏览器似乎不会在我们每次进入新页面时都要求我们登录，这对我们来说似乎是不可思议的。然而，互联网被创造成无状态的，这意味着每一个新的页面本质上都是一个新的页面！应用程序必须通过在每个请求中请求神奇宝贝的 id 来弥补这一点。

但是浏览器“记忆”魔力的 TLDR 是会话和 cookies 的参与。Cookies 是你访问每个网站的通行证。每次去一个网站，你的护照都会被“盖章”。浏览器会查看你的护照，以确定你是谁，你应该去哪里。

![](img/1e6ed41422a70c4f1d7a5dcfe9ea8167.png)

照片由[梅根·麦克莱恩](https://unsplash.com/@nutmeganginger?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# CRUD 的删除

所以 CRUD 有 7 个路径，我们到目前为止已经讨论了 6 个，这意味着删除路径只有一个方法:

```
**delete ‘/pokemon/:id/delete’ do
  pokemon = Pokemon.find_by(id: params[:id])
  pokemon.delete
  redirect to '/pokemon'
end**
```

到目前为止，这一切似乎都很熟悉。我们不需要实例变量，因为我们正在重定向，也因为我们正在删除记录！如果我们只是要删除一个局部变量，就不需要给它赋值。

在我们从`**params**`方法中找到带有`**id**`值的神奇宝贝后，我们将使用 ActiveRecord `**delete**`方法从我们的数据库中删除记录。根据您对应用程序的期望，您可以将用户重定向到其他路径，而不是`**/pokemon**`路径，但是这样做只是惯例。

![](img/658497250f7926aa2687daad4720237a.png)

Thimo Pedersen 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# **结论**

重要的是要记住，如果没有合适的数据库和详细的表单，动态 web 应用程序将很难维护。

从我自己的 Sinatra 项目中，我了解到很难在第一次尝试中为我的表获得完美的模式。

完善你的表格的秘密是使用`**pry**`宝石，并实际查看`**params**`的返回值是多少！在使用正确的语法方面有很多尝试和错误，但是写这篇文章以及编写我的项目确实帮助我维护了 RESTful 约定。

最终，这都是关于关注点的分离。REST 在区分哪些文件夹应该包含哪些文件，哪些文件应该包含哪些类/方法方面非常有帮助。也就是说，不要忘记使用 MVC(模型、视图、控制器)概念！