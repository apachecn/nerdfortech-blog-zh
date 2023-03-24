# 在 React with Typescript 中使用上下文

> 原文：<https://medium.com/nerd-for-tech/using-context-in-react-with-typescript-55c1383f9c9d?source=collection_archive---------1----------------------->

所以你要用上下文([https://reactjs.org/docs/context.html](https://reactjs.org/docs/context.html))🙌
➡️所以你希望你的上下文提供状态和一个更新状态的函数。
➡️所以你要用 Typescript。

试试这个:

```
import { createContext } from "react";const Context = createContext({
  state: [] as string[],
  dispatch: (state: string[]) => {},
});
```

🤫*假设* `*state*` *是一个字符串数组。*

也许这是显而易见的，但我花了一段时间来键入传递给`createContext`的`defaultValue`。

《T3》的演员阵容很有趣。如果你不强制转换，当你将`state`传递给 context 的`value`道具时，Typescript 会抱怨(见下文)。它会说`state`的类型是`never[]`(毕竟是空数组)并且和状态变量冲突，状态变量是`string[]`。

下面是钩子以及如何将它传递给上下文。

```
const [state, dispatch] = useState(["initial"]);return (
  <div>
    <Context.Provider value={{ state, dispatch }}>
      <ComponentTreeThatNeedsAccessToState />
      <ComponentTreeThatNeedsAccessToDispatch />
    </Context.Provider>
  </div>
);
```

因此，如果您的`createContext`调用中有一个空数组，并且出现以下错误:

```
Type 'string[]' is not assignable to type 'never[]'
```

那么我希望这有所帮助🙌

*参见*[*https://bit bucket . org/gabrielmccallin/react-context/src/main/src/app . tsx*](https://bitbucket.org/gabrielmccallin/react-context/src/main/src/App.tsx)