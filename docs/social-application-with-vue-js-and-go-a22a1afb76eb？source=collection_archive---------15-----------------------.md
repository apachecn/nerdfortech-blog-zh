# 使用 Vue.js 和 GO 的社交应用

> 原文：<https://medium.com/nerd-for-tech/social-application-with-vue-js-and-go-a22a1afb76eb?source=collection_archive---------15----------------------->

![](img/4cdfb643ef90df60dcd8aaa1429ba169.png)

标志归功于 vuejs.org 和 golang.org

## 用 vue.js 和 golang 创建和服务一个类似 twitter 的应用程序第 6 部分:VUEX 中的表单和数据

这是本系列的第六部分。在这里检查所有零件:

*   [第一部分:设置](https://ivano-dalmasso.medium.com/social-application-with-vue-js-and-go-4e4db0cdde64)
*   [第二部分:VUE 入门](https://ivano-dalmasso.medium.com/social-application-with-vue-js-and-go-64b3adee8dac)
*   [第三部分:组件&插槽](https://ivano-dalmasso.medium.com/social-application-with-vue-js-and-go-24a1d1e7137d)
*   [第 4 部分:Vuex 首次设置](/nerd-for-tech/social-application-with-vue-js-and-go-3a11d506fc38)
*   [第 5 部分:Vuex 终结](/nerd-for-tech/social-application-with-vue-js-and-go-ef364b572422)
*   第 6 部分:Vuex 中的表单和数据(this)
*   [第七部分:与 golang 服务器的连接](https://ivano-dalmasso.medium.com/social-application-with-vue-js-and-go-d9e563466b66)
*   [第 8 部分:基于令牌的认证](https://ivano-dalmasso.medium.com/social-application-with-vue-js-and-go-64978f7c381f)
*   [第 9 部分:存储索引为 DB 的认证令牌](https://ivano-dalmasso.medium.com/social-application-with-vue-js-and-go-4d0caa37ddac)

在本章中，我们将最终创建一些表单，这样我们就可以使用之前创建的存储，并在其中插入/更新一些数据。请注意，在本部分的最后，我们将有一个应用程序，它有一个客户端内存存储，我们可以在其中添加和更新值。此外，我们还没有一个持久的数据层:为此，我们将不得不等待更多的教训。

这里的主要目标是更新前端项目，你可以在这里找到代码。

## 商店的更新

在插入实际的表单之前，让我们对 post store 做一些最后的更新，以便可以接受帖子和评论的插入。

首先，删除状态列表中的所有 post 对象，这样它将作为一个空数组开始。然后，更新 ADD_POST 变体，使其也有一些逻辑来填充帖子的 id(记住:当有一个工作的后端时，它将不再被使用)，并添加一个变体来为帖子添加评论，如下所示:

```
ADD_POST(state, post) {
      if (state.posts.length < 1) {
        post.id = 1;
      } else {
        const max = state.posts.reduce((prev, current) =>
          prev.id > current.id ? prev : current
        );
        post.id = max.id + 1;
      }
      post.comments = [];
      state.posts.push(post);
    },ADD_COMMENT(state, { postId, comment }) {
      const post = state.posts.find(post => post.id === postId);
      if (post.comments.length < 1) {
        comment.id = 1;
      } else {
        const max = post.comments.reduce((prev, current) =>
          prev.id > current.id ? prev : current
        );
        comment.id = max.id + 1;
      }
      post.comments.push(comment);
    } 
```

这里的代码非常简单。它为每个插入的帖子分配一个 id，增量。第二个突变对评论也是如此。在 actions 中，在调用 ADD_POST 突变之前，还要插入帖子的日期，还要创建一个 action，可以用一个 comment 做同样的事情，并调用相应的突变:

```
async addPost(context, post) {
      post.date = getFormattedDate();
      context.commit("ADD_POST", post);
},
async addComment(context, { postId, comment })
{
      comment.date = getFormattedDate();
      context.commit("ADD_COMMENT", { postId, comment });
}
```

“ *getFormattedDate* ”函数实际上是我之前在同一个文件中创建的一个实用函数，只是为了给帖子和评论添加一个格式良好的日期。

## 更新帖子视图

现在我们有了一个可以添加帖子和评论的商店，我们必须在组件中实际使用这些新功能。

让我们添加一个新的组件，它可以用于输入单个文本，并且可以重用。在 *src/components* 文件夹中添加一个新文件 *AddTextForm.vue* ，并填入以下内容:

```
<template>
  <form>
    <label v-if="showLabel" :for="'text' + this._uid">
     {{ textRequest }}</label>
    <input
      :id="'text' + this._uid"
      type="text"
      :placeholder="textRequest"
      v-model="textValue"
    />
    <button @click.prevent="submitted">submit</button>
  </form>
</template>

<script>
export default {
  data() {
    return {
      textValue: ""
    };
  },
  props: {
    textRequest: { type: String, default: "" },
    showLabel: { type: Boolean, default: false }
  },
  methods: {
    submitted() {
      this.$emit("text-added", this.textValue);
      this.textValue = "";
    }
  },
  emits: ["text-added"]
};
</script>

<style scoped>
label {
  padding-right: 1rem;
  display: block;
}
button {
  margin-top: 1rem;
  width: 10%;
  border-top-right-radius: 8px;
  border-bottom-right-radius: 8px;
  background-color: darksalmon;
  padding: 8px;
}
input {
  box-sizing: border-box;
  border-top-left-radius: 8px;
  border-bottom-left-radius: 8px;
  width: 80%;
  padding: 8px 20px;
}
input:focus {
  outline: 0;
  background-color: wheat;
}
</style>
```

这里一切都应该很清楚:我们使用了一对道具，一个决定是否在表单中显示标签，另一个决定要显示的文本框的占位符。

这个文件中唯一的新东西实际上是使用了" *v-model* "指令，它与输入字段一起使用，以一种反应的方式将输入中的值实际连接到组件数据中的变量。

在按钮的 clicked 事件中，调用的方法向任何使用它的父组件发出一个事件" *text-added* "。另外，请注意，我们实现了一个“*发射*”配置参数，以让父节点知道，这不是强制性的，而是一个很好的最佳实践。

这个事件发射为我们提供了 vue 中组件之间的第二种通信方式，第一种是“ *props* ”，因此我们可以从父组件向子组件发送数据，反之亦然。

这样，任何使用 AddTextForm 的组件都将能够监听事件"*文本添加的*"并获得插入的数据。

现在，让我们使用这个表单在 Posts.vue 组件中添加一些文章。更新模板，像这样

```
<div class="posts">
    <h1>All Posts</h1>
    <add-text-form
      textRequest="Add Post"
      v-if="loggedIn"
      :showLabel="true"
      @text-added="addPost"
    ></add-text-form>
    <post-list :posts="posts" title="" />
  </div>
```

所以，这一切都像以前一样，但我们添加了一个小标题和新的组件。只有当用户登录时，我们才显示这个组件，我们显示一个标签“Add Post ”,在事件“text-added”时，我们调用 addPost 方法。这其实很简单:

```
methods: {
    addPost(text) {
      this.$store.dispatch("posts/addPost", {
        user: this.$store.getters["auth/currentUser"].username,
        post: text
      });
    }
  }
```

因此，这只是分派 addPost Post，传递实际的认证用户用户名和文本 post。这将在操作中使用，并将文章添加到实际商店中。

下一步自然是使用相同的 add-text-form 向帖子添加评论。这可以分两步完成。首先，我们必须更改 BaseCard 模板，这样它也可以有一些“actions”插槽，我们可以在其中插入一些其他代码。这很容易做到，在页脚后添加:

```
<div class="actions">
   <slot name="actions"></slot>
</div>
```

然后，我们可以在 SinglePost 组件中使用此插槽，在 BaseCard 的结束标记之前的页脚后添加以下行:

```
<template v-if="loggedIn" v-slot:actions>
   <add-text-form
        textRequest="Add comment"
        :showLabel="false"
        @text-added="text => addComment(text, post)">
    </add-text-form>
</template>
```

注意，为此我们必须向存储的 loggedIn getter 添加一个 mapGetter，以便仅在用户实际登录时呈现按钮。

在事件“text-added”的发射上，它被称为函数 *addComment* ，我们必须像这样添加到方法中:

```
addComment(text, post) {
      this.$store.dispatch("posts/addComment", 
          {
             postId: post.id,
             comment: 
             {
                user: this.currentUser.username,
                post: text
            }
      });
```

这显然会将评论添加到商店中，因此其他页面可以在需要时加载评论。

## 添加登录页面

到目前为止，登录按钮只是将实际用户的登录状态从“已登录”转换为“未登录”。将来，用户将不得不通过用户/密码表单请求登录，所以现在让我们创建一个登录页面来完成这项工作。现在它将是一个占位符，只有一个“登录”按钮，但它将很快完成其他功能。

在视图文件夹中，使用以下内容创建 Login.vue 文件:

```
<template>
  <div class="login">
    <button @click="loginButtonClicked">LOGIN</button>
  </div>
</template>

<script>
import { mapActions } from "vuex";
export default {
  methods: {
    ...mapActions({ login: "auth/login" }),
    loginButtonClicked() {
      this.login().then(() => {
        this.$router.push({ name: "Posts" });
      });
    }
  }
};
</script>

<style scoped>
button {
  margin-top: 1rem;
  width: 20rem;
  height: 5rem;
  border-radius: 8px;
  background-color: darksalmon;
  padding: 8px;
}
</style>
```

这里没有什么新东西，只需要注意" *router.push* "指令，它会在登录存储操作成功完成后将浏览器重定向到 Posts 视图。

要完成这一步，我们现在只需更改应用程序栏，当按下登录按钮时，应用程序栏必须导航到登录视图。为此，只需稍微修改一下登录链接，从一个“*标签和一个*标签改为一个路由器链接，该链接指向登录视图:

```
<router-link
        class="app-bar-item"
        href="#"
        v-if="!loggedIn"
        @click.prevent :to="{ name: 'Login' }"LOGIN        >LOGIN</router-link>
```

此外，为了完成此更新，我们可以确保当用户从应用程序注销时，会自动转到登录页面，并将此新方法附加到注销链接点击事件:

```
logoutButtonClicked() {
      this.logout().then(() => {
        this.$router.push({ name: "Login" });
      });
    }
```

最后要做的是，更新路由器文件以添加新的“登录”路由，如下所示:

```
import Login from "../views/Login.vue";...
{
     path: "/login",   
     name: "Login",    
     component: Login
}
```

## 在登录状态下增加一些导航守卫

如果用户没有登录，我们希望用户不能进入某些页面(例如，用户页面)。因此，首先，我们可以转到应用程序栏组件，如果用户未被启用，则只显示一些链接:在模板中，将实际存在的 *v-for="link in links"* 更改为*v-for = " link in active links "*，并添加如下的 *activeLinks* 计算方法:

```
activeLinks() {
      return this.links.filter(
        link => link.visibleIfLoggedOut || this.loggedIn
      );
    }
```

这样，所显示的链接只有在用户注销或登录时才可见。我们还应该将属性"*visibleifloggeout*"添加到链接中，如下所示:

```
links: [
        {
          visibleIfLoggedOut: true,
          name: "Posts",
          to: { name: "Posts" }
        },
        {
          visibleIfLoggedOut: false,
          name: "User",
          to: {
            name: "User",
            params: {
              userid: this.$store.getters["auth/currentUser"].username
            }
          }
        }
```

如果用户被注销，这段代码实际上会隐藏“用户”的链接，但这实际上并不是我们需要做的全部。事实上，如果用户将用户页面的 url 放在浏览器的地址栏中，就会看到该页面。在这种情况下，我们需要确保路由器进行某种处理来决定用户是否可以查看某些页面。

这实际上是通过导航卫士来完成的:这些配置方法可以用来指示路由器在导航事件调用之前和之后要做的事情。

这些可以在路由或全局级别设置，在这种情况下，例如，我们将在用户路由上设置导航保护，告知如果用户实际上没有登录，则应该将其路由到登录页面。同样，我们希望已经登录的用户不要转到登录页面，而是被重定向到例如 posts 页面。

在路由器文件中，更新两条路由，如下所示:

```
import store from "@/store/index.js";
...
  {
    path: "/user/:userid",
    name: "User",
    component: User,
    props: true,
    beforeEnter: (to, from, next) => {
      if (!store.getters["auth/isLoggedIn"]) {
        next({ name: "Login" });
      } else {
        next();
      }
    }
  },
  {
    path: "/login",
    name: "Login",
    component: Login,
    //This is not needed right by now, because the store is  refreshed on page refresh... Will be needed!
    beforeEnter: (to, from, next) => {
      if (store.getters["auth/isLoggedIn"]) {
        next({ name: "Posts" });
      } else {
        next();
      }
    }
  }
```

beforeEnter 配置实际上是一个具有三个输入参数的函数:

1.  to:被导航到的目标[路线对象](https://router.vuejs.org/api/#the-route-object)。
2.  出发地:当前被导航离开的路线
3.  下一个:解析导航的函数。当它被调用时，取决于传递给它的参数，解析将会不同:如果没有参数被传递，解析将是正常的解析，如果我们传递给它一个 route 对象，这将被推入浏览器的历史中(在我们的例子中，我们将去那里，而不是正常的路由)。

因此，在我们的例子中，我们可以对这两条路线有我们想要的行为。显然，没有什么可以阻止恶意用户强制请求，如果他真的想要的话，所以也要记住在后端复制所有的“安全”控制！

现在，我们已经在前端达到了一个几乎停止的点，所以下一步将开始添加一个 go 后端，它将为一些 api 提供实际的帖子，以及我们需要的身份验证功能。