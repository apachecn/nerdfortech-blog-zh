# 如何在 ReactJs 应用程序中添加复制到剪贴板功能。

> 原文：<https://medium.com/nerd-for-tech/how-to-add-copy-to-clipboard-functionality-in-a-reactjs-app-45404413fdb2?source=collection_archive---------1----------------------->

![](img/ff0405cbcd79c5a527f689b26fb8e70b.png)

[金立](https://unsplash.com/@kinli?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

在这篇文章中，我将分享如何在 react 应用中添加**复制到剪贴板**功能，而无需在应用中安装库。😌

所以我们需要三样东西:♻️

01: **一个按钮**:你可以选择 div 或者复制图标，任何你喜欢的东西。

02:一个 **onClick** 处理函数:我在这里使用了一个匿名的箭头函数，但是我们可以为它创建一个单独的函数。

03: **变量** / **状态变量**(取决于您的数据类型) :存储将要复制的文本！

```
import "./styles.css";export default function App() {// variable to store the data to be copied.
const **text** = "Lorem ipsum dolor sit amet, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.";return (
     <div className="App">
       <h1>Copy to Clipboard!</h1>
       <h4>By Tanya</h4>
       <p>{text}</p> <**button** onClick={() => {
         navigator.clipboard.writeText(text);}}>
        **Copy** </**button**>
     </div>);
}
```

因此，在这里你可以看到，我是如何在上面的代码中使用这三样东西来实现复制功能的。

为了更好地理解，下面是完整的代码/演示:

*如果你喜欢这篇文章，请点击掌声图标并关注更多！🐾*