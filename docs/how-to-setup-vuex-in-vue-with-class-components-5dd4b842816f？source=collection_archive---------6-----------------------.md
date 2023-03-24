# 如何用类组件在 Vue 中设置 Vuex

> 原文：<https://medium.com/nerd-for-tech/how-to-setup-vuex-in-vue-with-class-components-5dd4b842816f?source=collection_archive---------6----------------------->

![](img/97f5a764617424c53e497588eb9bf9e0.png)

自由由 [**贾森·恰内利**](https://fineartamerica.com/profiles/jaison-cianelli)

如果您的项目是使用带有类组件的 Vue 构建的，从头开始设置 Vuex 可能有点困难，因为 Vuex 文档中没有现成的解决方案。这个指导将帮助你对抗这条龙:下面你可以找到一个带有解释的安装实例。

1.  首先你需要安装 Vuex:

```
$ npm install vuex --save
```

或者，如果你喜欢纱线:

```
$ yarn add vuex
```

2.你还需要一个额外的包: [vuex-class-component](https://www.npmjs.com/package/vuex-class-component) ，它不仅允许你用类组件来使用 vuex，还提供了一个非常简洁的功能，叫做*代理。*代理允许我们避免使用 mapState、mapMutations 和 mapActions，但是只有当您将 strict 模式设置为 *false* 时，才可以使用它们。

```
$ npm install --save vuex-class-component
```

或者，如果你喜欢纱线:

```
$ yarn add vuex-class-component
```

3.由于我们已经安装了所有的包，现在我们可以创建一个新的文件夹，并将其命名为/ ***store*** *。*这是我们放置所有 Vuex 文件的地方。在这个文件夹中，我们创建一个文件，并将其命名为***store . vuex . ts***。这将是 Vuex 配置的主文件。你可以将所有存储变量、突变、动作和 getter 放在这个文件中(适合小项目)，但我建议将所有这些功能分解到模块中，这种方法更具可扩展性。要创建一个模块，在同一个文件夹下创建一个文件，命名为 ***user.vuex.ts*** 。

4.在你的 **main.ts** 文件中包含以下导入:

```
import { store } from ‘./store/store.vuex’;
```

5.现在我们来填写***store . vuex . ts***的内容，其中应该包括所有的 Vuex 设置 ***:***

```
import Vue from 'vue';
import Vuex from 'vuex';
import { createProxy, extractVuexModule } from "vuex-class-component";
import UserStore from './user.vuex'Vue.use(Vuex);export const store = new Vuex.Store({
  modules: {
    ...extractVuexModule( UserStore )
  }
})export const vxm = {
  user: createProxy( store, UserStore ),
}
```

6.正如你在上面的代码片段中看到的，我们添加了一个模块，我们称之为 UserStore。你可以随意称呼你的模块。对于每个新模块，您需要在/store 文件夹中创建一个单独的文件，然后您需要将该文件导入到 *store.vuex.ts* 中。

让我们填写 ***user.vuex.ts*** 的内容来创建出第一个模块:

```
import { createModule, mutation, action, getter, Module, createProxy } from "vuex-class-component";const VuexModule = createModule({
  namespaced: "user",
  strict: false,
})export class UserStore extends VuexModule {
  public firstName = "Jan";
  public lastName = "Sverak";
  specialty = "JavaScript";[@mutation](http://twitter.com/mutation) clearName() {
    this.firstName = "";
    this.lastName = "";
  }[@action](http://twitter.com/action) async doSomethingAsync() { return 20 }// Explicitly define a vuex getter using class getters.
  get fullname() {
    return this.firstName + " " + this.lastName;
  }// Define a mutation for the vuex getter.
  // NOTE: this only works for getters.
  set fullName( name :string ) {
    const names = name.split( " " );
    this.firstName = names[ 0 ];
    this.lastName = names[ 1 ];
  }get bio() {
    return `Name: ${this.fullName} Specialty: ${this.specialty}`;
  }
}export default UserStore;
```

7.最后一步:让我们试着在组件中使用它:

```
import {vmx} from '@/client/store/store.vuex;...private mounted() {
// use this to see all available properties of user variable:
    console.log(vmx.user)
}
```

你可以在 Vuex 类组件自述文件中了解更多关于代理的信息。使用代理有它的缺点——它允许直接从组件改变存储，这是不安全的(这就是为什么你需要禁用严格模式来使用它们),但同时它也很方便，因为它节省时间:你不必每次想改变存储时都创建和导入变化。