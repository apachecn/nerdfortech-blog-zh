# 绅士国家经理，你需要的角度应用的救星

> 原文：<https://medium.com/nerd-for-tech/gentleman-state-manager-the-savior-you-need-for-angular-applications-e6c462a76794?source=collection_archive---------7----------------------->

![](img/978ff7d482b6568d89bfa974f4a1d141.png)

嗨大家好！我经常发现自己为我的初级同事创建定制的状态管理问题解决方案，因为让我们面对现实吧…反应式编程和 ngrx/store 很难理解，更难掌握。

我**爱** Rxjs！通过可观察对象定制数据流并组合它们的结果是我在开发中的罪恶乐趣之一，但是当不得不在兄弟组件或不同模块之间共享信息时，并不是每个人都有同样的感觉。您可能经常使用服务、输入和输出，甚至创建 Rxjs 的 observables 来尝试解决这个问题，但是随着应用程序的增长，ngrx/store 成为一个不错的选择。如果你和你的队友有足够的挑战经验。

那么…如果你像我一样有一个需要共享信息的应用程序，并且可以受益于 ngrx/store，但是你或你的队友没有足够的经验来使用它，会发生什么？

# 首先…让我们学习一些东西！

**反应式编程**是开发代码的概念，其中不同的实体通过信息通道观察数据的传递，当它传递时，每个实体将**对这种情况做出反应，执行它们的业务逻辑。**

**想象一个有三个洞的空管，在每个洞我们都有一个人通过它看，并等待一个小球通过。因为每个人都不一样，所以会看到球的不同方面。例如，一个人可以看到球是红色的，另一个人可以看到球在通过水槽时有一点反弹…甚至可能它根本就不是一个球！。**

**这就是反应式编程！我们有一个信息通道(observables ),包含通过它传递的数据，另一方面，我们有一些实体(例如角度组件)将被订阅并等待信息传递……当信息传递时，它们将对它做出反应，执行我们为它们编写的任何业务逻辑。**

**Redux、Ngrx/store 等。是使用**“真实的单一来源”**的概念提供管理应用程序状态的工具的库，这意味着应用程序的全部信息将包含在**“存储”**中，在这里您可以访问有用的和最新的信息。**

**每次应用程序内部发生变化时，存储库都会更新最新的变化。因此，如果不需要的话，您不需要再次调用后端进行更新，并且所有正在监视商店状态的实体都会自动收到有关更改的警报。**

**Rxjs 是一个包含许多管理这些信息渠道的惊人工具的库，这些工具提供了创建、组合和修改可观察对象的能力，包括新的可使用类型，每个都有自己的功能。**

**在这种情况下，我们将大量使用**行为主题**，它是一个可观察对象，将总是包含通过它的最后数据…这就是 ***惊人的*** 为我们想要做的事情！当我们通过行为主体发送信息时，即使一个实体在信息发送后订阅，它也会收到最新的数据！所以我们不会丢失任何东西。**

# **那么……什么是绅士国家经理？**

**绅士状态管理器的主要概念是提供一种有用的方法来管理你的应用程序的状态，使用像 Rxjs 这样的可理解的和清晰的工具。简而言之，我们创建了一个由键标识的观察值数组，每个模块都可以增加数组的大小(所以，是的，我们使用了延迟加载！)来管理通过 Angular 应用程序的不同实体传递的信息。您只需要在 app 模块中创建初始对象，然后如果您有一个惰性加载的模块，您可以通过添加更多的项来增加数组！。**

**但是不要担心，该库的主要思想是易于使用，而且更重要的是…透明，您可以在任何地方访问数组数据，以通过一个超级简单和有用的 API 来获取或发送存储在里面的信息，接下来将详细介绍该 API。**

# **如何使用**

**1-使用库提供的**sourceoftrustinitiate**类创建一个对象。我建议每个模块有一个 **state.ts** ，代表状态接口和属性:**

```
state.ts for Root ( AppModule ):

export interface FirstState {
   stateProperty: string | null;
}

export enum FirstStateProperties {
   STATEPROPERTY: 'stateProperty'
}

export enum StoreKeys {
   FIRSTSTATE: 'firstState'
}

AppModule:

const sourceOfTruthInitiate: SourceOfTruthInitiate[] = [
    {
        key: StoreKeys.FIRSTSTATE,
        state: {
            stateProperty: 'your state property value'
        },
        stateProperties: FirstStateProperties
    }
];
```

*   ****键:**代表访问我们想要使用的适当可观察值的键。**
*   ****状态:**物体将穿过可观测物的空态。**
*   ****stateProperties:** 对象的属性，我们要用它来访问没有歧义的信息。**

**2-通过以下方式将**GentlemanStateManagerModule**导入到您的应用程序中:**

```
@NgModule({
    imports: [
        ...
        GentlemanStateManagerModule.forRoot(sourceOfTruthInitiate)
        ...
    ]
})
export class AppModule {}
```

