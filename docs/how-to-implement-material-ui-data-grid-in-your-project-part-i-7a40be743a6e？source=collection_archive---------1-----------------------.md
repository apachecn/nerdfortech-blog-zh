# 如何在您的项目中实现材质 UI 数据网格—第一部分

> 原文：<https://medium.com/nerd-for-tech/how-to-implement-material-ui-data-grid-in-your-project-part-i-7a40be743a6e?source=collection_archive---------1----------------------->

当我第一次遇到 **DataGrid** 的时候，就好像把你一直面临的所有问题都外包出去，有人高效地管理。

本质上，它负责基本的实用功能，如编辑、排序、过滤、更新等，并允许您更多地关注手头的问题。

Material UI 已经有了一个 **Material Table** 组件，可以很好地处理较小的数据，但是 MUI 团队引入的 **DataGrid** 组件是一个破坏性的组件

首先，让我们了解一下它们之间的区别。区别在于 DOM 节点。

MUI 表使用 DOM 中传统的 HTML 元素，而 as MUI **DataGrid** 使用 DOM 中嵌套的< div >元素

在 **DataGrid** 中增加功能的原因也是一样的，因为有了 HTML <表>，您的手被束缚在定制上。

这两个怎么选？这完全取决于目的。如果**显示数据**为主，使用**表**。如果**用户与数据的交互**是主要的，使用**数据网格**。就这么简单！

我们来实施吧。首先，每个表格都有**行**和**列**。因此**数据网格**有两个强制属性**行**和**列**。

```
<**DatGrid**
**rows** = // expects an array of objects
**columns** = // expects an array of objects
/>
```

列将是一个对象数组，每个对象都应该有一个名为“**字段**的强制属性，并且应该与行的属性相匹配(我们将在后面看到)

默认情况下，**字段**属性将是特定列的列标题。我们可以通过给定' **headerName** '和' **Description** '属性来自定义标题。 **headerName'** 将显示在列标题中，属性值应为 **'string'** ， **'description'** 属性将在您放置鼠标时给出列数据的描述。

```
const columns = [
{**field**: // expects a string -->Should match with rows property**headerName**: // expects a string -->Will be column header name**description**: // expects a string-->Description is displayed when     mouse is placed over}
]
```

注意:如果我们像下面这样给出行数据，我们可以直接显示数据(注意:**【id】**属性在行中是必需的)

```
<DataGrid columns = [
{field:'**name**'},
{field:'**age**'},
{field:'**gender**'}
]
rows = [

// Each property should match with column field's value (**name, age and gender**) except '**id**' which is mandatory for rows //
{
**id**:1,
**name**:'Harold',
**age**:'50',
**gender**:'male'
},{
**id**:2,
**name**:'John',
**age**:'53',
**gender**:'male'
},{
**id**:3,
**name**:'Root',
**age**:'30',
**gender**:'Female'
},]
/>
```

默认情况下，列是根据它在数组中的定义顺序显示的，即姓名、年龄、性别