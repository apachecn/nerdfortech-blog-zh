# 带有 Vue.js 和 GO 的社交应用

> 原文：<https://medium.com/nerd-for-tech/social-application-with-vue-js-and-go-4d0caa37ddac?source=collection_archive---------11----------------------->

![](img/4c40d84dbd8cd0cbadf21627e76840d9.png)

标志归功于 vuejs.org 和 golang.org

## 用 vue.js 和 golang 创建和服务一个类似 twitter 的应用程序第 9 部分:用 indexedDB 存储认证令牌

这是本系列的第九部分。在这里检查所有零件:

*   [第一部分:设置](https://ivano-dalmasso.medium.com/social-application-with-vue-js-and-go-4e4db0cdde64)
*   [第二部分:VUE 入门](https://ivano-dalmasso.medium.com/social-application-with-vue-js-and-go-64b3adee8dac)
*   [第三部分:组件&插槽](https://ivano-dalmasso.medium.com/social-application-with-vue-js-and-go-24a1d1e7137d)
*   [第 4 部分:Vuex 首次设置](/nerd-for-tech/social-application-with-vue-js-and-go-3a11d506fc38)
*   [第 5 部分:Vuex 终结](/nerd-for-tech/social-application-with-vue-js-and-go-ef364b572422)
*   [第六部分:Vuex 中的表格和数据](https://ivano-dalmasso.medium.com/social-application-with-vue-js-and-go-a22a1afb76eb)
*   [第七部分:与 golang 服务器的连接](/nerd-for-tech/social-application-with-vue-js-and-go-d9e563466b66)
*   [第 8 部分:基于令牌的认证](/nerd-for-tech/social-application-with-vue-js-and-go-64978f7c381f)
*   第 9 部分:使用 indexedDB 存储令牌(this)

在本课中，我们将更改客户端层的身份验证设置，以便在关闭浏览器时不会丢失访问登录和注销。这将是一个有点短的，你可以找到代码，这将只升级前端，[这里](https://github.com/idalmasso/go-vue-tutorial-frontend/releases/tag/v0.9)。

## 将身份验证数据保存到客户端

在上一课之后，应用程序要求我们使用用户名和密码进行身份验证，然后给了我们一个令牌，我们可以用它来调用我们需要的各种 API。

现在我们有一个问题:我们的客户端没有在任何地方存储这个令牌，如果不在存储中的话，每次刷新浏览器时这个令牌都会被清除。

为了解决这个问题，我们将认证数据存储在 indexedDB 中，这是一个特殊的存储，所有现代浏览器都可以使用它来保存一些数据(即从 API 返回值)。

现在，请避免考虑使用本地存储来保存这些信息，这一点也不安全，阅读[这个](https://dev.to/rdegges/please-stop-using-local-storage-1i04)来更好地理解为什么。

实际上，有几个工具可以让这项任务变得简单一点:

*   vuex-persist 是一个包，让你告诉你的商店使用一个个性化的持久层，一旦设置好，一切都会保持在那里
*   idb 是一个软件包，它将使使用 indexedDB 变得更容易，并提供返回承诺的函数，这是 indexedDB 本身没有的特性

实际上我不会在本教程中使用这两个包，我们将使用纯 indexedDB 代码。

## 创建一个实用程序文件来访问 indexedDB

首先创建一个包含一些实用函数的文件，用于我们的用例。在这里，我们将提供创建数据库的功能，并使用它来存储和检索当前登录用户的数据。

使用以下代码创建文件/store/indexedDBUtils.js:

```
const DB_NAME = "authDB";
const DB_VERSION = 1;
let DB;export default {
  async getDb() {
    return new Promise((resolve, reject) => {
      if (DB) {
        return resolve(DB);
      }
      let request = window.indexedDB.open(DB_NAME, DB_VERSION);request.onerror = e => {
        console.log("Error opening db", e);
        reject("Error");
      };request.onsuccess = e => {
        DB = e.target.result;
        resolve(DB);
      };request.onupgradeneeded = e => {
        console.log("onupgradeneeded");
        let db = e.target.result;
        let objectStore = db.createObjectStore("userAuth", {
          autoIncrement: true,
          keyPath: "username"
        });
      };
    });
  },
  async addUser(user) {
    //I ONLY want to have 1 user in this db... I delete all other before
    let db = await this.getDb();return new Promise(resolve => {
      let trans = db.transaction(["userAuth"], "readwrite");
      trans.oncomplete = () => {
        resolve();
      };let store = trans.objectStore("userAuth");
      store.openCursor().onsuccess = e => {
        let cursor = e.target.result;
        if (cursor) {
          if (cursor.value.username != user.username) {
            store.delete(cursor.value.username);
          }
          cursor.continue();
        }
      };
      store.put(user);
    });
  },
  async removeUser(user) {
    if (!user || !user.username) {
      return;
    }
    let db = await this.getDb();return new Promise(resolve => {
      let trans = db.transaction(["userAuth"], "readwrite");
      trans.oncomplete = () => {
        resolve();
      };let store = trans.objectStore("userAuth");
      store.delete(user.username);
    });
  },
  async getUser() {
    let db = await this.getDb();return new Promise(resolve => {
      let trans = db.transaction(["userAuth"], "readwrite");
      trans.oncomplete = () => {
        resolve(user);
      };
      let user = null;
      let store = trans.objectStore("userAuth");
      store.openCursor().onsuccess = e => {
        let cursor = e.target.resul
        if (!cursor) return;
        console.log(cursor.value);
        user = cursor.value;
      };
    });
  }
};
```

第一行是不言自明的，我们在 indexedDB 中定义我们将保存数据的商店的名称。然后，我们导出函数，这些函数以类似承诺的方式提供对我们的 4 个功能的访问:获取数据库实例、创建用户、删除用户和检索用户。

getDb 函数返回一个打开数据库的承诺，然后，如果需要(通过事件“onupgradeneeded”)，它会添加 userAuth 存储来更新数据库，这实际上就是您可以在关系数据库中称为“表”的东西。注意，userAuth 还获得要使用的对象的键的名称作为“keyPath”。在这种情况下，我们将使用用户名作为键来插入用户数据。这个调用设置了我们之前创建的 DB 变量，因此它可以在每个其他函数中用作第一个调用。

AddUser 是第一个我们可以看到 indexedDB 如何工作的函数。它首先获取数据库实例，然后在数据库上创建一个“事务”。该交易的结果将决定承诺的返回状态。首先，我们获得之前创建的存储的 objectStore 实例(“userAuth”)，然后我们删除其中的所有记录(这是因为我们只想在这个数据库中保存实际的用户，记住我们实际上是在用户浏览器中)。最后，我们使用 store.put()调用存储实际的用户数据。

RemoveUser 其实和 addUser 一样，只是把用户从数据库中删除，而 getUser 其实是创建一个游标，返回在数据库中找到的第一条记录(记住，应该只有一条！).

实际上，这就是我们实现这一功能所需的全部内容，现在我们可以在商店中再次更新并使用这一功能。

## 使用商店中的实用功能

在 auth store 的 index.js 文件中，更新登录操作，以便如果登录成功，我们可以将用户添加到 indexedDB，如果出现错误，用户将被删除:

```
async login(context, { username, password }) {
....
.then(data => {
  dbUtils.addUser({ username: username, token: data.token });
  context.commit("LOGIN", 
    {
      username: username, token: data.token
    }
  );
})
.catch(error => {
  dbUtils.removeUser({ username: username });
  context.commit("LOGOUT");
  throw error;
});
```

同样的更新可以在注册动作中完成，而注销动作应该只获得 removeUser 部分。现在，在用户登录或注册后,“用户”对象会自动放入商店。

现在创建一个新操作，以便在需要时检索用户:

```
async loadUser(context) {
  dbUtils.getUser().then(user => {
    console.log(user);
    if (user && user != {})
      context.commit("LOGIN", user);
  });
}
```

这实际上只是检查数据库中是否有用户。如果答案是肯定的，那么存储实际上向该用户提交了登录变异。请记住，令牌实际上是在用户对象内部，所以现在的一切都将是持久的。

我们现在必须在需要时调度这个操作。最简单、最直接的方法是应用程序本身的挂载方法，所以只需进入。/App.vue 文件，并向我们的应用程序的配置对象添加一个 mounted()函数，如下所示:

```
mounted() {
    this.$store.dispatch("auth/loadUser");
}
```

这就是我们所要做的。

现在您可以尝试启动应用程序，它将检查数据库中是否有用户。如果找到它，它将被存储在存储中，它的令牌将被发送到我们的服务器，就像以前一样。

在下一课中，我们将添加 mongoDB 来存储服务器端的数据，这样应用程序最终将能够被称为“完整的”。