**3-使用我们的 **API** 访问信息或发送新内容！**

**示例:**

```
export class OverviewMetricComponent implements OnDestroy {
     constructor(gentlemanStateManager: GentlemanStateService) {
        console.log(gentlemanStateManager.getEntity(StoreKeys.FIRSTSTATE).getPropertyFromState(FirstStateProperties.STATEPROPERTY));
    }
}
```

**4-要延迟加载更多的可观察对象，只需执行与 app 模块中相同的功能，但是是在延迟加载模块的构造函数中。**

**示例:**

```
state.ts for Root ( AppModule ):

...
export enum StoreKeys {
   FIRSTSTATE: 'firstState',
   LAZYLOADEDSTATE: 'lazyLoadedState' // we add the new state key
}
...

state.ts of your lazy loaded module:

export interface LazyLoadedState {
   lazyLoadedStateProperty: string | null;
}

export enum LazyLoadedStateProperties {
   LAZYLOADEDSTATEPROPERTY: 'lazyLoadedStateProperty'
}

const sourceOfTruthInitiate: SourceOfTruthInitiate[] = [
    {
        key: StoreKeys.LAZYLOADEDSTATE,
        state: {
            lazyLoadedStateProperty: 'Your Lazy Loaded State Property'
        },
        stateProperties: LazyLoadedStateProperties
    }
];

@NgModule({
   ...
})
export class LazyLoadedModule {
   constructor(gentlemanStateService: GentlemanStateService) {
    sourceOfTruthInitiate.forEach(state => gentlemanStateService.createObservable(state.key, state.state, state.stateProperties));
  }
}
```

# **应用程序接口**

# **可观测量管理阵列:**

**创建可观察对象**

```
/**
* @desc it creates and observable and adds it to the observable array.
* @param key: the key to be used to represent the observable item inside the array
* @param state: the state of the observable, the object that represents what the observable is going to contain
* @return void
*/
  createObservable(key: string, state: any, stateProperties: StateProperties): void
```

**getEntity**

```
/**
* @desc it returns the selected observable using the provided key.
* @param key - the key to be used to represent the observable item inside the array
* @return GentlemanStateObject
*/
  getEntity(key: string): GentlemanStateObject
```

**可观察的毁灭**

```
/**
* @desc it destroys an object from the observable array.
* @param key - the key to be used to represent the observable item inside the array
* @return void
*/
  destroyObservable(key: string): void
```

# **绅士陈述对象:**

**getObservable**

```
/**
* @desc returns the observable that contains the state for async operations - it listens for changes
* @return Observable<any>
*/
  getObservable(): Observable<any>
```

**getStateProperties**

```
/**
* @desc returns the state properties object
* @return StateProperties
*/
  getStateProperties(): StateProperties
```

**取消订阅(当您正在销毁模块的主要组件时使用)**

```
/**
* @desc unsubscribes from the observable
* @return void
*/
  unsubscribe(): void
```

**getStateSnapshot**

```
/**
* @desc returns the value of the state at the time of the call
* @return any
*/
  getStateSnapshot(): any
```

**getPropertyFromState**

```
/**
* @desc returns the value of a property of the state at the time of the call
* @param property - the name of the requested property
* @return any
*/
  getPropertyFromState(property: string): any
```

**getpropertyfromsobservable**

```
/**
* @desc returns the value of a property of the state for async operations - it listens for changes
* @param property - the name of the requested property
* @return Observable
*/
  getPropertyFromObservable(property: string): Observable<any>
```

**setObservableValues**

```
/**
* @desc sets the value for a certain property inside the state, triggers an async event
* @param value - the value for the requested property
* @param property - the name of the requested property
* @param emit - if true it will trigger an async event
* @return void
*/
  setObservableValues(value: any, property: string | null = null, emit = true): void
```

**setStateValues**

```
/**
* @desc sets the value for a certain property inside the state, doesn't triggers an async event
* @param value - the value for the requested property
* @param property - the name of the requested property, if no property it will try to patch the values into the state
* @return void
*/
  setStateValues(value: any, property: string | null): void
```

**重置状态**

```
/**
* @desc resets the state
* @return void
*/
  resetState(): void
```

> **感谢大家的阅读！我很乐意尽我所能帮助社区！**
> 
> **我邀请大家下载绅士状态管理器，并在 Github 上给它一颗星！它是开源的，你可以对它做任何你想做的事情，如果你想用它来测试你自己的东西，这将是一种荣誉:)**
> 
> **npmjs.com NPM:[绅士-国家-管理者-NPM](https://www.npmjs.com/package/gentleman-state-manager)**
> 
> **Github: [阿普尔兰/绅士国家经理](https://github.com/AppleLAN/gentleman-state-manager#readme)**
> 
> **我也想邀请你来我的前端社区，因为[不和](https://discord.gg/KEavKkDc5Y)！在那里，我通过我的 YouTube 频道进行指导和授课。**
> 
> **大家编码快乐！！**