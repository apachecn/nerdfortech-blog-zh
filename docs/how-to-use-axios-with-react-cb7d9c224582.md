# 如何配合 React 使用 Axios

> 原文：<https://medium.com/nerd-for-tech/how-to-use-axios-with-react-cb7d9c224582?source=collection_archive---------2----------------------->

以下是我如何使用 Axios 和 react(你需要知道的一切)的经验

![](img/b530ca2b0e24961072533ed0eee7b826.png)

React 中 API 调用的 Axios

> 过去 3 年一直在研究 react。这是一个巨大的旅程，为解决世界问题提供了许多信心。和主要思想过程，为开始学习 react 的开发人员(初级或中级)写下这段文字，以帮助他们如何使用 Axios 在 react 中使用 API 调用。

在深入研究 Axios 之前，您需要具备 javascript 和 XMLHttpRequest 的基础知识。XMLHttpRequest 对象可用于从 web 服务器请求数据。XMLHttpRequest 对象是开发人员的梦想，因为您可以:

*   更新网页而不重新加载页面
*   从服务器请求数据—在页面加载后
*   在后台向服务器发送数据
*   从服务器接收数据—在页面加载后

了解更多关于 [XMLHttpRequest](https://javascript.info/xmlhttprequest) 的信息。

在本文中，我将解释如何在 React 应用程序中使用 Axios 进行 GET、POST、PUT 和其他 HTTP 请求

# Axios 是什么？

Axios 是一个受欢迎的、基于 promise 的 HTTP 客户端，它拥有一个易于使用的 API，可以在浏览器和 Node.js 中使用。我们可以使用 Axios 和 React 向 API 发出请求，从 API 返回数据，然后在 React 应用程序中处理这些数据。

有了 Axios，开发人员还可以利用 async/await 获得更可读的异步代码。

## 安装 Axios

首先，我们需要将 Axios 安装到 react 项目中。使用以下命令安装。

```
// Install with npm
npm install axios**or**// Install with yarn
yarn add axios**or**// Install with bower
bower install axios
```

一旦您在 package.json 中安装了 Axios，让我们开始编写一些代码片段。要打电话给 Axios，我们需要知道如何跟进-

1.  请求的 URL。
2.  方法— (GET、POST 或 PUT 等。,)
3.  头球

这是你的应用程序中 Axios 调用的文件夹结构

```
src/components-
    -- Datalayer
         -- axiosMethodCalls.js
         -- datalayerUtilities.js
    -- Configuration
         -- config_url.js
```

将所有端点保存在一个特定的文件中——在这个场景中是 **config_url.js** 。因此，我们可以在应用程序的一个文件中跟踪所有端点。
**例如— config_url.js**

```
// All Endpoints.
export const jokeCategoriesUrl = `[https://api.chucknorris.io/jokes/categories](https://api.chucknorris.io/jokes/categories)`;export const mockablePostUrl = `[http://demo0725191.mockable.io/post_data](https://www.google.com/url?q=http://demo0725191.mockable.io/post_data&sa=D&source=hangouts&ust=1618854235003000&usg=AFQjCNGnjVmyaEF9avGv91AlqoNrVntG0g)`;export const mockablePutUrl = `[http://demo0725191.mockable.io/put_data](https://www.google.com/url?q=http://demo0725191.mockable.io/post_data&sa=D&source=hangouts&ust=1618854235003000&usg=AFQjCNGnjVmyaEF9avGv91AlqoNrVntG0g)`;export const mockableDeleteUrl = `[http://demo0725191.mockable.io/delete_data](https://www.google.com/url?q=http://demo0725191.mockable.io/post_data&sa=D&source=hangouts&ust=1618854235003000&usg=AFQjCNGnjVmyaEF9avGv91AlqoNrVntG0g)`;
```

将所有 Axios 方法调用保存在特定文件中——在这个场景中**axiomethodcalls . js**,我们可以在应用程序的一个文件中跟踪所有 Axios 方法调用。
**比如—axiomethodcalls . js** 我为 Axios 方法调用创建了一些通用函数，比如 GET、POST、PUT、DELETE。

```
// API Axios Get Call.
export const getAPICall = (url) => {
    return axios.get(url);
}// API Axios Post Call.
export const postAPICall = (url, data) => {
    return axios.post(url, data);
}// API Axios Put Call.
export const putAPICall = (url, data) => {
    return axios.put(url, data);
}// API Axios Delete Call.
export const deleteAPICall = (url) => {
    return axios.delete(url);
}
```

将所有 Axios 调用保存在特定文件中——在这个场景中 **datalayerUtilities.js** 我们可以在应用程序的一个文件中跟踪所有 API 调用。
**例如— datalayerUtilities.js**

```
// Import the endpoints.
import { jokeCategoriesUrl, mockablePostUrl, mockablePutUrl, mockableDeleteUrl } from  './configuration/config_url';// Import the axios Method.
import { getAPICall, postAPICall, putAPICall, deleteAPICall } from './axiosMethodCalls';
```

## 获取请求-

```
// Axios Get Call - Get all jokes categories.
export const getJokeCategorieData = async () => {
    try {
        const response = await getAPICall(jokeCategoriesUrl);
        return response.data;
    }
    catch(error){
        return error.response;
    }
}
```

## 发布请求-

```
// Axios Post Call - MockablePost.
export const mockablePostData = () =>{
     try {
        const response = await postAPICall('mockablePostUrl', {
              posted_data: 'example' });
        console.log('👉 Returned data:', response);   
     } 
     catch (error) {
        console.log(`😱 Axios request failed: ${error}`);
     }
}
```

## 上传请求-

```
// Axios Put Call - MockablePut.
export const mockablePutData = () =>{
     try {
        const response = await putAPICall('mockablePostUrl', {
              posted_data: 'example' });
        console.log('👉 Returned data:', response.data);   
     } 
     catch (error) {
        console.log(`😱 Axios request failed: ${error}`);
     }
}
```

## 删除请求-

```
// Axios Delete Call - MockableDelete.
export const mockableDeleteData = () =>{
     try {
        const response = await deleteAPICall('mockablePostUrl');
        console.log('👉 Returned data:', response.data);   
     } 
     catch (error) {
        console.log(`😱 Axios request failed: ${error}`);
     }
}
```

## 反应成分—

```
import React from 'react';
import { getJokeCategorieData, mockablePostData, mockablePutData, mockableDeleteData } from '../Datalayer/datalayerUtilities';class App extends Component{
    constructor(props){
        super(props);
        this.state = {
             ListOfJokeCatgories: "",
             isLoading: true,
        }
    }
    handlerListOfJokesCategories = async() => {
         const responseData = await getJokeCategorieData();
         this.setState({ 
             ListOfJokeCatgories: responseData.data,
             isLoading: false
         });
    }componentDidMount(){
        this.handlerListOfJokesCategories();
    }

    render(){
       return (
          <div className="container">
              { this.state.isLoading ?
                   <div>Loading.....</div>
                :
                   // Render your api call logic here...
              }
          </div>
       )
    }
}
export default APP;
```

# 结论

用 Axios 处理请求非常容易理解。还有改进的空间，比如增加更多的抽象和结构，但那是额外的时间。

使用 React 和 Axios 会相信什么？是否有人认为我遗漏了或应该涵盖某些特征？请在下面的评论中告诉我！