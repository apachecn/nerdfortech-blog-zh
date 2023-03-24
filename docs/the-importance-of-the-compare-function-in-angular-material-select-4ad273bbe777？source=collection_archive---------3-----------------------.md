# 比较函数在角材选择中的重要性

> 原文：<https://medium.com/nerd-for-tech/the-importance-of-the-compare-function-in-angular-material-select-4ad273bbe777?source=collection_archive---------3----------------------->

## 选择不加载我以前选择的选项。为什么？

![](img/f01eac96d4489376c7da2bb469a9882f.png)

照片由[HalGatewood.com](https://unsplash.com/@halacious?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

在本文中，我们可以了解比较函数`compareWith`的重要性，这个属性与表单控件`<mat-select>`一起使用，用于比较选项值和选定值。

在选择材料时，我们经常会忘记一些重要的元素或属性，没有这些元素或属性，选择将无法按预期进行。在我的演示中，我将使用一个包含两个组件的简单角度项目。在第一页上，用户可以选择一种颜色，然后导航到下一页并返回以查看选择是否加载了之前选择的选项。

# 答。先决条件

## **导入** API 引用

角度材料选择的 API 参考需要导入角度应用模块。

```
import {MatSelectModule} from '@angular/material/select';
```

## 具有材料形式字段

> “它被设计成在一个`[<mat-form-field>](https://material.angular.io/components/form-field/overview)`元素内工作”，*根据* [material.angular](https://material.angular.io/components/select/api)

## 拥有 HTML 视图和模块元素

我们需要一个带有`<mat-select>`的表单字段，其中包含由`<mat-options>`元素持有的选项。

main-page-component.html

在我们的 angular 模块中，我们需要一个表单域元素来存储我们的数据，在我们的例子中是从颜色列表中选择的颜色。

主页组件. ts

## 存储我们选择的数据

在我们的示例中，我们将数据存储在浏览器的本地存储中。在其他情况下，这可能由从后端服务器获取数据并将其加载到`FormControl`的服务来执行。

主页组件. ts

导航到下一页时，所选颜色将保存在单击事件中。

除此之外，我们有两个模块，主要模块，我们可以选择一种颜色，另一个用于演示目的。

# [B]。选择不加载我以前选择的选项！为什么以及如何解决

当导航到下一页，然后返回到主页时，我们可以看到数据被正确地存储并加载到我们的表单字段中，但是我们注意到 select 字段没有预加载我们的选项值。

这是因为我们在 HTML 视图中没有使用`compareWith`功能，因此物料选择不知道如何从其他选择选项中比较和选择我们的项目。

main-page-component.html

主页组件. ts

我们可以有多种形式的比较。在上面的例子中，我们指定了我们想要比较两个对象的确切模型。在返回一个`boolean`值的限制下，`compareWith`函数可以根据我们的需要定制。

# D。工作示例源代码

下面你可以在我做的一个简单的例子中看到这一点，如果我们不写一个`compareWith` 函数，选中的颜色将在从下一页返回后消失，即使它被加载到`FormControl`中。

## 请在此查看源代码:

> [https://github.com/bnam0154/AngularMaterialSelect](https://github.com/bnam0154/AngularMaterialSelect)

# **【E】。**资源。**去哪里找更多！**

属性的详细描述可以在官方的 Angular 文档中找到，其中有更有用的指令和示例。

[](https://material.angular.io/components/select/api) [## 角状材料

### 用于移动和桌面 Angular web 应用程序的 UI 组件基础结构和材料设计组件。

材料.角度. io](https://material.angular.io/components/select/api)