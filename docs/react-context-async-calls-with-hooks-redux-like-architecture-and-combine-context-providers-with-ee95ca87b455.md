# React 上下文、带钩子的异步调用、Redux like 架构以及将上下文提供者与 Typescript 相结合

> 原文：<https://medium.com/nerd-for-tech/react-context-async-calls-with-hooks-redux-like-architecture-and-combine-context-providers-with-ee95ca87b455?source=collection_archive---------2----------------------->

![](img/b2556d80c27377e7d88a0f841d2775e7.png)

图片来自[https://www . robinwieruch . de/static/dbb 6b 6 b 589 aad 81 a3 ed 374590 f4d/2b1a 3/banner . jpg](https://www.robinwieruch.de/static/dbb6b6b256b589aad81a3ed374590f4d/2b1a3/banner.jpg)

我们在这里有一个简单的任务，我们需要用 React 上下文实现状态管理，处理一些异步调用，在坚持 redux 概念的同时这样做，最后将所有东西组合在一个单一存储提供者中。

比方说，我们有一个 web 应用程序，它有一些用户验证、一些网站配置，当然还有错误处理。

**总体架构和原理**

我们希望实现组件没有异步调用，状态通过 reducers 改变，我们合并提供者，这样代码更干净，更容易推理。

***文件夹结构***

```
src---...
      context---
              auth---
                     actions.ts
                     reducer.ts
                     types.ts
                     index.ts
              configuration---
                     actions.ts
                     reducer.ts
                     types.ts
                     index.tserror---
                     actions.ts
                     reducer.ts
                     types.ts
                     index.ts
              index.tsx
      pages---
              Login.tsx
              Configuration.tsx
              ErrorBaner.tsx...
```

**显示错误**

***上下文/错误/类型. ts***

```
*import* { *GeneralErrorModel* } *from* '...';*export const* SET_GENERAL_ERROR = 'SET_GENERAL_ERROR';
*export const* CLEAR_GENERAL_ERROR = 'CLEAR_GENERAL_ERROR';*export interface SetGeneralErrorAction* {
  type: *typeof* SET_GENERAL_ERROR;
  payload: *GeneralErrorModel*;
}*export interface ClearGeneralErrorAction* {
  type: *typeof* CLEAR_GENERAL_ERROR;
}
```

***上下文/错误/动作. ts***

```
*import* { *GeneralErrorModel* } *from* '...';
*import* {
  CLEAR_GENERAL_ERROR,
  *ClearGeneralErrorAction*,
  SET_GENERAL_ERROR,
  *SetGeneralErrorAction* } *from* './types';*export const* setGeneralErrorAction = (
  *error*: *GeneralErrorModel*,
  *dispatch*: (*action*: *SetGeneralErrorAction*) => *void* ): *void* => {
  *dispatch*({
    type: SET_GENERAL_ERROR,
    payload: *error* });
};*export const* clearGeneralErrorAction = (
  *dispatch*: (*action*: *ClearGeneralErrorAction*) => *void* ): *void* => {
  *dispatch*({
    type: CLEAR_GENERAL_ERROR
  });
};
```

***上下文/错误/reducer.ts***

```
*import* { *GeneralErrorModel* } *from* '../../shared/models';
*import* { *ClearGeneralErrorAction*, *SetGeneralErrorAction* } *from* './types';*export const* initialErrorState: *GeneralErrorModel* = {
  message: ''
};*export const* errorReducer = (
  *state*: *GeneralErrorModel* = initialErrorState,
  *action*: *SetGeneralErrorAction* | *ClearGeneralErrorAction* ): *GeneralErrorModel* => {
  *switch* (*action*.type) {
    *case* 'SET_GENERAL_ERROR':
      *return* {
        ...*state*,
        ...action.payload
      };
    *case* 'CLEAR_GENERAL_ERROR':
      *return* initialErrorState;
    *default*:
      *return state*;
  }
};
```

**用户认证**

**context/auth/types . ts**

```
*import* { *UserModel* } *from* '...';*export const* SET_USER = 'SET_USER';
*export const* CLEAR_USER = 'CLEAR_USER';*export interface ClearUserAction* {
  type: *typeof* CLEAR_USER;
}*export interface SetUserAction* {
  type: *typeof* SET_USER;
  payload: *UserModel*;
}
```

***context/auth/actions . ts***

```
*import* { *UserModel* } *from* '...';
*import* {
  CLEAR_USER,
  *ClearUserAction*,
  SET_USER,
  *SetUserAction* } *from* './types';*export const* setUserAction = (
  *user*: *UserModel*,
  *dispatch*: (*action*: *SetUserAction*) => *void* ): *void* => {
  *dispatch*({
    type: SET_USER,
    payload: *user* });
};*export const* clearUserAction = (
  *dispatch*: (*action*: *ClearUserAction*) => *void* ): *void* => {
  *dispatch*({
    type: CLEAR_USER
  });
};
```

**context/auth/reducer . ts**

```
*import* { *UserModel* } *from* '../../shared/models';
*import* {
  SET_USER,
  CLEAR_USER,
  *ClearUserAction*,
  *SetUserAction* } *from* './types';*export const* initialUserState: *UserModel* = {
  email: '',
  id: ''
};*export const* userReducer = (
  *state*: *UserModel* = initialUserState,
  *action*: *SetUserAction* | *ClearUserAction* ): *UserModel* => {
  *switch* (*action*.type) {
    *case* SET_USER:
      *return* {
        ...*state*,
        ...action.payload
      };
    *case* CLEAR_USER:
      *return* initialUserState;
    *default*:
      *return state*;
  }
};
```

现在我们想从组件中提取分派，组件本身不应该知道我们是如何处理动作的，它应该只使用上下文，而不用担心它的内部实现。这很容易做到，而且在我看来，它确实有助于分离我们的应用层之间的关注点。让我们从错误上下文开始。

***上下文/错误/索引. ts***

```
*import* React, {
  createContext,
  useReducer,
  useContext,
  ComponentProps,
  FC
} *from* 'react';
*import* { *GeneralErrorModel* } *from* '...';
*import* { errorReducer, initialErrorState } *from* './reducer';
*import* { clearGeneralErrorAction, setGeneralErrorAction } *from* './actions';*interface ErrorContext* {
  state: *GeneralErrorModel*;
  setError: (*error*: *GeneralErrorModel*) => *void*;
  clearError: () => *void*;
}*const* ErrorContext = createContext<*ErrorContext*>({} *as ErrorContext*);*export const* useErrorContext =
  (): *ErrorContext* => useContext(ErrorContext);*export const* ErrorContextProvider = ({
  *children* }: ComponentProps<FC>): JSX.Element => {*const* [state, dispatch] =
    useReducer(errorReducer, initialErrorState);*const* setError = (*error*: *GeneralErrorModel*): *void* => {
    setGeneralErrorAction(*error*, dispatch);
  };*const* clearError = (): *void* => {
    clearGeneralErrorAction(dispatch);
  };*return* (
    <ErrorContext.Provider *value*={{ state, setError, clearError }}>
      {*children*}
    </ErrorContext.Provider>
  );
};
```

现在我们只需要在 ErrorBanner 组件中使用 ErrorContext。

```
*import* React *from* 'react';
*import* { useErrorContext } *from* '...';*export const* ErrorBanner = (): JSX.Element => {
  *const* { state, clearError } = useErrorContext();*if* (!state.message) {
    *return* <></>;
  }*return* (
    <div *className*={'error-banner'}>
      <span className="error-message">{state.message}</span>
      <span *key*="close" *className*="close" *onClick*={clearError} />
  );
};
```

这很容易，因为逻辑非常简单，只需设置和清除错误，但是如何处理 **auth** 我们有一个 Http 调用和可能的错误，让我们看看如何处理这个问题。

**但在此之前，我想休息一下，谈谈我们的应用程序对外部服务的 HTTP 请求。** 在这一点上，我们已经可以推导出，如果你想把它做对你就用函数来做，甚至更好的高阶函数，但是为什么呢？让我解释一下，使用定制钩子(HOFs)的好处不仅仅是用钩子写代码，因为这在社区里很流行，最重要的一点是，在你写为钩子的函数中，你可以访问上下文，这真的很酷，允许你做比你想象的更多的事情。因此，让我们为 HTTP 调用创建一个通用钩子。

***使用 Http.ts***

```
*import* axios, { AxiosResponse, AxiosError } *from* 'axios';
*import* { useErrorContext } *from* '...';*interface* UseHttp {
  get: <T>(url: *string*) => Promise<AxiosResponse<T>>;
  post: <T>(url: *string*, payload: *any*) => Promise<AxiosResponse<T>>;
}*export const* useHttp = () => {
  *const* { clearError, setError } = useErrorContext();axios.interceptors.request.use(req => {
    clearError();*return req*;
  });axios.interceptors.response.use(
    *res* => {
      *return res*;
    },
    (*error*: AxiosError) => {
      *if* (!*error*.response) {
        *return*;
      }
      setError(*error*.response.data);
    }
  );*const* formatUrl = (*partialUrl*: *string*): *string* => {
    *return* `${process.env.REACT_APP_API_URL}/${*partialUrl*}`;
  };*const* get = *async* <T>(*url*: *string*): *Promise*<AxiosResponse<T>> => {
    *return* axios.get(formatUrl(*url*));
  };*const* post = <T>(
    *url*: *string*,
    *payload*: *any* ):*Promise*<AxiosResponse<T>> => {
    *return* axios.post(formatUrl(*url*), *payload*);
  };*return* { get, post };
};
```

哇！我们可以在每个 HTTP 请求上设置和清除错误，而不必担心内层的错误，这很酷。现在让我们创建这个通用钩子的专门化，这样我们就可以处理具体的问题。

我们有 auth Http 调用，所以我们将创建 useAuthHttp 挂钩。

**useauthhttp . ts**

```
*import* { useHttp } *from* './useHttp';
*import* jwt *from* 'jsonwebtoken';*import* { *UserModel*, *LoginResponseModel* } *from* '...';
*import* { ApiEndPointsEnum } *from* '...';*interface UseAuthHttp* {
  loginUser: (
    *email*: *string*,
    *password*: *string* ) => *Promise*<*UserModel* | *null*>;
}*export const* useAuthHttp = (): *UseAuthHttp* => {
  *const* { post } = useHttp();*const* loginUser = *async* (
    *email*: *string*,
    *password*: *string* ): *Promise*<*UserModel* | *null*> => {
    *const* response = 
      *await* post<*LoginResponseModel>*(ApiEndPointsEnum.*Login*, {
        *email*,
        *password* });*return* response
      ? jwt.verify(
          response.data.token,process.env.REACT_APP_JWT_SECRET
        )
      : *null*;
  };*return* { loginUser };
};
```

正如您在这里看到的，我们不必担心失败的请求，这里我们只担心响应中的 auth token。

我们现在可以继续我们在 auth 上下文上的工作。

***context/auth/index . ts***

```
*import* React, {
  ComponentProps,
  createContext,
  FC,
  useContext,
  useReducer
} *from* 'react';*import* { *UserModel* } *from* '...';
*import* { initialUserState, userReducer } *from* './reducer';
*import* { useAuthHttp } *from* '../../shared/http';
*import* { clearUserAction, setUserAction } *from* './actions';*interface UserContext* {
  state: *UserModel*;
  login: (*email*: *string*, *password*: *string*) => *void*;
  logout: () => *void*;
  isAuthenticated: () => *boolean*;
}*const* UserContext = createContext<*UserContext*>({} *as UserContext*);*export const* useUserContext =
  (): *UserContext* => useContext(UserContext);*export const* UserContextProvider = ({
  *children* }: ComponentProps<FC>): JSX.Element => {*const* [state, dispatch] =
    useReducer(userReducer, initialUserState);*const* { loginUser } = useAuthHttp();*const* isAuthenticated = (): *boolean* => {
    *return* !!state.id;
  };*const* login = *async* (
    *email*: *string*,
    *password*: *string* ): *Promise*<*void*> => {
    *const* user = *await* loginUser(*email*, *password*);
    *if* (user) {
      setUserAction(user, dispatch);
    }
  };*const* logout = (): *void* => {
    clearUserAction(dispatch);
  };*return* (
    <UserContext.Provider
      *value*={{state, login, logout, isAuthenticated }}
    >
      {*children*}
    </UserContext.Provider>
  );
};
```

***log in . tsx***(*我就不重点说验证之类的了，这只是为了故事上下文。如果你对此感兴趣，你可以阅读下面的故事*

 [## React，自定义验证挂钩

### 表单域验证总是需要的，但是你不应该被吸引去使用一些流行的库…

medium.com](/@rasha08/react-custom-validation-hooks-7699416907e6) 

```
*import* React, { ChangeEvent, useEffect, useState } *from* 'react';
*...**import* { useUserContext } *from* '...';...
*export const* Login = (): JSX.Element => {
  *const* [email, setEmail] = useState('');
  *const* [password, setPassword] = useState('');*const* { login } = useUserContext();*const* onEmailChange = ({
    target: { *value* }
  }: ChangeEvent<*HTMLInputElement*>): *void* => {
    setEmail(*value*);
  };*const* onPasswordChange = ({
    target: { *value* }
  }: ChangeEvent<*HTMLInputElement*>): *void* => {
    setPassword(*value*);
  };*const* loginUser = (): *void* => {
    login(email, password);
  };*return* (
    <Container *maxWidth*="md">
      <TextField
        *label*="Email"
        *onChange*={onEmailChange} 
        value={email}/>
      <TextField
        *type*="password"
        *label*="Password"
        *onChange*={onPasswordChange}
        value={password}/><Button *onClick*={loginUser}>Login</Button>
    </Container>
  );
};
```

**配置** 这里还是老样子，我们有类型、动作、reducer、上下文，对 Http 请求使用 ConfigHttp，所以为了后面的故事，没必要再去扔那个了。

**组合上下文提供者** 这是更好的选择，这不是必须的，但我认为如果你有多个上下文提供者，一些组件需要使用，这是必须的。我们只需要看看上下文提供者，只是反应组件。

**组合组件*。tsx***

```
*import* React, { ComponentProps, FC } *from* 'react';*export const* combineComponents = (...*components*: FC<*any*>[]): FC => {
  return components
    .reverse()
    .reduce(
      (*AccumulatedComponents*, *CurrentComponent*) => {
        *return* ({ *children* }: ComponentProps<FC>): JSX.Element => {
          *return* (
            <AccumulatedComponents>
              <CurrentComponent>{*children*}</CurrentComponent>
            </AccumulatedComponents>
          );
        };
      },
      ({ *children* }) => <>{*children*}</>);
};
```

我知道这可能看起来令人困惑，但我们只是在这里堆叠组件作为包装。和我在一起，我保证你很快就会看到好处。

最后，让我们创建一个组合的上下文提供者，并在我们的应用程序 ***index.tsx*** 中提供它，就像我们对 redux 所做的那样。

**store/index . tsx**

```
*import* { ErrorContextProvider } *from* './error';
*import* { AuthContextProvider } *from* './auth';
import { ConfigurationContextProvider } from './configuration';*import* { combineComponents } *from* '../shared/utils';*export const* AppContextProvider = combineComponents(
  ErrorContextProvider,
  AuthContextProvider,
  ConfigurationContextProvider
);
```

这里重要的是要知道提供者的依赖关系，如果有的话，我们对 ErrorContext 上的 useHttp hook 有一个依赖关系，当我们在 Auth 和 Configuration 上下文中使用这个钩子时，我们需要在依赖关系之前提供 ErrorContextProvider。

**src/index . tsx**

```
*import* ReactDOM *from* 'react-dom';
*import* { AppContextProvider } *from* './context';
*import* App *from* './App';
*import* * *as* serviceWorker *from* './serviceWorker';

*import* './styles.scss';

ReactDOM.render(
  <AppContextProvider>
    <App />
  </AppContextProvider>,
  document.getElementById('root')
);
serviceWorker.unregister();
```

那就是我们现在可以在应用程序的任何地方使用我们的上下文。这些是你可以用 redux 架构开发大规模应用程序的原则。