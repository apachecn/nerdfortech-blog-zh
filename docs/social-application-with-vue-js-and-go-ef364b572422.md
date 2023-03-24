# 带有 Vue.js 和 GO 的社交应用

> 原文：<https://medium.com/nerd-for-tech/social-application-with-vue-js-and-go-ef364b572422?source=collection_archive---------9----------------------->

![](img/4cdfb643ef90df60dcd8aaa1429ba169.png)

标志归功于 vuejs.org 和 golang.org

## 用 vue.js 和 golang 创建和服务一个类似 twitter 的应用程序第 5 部分:VUEX 终结

这是本系列的第五部分。在这里检查所有零件:

*   [第一部分:设置](https://ivano-dalmasso.medium.com/social-application-with-vue-js-and-go-4e4db0cdde64)
*   [第二部分:VUE 入门](https://ivano-dalmasso.medium.com/social-application-with-vue-js-and-go-64b3adee8dac)
*   [第三部分:组件&插槽](https://ivano-dalmasso.medium.com/social-application-with-vue-js-and-go-24a1d1e7137d)
*   [第 4 部分:Vuex 首次设置](/nerd-for-tech/social-application-with-vue-js-and-go-3a11d506fc38)
*   第五部分:Vuex 终结(本)
*   [第 6 部分:Vuex 中的表格和数据](https://ivano-dalmasso.medium.com/social-application-with-vue-js-and-go-a22a1afb76eb)
*   [第七部分:与 golang 服务器的连接](https://ivano-dalmasso.medium.com/social-application-with-vue-js-and-go-d9e563466b66)
*   [第 8 部分:基于令牌的认证](https://ivano-dalmasso.medium.com/social-application-with-vue-js-and-go-64978f7c381f)
*   [第 9 部分:存储索引为 DB 的认证令牌](https://ivano-dalmasso.medium.com/social-application-with-vue-js-and-go-4d0caa37ddac)

在本课中，我们将更新 vuex 商店，我们将有一个更好的逻辑来维护用户的身份验证。然后我们将更新应用程序以使用我们创建的 vuex 商店

这里的主要目标是更新前端项目，你可以在这里找到代码。

## 改善 vuex 商店

在第一部分中，我们对上节课中写的 vuex 商店做了一点修改。我们想要做的是从用户存储中删除登录和注销逻辑，这样将只包含用户数据列表，并将其放入另一个存储中，仅用于身份验证目的。

因此，只需创建一个文件夹" **src/store/authStore** "，并将之前在用户存储中绑定到身份验证的代码移到其中:

```
export default {
  namespaced: true,
  state: {
    user: {
      id: 1,
      username: "idalmasso",
      description: "Here is the description",
      //here there will be the logic for auth and so on...
      loggedIn: false
    }
  },
  mutations: {
    LOGIN(state) {
      state.user.loggedIn = true;
    },
    LOGOUT(state) {
      state.user.loggedIn = false;
    }
  },
  actions: {
    async login(context) {
      context.commit("LOGIN");
    },
    async logout(context) {
      context.commit("LOGOUT");
    }
  },
  getters: {
    currentUser(state) {
      return state.user;
    },
    isLoggedIn(state) {
      if (!state.user) return false;
      return state.user.loggedIn;
    }
  }
};
```

只是在这上面做了一点重构，代码和以前一样。现在，用户存储也可以这样更改:

```
export default {
  namespaced: true,
  state: {
    loadedUsers: [
      {
        id: 1,
        username: "idalmasso",
        description: "Here is the description",
        //here there will be the logic for auth and so on...
        loggedIn: false
      }
    ]
  },
  mutations: {
    ADD_USER(state, user) {
      if (state.loadedUsers.some(u => u.username == user.username)) {
        state.loadedUsers.splice(
          state.loadedUsers.indexOf(u => u.username == user.username),
          1
        );
      }
      state.loadedUsers.push(user);
    }
  },
  actions: {},
  getters: {
    getUser: state => userid => {
      if (state.loadedUsers.some(user => user.username == userid)) {
        return state.loadedUsers.find(user => user.username == userid);
      } else {
        //Here I'll have to request from the server!!
        return {};
      }
    }
  }
};
```

在这里，我们已经删除了认证代码，并添加了 ADD_USER 变体，这将允许我们向状态数据中的数组添加用户，并添加了一个 getter“getUser”:这将返回一个函数，该函数从 *userid* 返回来自商店的相应用户。为了简单起见，我们使用用户名作为 id，在实际应用中，您可能更喜欢使用某种 id。

请注意，在这个应用程序中，我们将每次从数据库中插入所有用户和所有帖子。这也只是为了简单起见，在实际的应用程序中，您可能希望从服务器获取并只维护一些数据(例如，您关注的人的最新帖子、最后查看的用户等)。)

最后，记得将导入模块添加到通用 index.js 存储文件中的“ **src/store/index.js** ”中。

## 更改组件以反映商店的变化

首先，让用户存储现在包含用户，我们可以更改路由器以确保它可以显示具有不同用户名参数的用户组件。为此，只需将用户路由的路径从“**/用户**”更改为“**/用户/:用户标识**”。通过这种方式，userid 将作为 prop 传递给用户组件，并且也将出现在 url 中，因此如果用户想要“共享”某个页面，不管出于什么原因，他都可以直接复制并粘贴链接。

现在，在用户组件中，我们可以添加道具配置:

```
props: {
 userid: { type: String, default: “” }
 },
```

然后更改“ *user* ”计算方法，从商店中返回用户，如下所示:

```
 return this.$store.getters[“users/getUser”](this.userid);
```

这将从 store getters 中获取函数“ *getUser* ”，并使用参数“ *this.userid* ”调用它。

现在，我们想把对这个路由的请求修改成包含这个 *userid* 参数。

我们想改变的第一个地方是应用程序栏。这实际上非常简单:我们只需在数据配置选项中更改到用户视图的链接，并使它具有一个 userid 参数。这可以通过以下方式轻松实现:

```
links: [
       ...
        {
          name: "User",
          to: {
            name: "User",
            params: {
              userid: this.$store.getters["auth/currentUser"].username
            }
          }
        }
      ]
```

这段代码从 *auth* 存储的" *currentuser* " getter 获取参数数据，并使用它路由到用户组件。很简单，对吧？

现在可以稍微扩展一下这个东西，确保所有的帖子标题都变成了链接，链接到实际写帖子的用户。我们在 SinglePost 组件中这样做，首先简单地添加一个“ *linkUser* ”方法，如下所示:

```
linkUser(username) {
      return {
        name: "User",
        params: {
          userid: username
        }
      };
    }
```

该方法构建路由器请求的链接对象，因此它可以使用用户名参数导航到正确的页面。通过这种方式，我们只需要对模板中的卡片标题做一点小小的修改，就可以让它工作了

```
<template v-slot:header>
      <h3>
        <router-link :to="linkUser(post.user)">
         {{ postTitle(post)}}
        </router-link>
      </h3>
</template>
```

这将把每篇文章的标题更新为指向用户页面的链接。在评论卡中，也可以用同样的方式轻松更新评论标题:

```
<template v-slot:header>
          <h3>
            <router-link :to="linkUser(comment.user)">
             {{ postTitle(comment)}}
            </router-link>
          </h3>
</template>
```

正如你所看到的，代码几乎是一样的，只是因为我们保持了 comment 和 post 对象的结构非常相似。

现在，您可以尝试使用存储，在用户存储数据对象中添加一些用户，在帖子存储中添加这些用户的一些帖子，并查看链接实际上是否工作正常。此外，应用程序栏中的链接会将您转到授权存储中用户的用户页面。

最后，让我们开始在应用程序栏中添加一些功能，以利用身份验证存储。

在上一课中，我们添加了一个“登录”按钮，它根本不起任何作用。现在，在 applicationbar 组件中添加几个方法来模拟登录/注销功能:

```
login() {
 this.$store.dispatch("auth/login");
},
logout() {
  this.$store.dispatch("auth/logout");
}
```

和一个计算方法，它告诉我们当前用户是否实际登录:

```
loggedIn() {
  return this.$store.getters["auth/isLoggedIn"];
}
```

我们可以使用这些来显示和获取模板中的登录/注销行为，将之前的登录按钮更改为:

```
<div class="right-links ">
      <a class="app-bar-item" href="#" v-if="!loggedIn" @click.prevent="login">LOGIN</a>
      <a class="app-bar-item" href="#" v-if="loggedIn" @click.prevent="logout">LOGOUT</a>
</div>
```

如果 loggedIn computed 方法为 false，则此模板显示登录按钮，否则显示注销按钮。@click 是我们之前看到的事件的普通 v-on 处理程序。

的。处理程序上的 prefix 实际上是一个修饰符，它确保如果有一些“标准”行为被调用，它就不会被调用，所以如果这个按钮是表单上的一个 submit 按钮，那么单击这个按钮不会触发 get 请求，也不会触发来自服务器的页面刷新(在这个实际的例子中，这是。不需要防止)。

因此，现在当按下登录按钮时，身份验证操作异步执行，当状态实际发生变化时，getter 方法(它是反应性的)将更改用于定义页面行为的 computed 属性的值，隐藏登录按钮并显示注销按钮。

我们实际上添加了两个方法，login 和 logout，以及一个计算属性，isLoggedIn，它们实际上是动作的包装器和存储的 getters。这是一个常见的过程，所以 vuex 有一些帮助器函数来简化这个过程，mapGetters 和 mapActions。

因此，我们可以做最后一个更改，这不会改变应用程序的行为，而只会使代码更加清晰和紧凑:只需在导出 applicationBar 组件之前导入两个函数:

```
<script>
import { mapGetters } from "vuex";
import { mapActions } from "vuex";
export default {
...
```

现在，在配置选项中使用这些助手，如下所示:

```
methods: {
    ...mapActions({
      login: "auth/login", 
      logout: "auth/logout"
    })
  },
  computed: {
    ...mapGetters({ loggedIn: "auth/isLoggedIn" })
  }
```

因此，“ *auth/login* 动作链接到了 *login* 方法、“ *auth/logout* 到 *logout* 和“*auth/isLoggedIn*”getter 链接到了“*logged in*computed 属性。

这样，我们就有了一个很好的主干来用于接下来的步骤，在下一课中，我们将最终创建一个输入系统来创建帖子和评论，并将它们推送到商店中。