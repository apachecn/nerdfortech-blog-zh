# 带 React 的部分应用

> 原文：<https://medium.com/nerd-for-tech/functional-component-object-state-724691aa7bfb?source=collection_archive---------11----------------------->

![](img/b626ec33270f0da76dcfb9d172e0f1a1.png)

如何在 React 功能组件中实现声明性表单状态管理策略？

思考这个问题一两分钟后，我意识到除了改变表单的底层对象状态之外，如果该功能还能在用户开始输入时清除任何验证错误，那就太好了。

所以我开始考虑这个结果。

```
<input type="text" onChange={**setsProperty('firstName')**} />
```

在确保可重用性的同时，我可以使用什么技术来完成这项工作？功能组成呢，具体来说，[局部应用](https://en.wikipedia.org/wiki/Partial_application)？考虑到这一点，我像这样逐步完成了这个过程。

当 onChange 事件触发时，我需要一个属性名来设置修改状态对象。

```
const formObjectMutator = **prop => event** =>{}
```

除此之外，我还需要访问当前状态，这样我就可以将它与更改后的属性结合起来。

```
const formObjectMutator = **currentState** => prop => event =>{}
```

我将如何更新状态？为此，我也需要一个 useState setter 函数。

```
const formObjectMutator = currentState => **updateStateFn** => prop => event =>{
      **updateStateFn**({...currentState,[prop]: event.target.value});
}
```

好了，这变得很混乱，currentState 和 updateStateFn 显然属于一起，可以移到同一个函数中。

```
const formObjectMutator = (**currentState, updateStateFn**) => prop => event =>{
        updateStateFn({...currentState,[prop]: event.target.value});
}
```

哦，对了，我差点忘了，那些验证错误怎么办？让我们使这一个成为可选的。我们也可以将这个参数传递给初始函数。

```
const formObjectMutator = (currentState, updateStateFn, **clearMessagesFn**) => prop => event =>{ updateStateFn({...currentState,[prop]: event.target.value});

     //clear validation errors
    if(**clearMessagesFn**) **clearMessagesFn();**
}
```

最后，我们如何在 React 功能组件中使用这个几乎不可读的函数？

```
// 1\. Create your state and mutator function
const [profile, setProfile] = useState(null);// 2.Configure a new function that takes the property name
const **setsProperty** = **formObjectMutator**(profile, setProfile, () => validationErrors && setValidationErrors(null));// 3\. Configure new functions that handle onChange events
<input type=”text” onChange={setsProperty('firstName')} />
<input type=”text” onChange={setsProperty('lastName')} />
<input type=”text” onChange={setsProperty('age')} />
```

完事了吗？几乎是，但在此过程中，我意识到“currentState”参数似乎产生正确结果的唯一原因是，formObjectMutator 函数在每个更新周期后都用最近的状态重新初始化。

用 useState 钩子[创建的 Mutator 函数也将接受一个函数来代替值](https://reactjs.org/docs/hooks-reference.html#functional-updates)。因此，如果我们传入一个回调，先前的状态值将作为参数传递给它，我们可以使用该值，结合我们的更改，来重建我们的状态对象。

重构之后，这就是我最终得到的结果。

```
export const formObjectMutator = (updateStateFn, clearMessagesFn) => property => event => { // do not reference the event object within 
    // the inner function passed to the state 
    // function or you will have problems
    **const newValue = event.target.value;** updateStateFn(**prevState => { 
        return { …prevState, [property]: newValue} 
    }**); if(clearValidationMessagesFn)
       clearValidationMessagesFn();
}// Now it only requires the setter function
const setsProperty = formObjectMutator(setProfile, () => validationErrors && setValidationErrors(null));.......<input type=”text” onChange={setsProperty('pronouns')} />
```