# 带有 Vue.js 和 GO 的社交应用

> 原文：<https://medium.com/nerd-for-tech/social-application-with-vue-js-and-go-3a11d506fc38?source=collection_archive---------11----------------------->

![](img/4cdfb643ef90df60dcd8aaa1429ba169.png)

标志归功于 vuejs.org 和 golang.org

## 使用 vue.js 和 golang 创建和服务一个类似 twitter 的应用程序第 4 部分:VUEX 首次设置

这是本系列的第四部分。在这里检查所有零件:

*   [第一部分:设置](https://ivano-dalmasso.medium.com/social-application-with-vue-js-and-go-4e4db0cdde64)
*   [第二部分:VUE 入门](https://ivano-dalmasso.medium.com/social-application-with-vue-js-and-go-64b3adee8dac)
*   [第三部分:组件&插槽](https://ivano-dalmasso.medium.com/social-application-with-vue-js-and-go-24a1d1e7137d)
*   第 4 部分:Vuex 首次设置(本)
*   [第 5 部分:VUEX 终结](https://ivano-dalmasso.medium.com/social-application-with-vue-js-and-go-ef364b572422)
*   [第 6 部分:Vuex 中的表格和数据](https://ivano-dalmasso.medium.com/social-application-with-vue-js-and-go-a22a1afb76eb)
*   [第七部分:与 golang 服务器的连接](https://ivano-dalmasso.medium.com/social-application-with-vue-js-and-go-d9e563466b66)
*   [第 8 部分:基于令牌的认证](https://ivano-dalmasso.medium.com/social-application-with-vue-js-and-go-64978f7c381f)
*   [第 9 部分:存储索引为 DB 的认证令牌](https://ivano-dalmasso.medium.com/social-application-with-vue-js-and-go-4d0caa37ddac)

在本课中，我们将添加 vuex，这是一个集中式数据存储。这将使我们有可能从整个应用程序中获取和放置数据。

这里的主要目标是更新前端项目，你可以在这里找到代码。

## vuex 是什么？

在实际开始工作之前，让我们解释一下什么是 vuex，它能为我们的应用做些什么。

Vuex 是一个状态管理库，它集中了应用程序的一些数据，因此可以从任何组件访问它。

这真的很有用，因为只使用道具和事件来传递数据对于组件到组件的传输来说很好也很容易，但是对于任何具有多层组件的应用程序来说，这将变得非常复杂。

这种复杂性可以通过使用集中存储来解决，在本例中是 vuex。

在这一课中，我们将在这个商店中插入文章列表，我们将看到如何以一种简单和模块化的方式设置它。

vuex 商店的配置将位于专用的“商店”文件夹中，如果您在项目设置开始时使用 vuex，vue cli 已经为您添加了该文件夹。如果没有，您可以使用 npm install 安装它并创建存储文件夹。

文件夹中有一个 index.js 文件，其中包含商店的设置和配置。应该是这样的:

```
import Vue from 'vue'
import Vuex from 'vuex'Vue.use(Vuex);export default new Vuex.Store({
  state: {  },
  mutations: {  },
  actions: {  },
  modules: {  }
});
```

它指导 Vue 如何使用 vuex，然后导出一个新的 vuex 存储。它将带有以下参数的配置对象传递给构造函数:

1.  状态:vuex 存储中的实际“数据”(就把它想象成一个组件的“数据”参数)。vue 使状态变得被动，因此数据的每次更改也会反映到组件中。
2.  突变:必须调用的同步函数列表，以改变状态中的一些数据。每个函数都获得实际的 vuex 状态和一个有效负载(它可以是一个自定义对象)。一个变异实际上叫做把它的名字传递给存储的“提交”方法。
3.  动作:另一个函数列表。这些必须实际调用一个突变来改变一些数据，但是可以是异步的。实际上，你不应该从一个组件中调用一个突变，而应该从动作中调用，并从组件中调用最后一个。
    动作在输入中获取一个上下文对象，该对象包含与作为第一个参数的商店实例相同的一组方法/数据，还可以获取一个自定义对象作为第二个参数。一个动作实际上被称为在存储的“dispatch”方法中将名称作为参数传递。
4.  getters:实际上作为 vuex 存储的计算属性的函数列表。
5.  模块:vuex 模块列表。在更大的应用程序中，存储会变得非常大，所以最好将不同应用程序范围的数据和代码拆分到不同的应用程序模块中。在这个应用中，如果不是强制性的，我们也将使用它们。模块本身实际上只是一个正常导出的 vuex 配置对象。所有模块共享相同的命名空间，因此，例如，如果一个模块使用名为“name”的状态变量，另一个模块就不能。为了解决这个问题，模块可以“命名空间”，这实际上意味着所有的名字都有一个命名空间前缀。

## 向应用程序添加模块

首先在存储中创建两个新文件夹，一个名为“postStore”和“userStore”。它们每个都包含一个模块，在每个模块内部创建一个“index.js”文件。在 postStore 中添加以下代码:

```
export default {
  namespaced: true,
  state: {
    posts: [
      {
        id: 1,
        user: "idalmasso",
        date: "2021-01-19 15:30:30",
        post:
          "Today I'm feeling sooooo well...",
        comments: [
          {
            id: 3,
            user: "Nostradamus",
            date: "2021-01-20 20:30:34",
            post: "LOL"
          },
          {
            id: 4,
            user: "FinnishMan",
            date: "2021-01-20 20:30:34",
            post: "Please..."
          }
        ]
      },
      {
        id: 2,
        user: "cshannon",
        date: "2021-01-19 15:25:20",
        post: "Say something here"
      }
    ]
  },

  mutations: {
    ADD_POST(state, post) {
      state.posts.push(post);
    },
    DELETE_POST(state, id) {
      state.posts = state.posts.filter(post => post.id != id);
    }
  },
  actions: {
    async addPost(context, post) {
      context.commit("ADD_POST", post);
    },
    async deletePost(context, id) {
      context.commit("DELETE_POST", id);
    }
  },
  getters: {
    allPosts(state) {
      return state.posts;
    },
    userPosts: state => user => {
      return state.posts.filter(post => post.user === user);
    }
  }
};
```

在这里，我们可以看到我们之前讨论过的所有内容:模块实际上是由标志“namespaced”命名的，有一个包含“posts”列表的状态，有两个操作状态的变化，ADD_POST 和 DELETE_POST，操作包含两个提交相对变化的异步函数，最后有两个 getters 函数来获取所有的帖子和用户的帖子列表。

这段代码几乎是不言自明的，所以只需在 UserStore 文件夹中添加 index.js，如下所示:

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
    }
  }
};
```

这里的状态包含一个用户对象，该对象包含实际登录的用户(到目前为止，它将始终保持不变……我们将更改它),以及用于登录/注销功能的两个变化和动作。目前，它只能在 vuex 商店中工作，所以它并不真正有用，但这几乎只是作为未来功能的占位符。

现在，要使用这两个模块，我们应该只将它们导入并添加到主 index.js 中的 store 文件夹中，在右边的参数中:

```
import postsModule from "./postStore/index.js";
import usersModule from "./userStore/index.js";
...
 modules: { posts: postsModule, users: usersModule }
...
```

注意，我们给 posts 模块命名为“posts”，给 users 模块命名为“users”。因为这两个模块是命名空间的，所以这个名称将是引用它们包含的对象时使用的前缀。

现在，要查看组件中的实际帖子，最后要做的是使用它们:只需进入 Posts.vue 组件，从数据配置参数中删除" *posts* "数组，并创建一个新的计算方法，也命名为" *posts* "(因此不需要更改任何其他内容):

```
computed: {
    posts() {
      return this.$store.getters["posts/allPosts"];
    }
}
```

因此，在这里我们可以看到，我们可以通过 vue 实例中的“$store”关键字来访问商店中的对象。注意 *allPosts* 方法名是如何以模块名为前缀的。

此外，删除帖子列表和 User.vue 组件的数据参数中的用户。然后，添加以下两个计算方法:

```
userPosts() {
  return this.$store.getters["posts/userPosts"](this.user.username); 
},    
user() {
  return this.$store.getters["users/currentUser"];    
}
```

因此，userPosts 获得参数“this.user.username ”,该参数是从第二个计算方法中获得的(记住，计算方法在被调用时没有括号)。

现在，所有这些都设置好了，帖子和用户管理将集中在商店中(同样，在未来，也用于身份验证)。在下一章中，我们将对这个商店做一些改变，并开始利用它的功能。