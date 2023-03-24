# React with MaterialUI 中的通用表格

> 原文：<https://medium.com/nerd-for-tech/a-generic-table-in-react-with-materialui-618fff24eb89?source=collection_archive---------11----------------------->

![](img/07d48d0de10ed68212a63c24754c949b.png)

基思·米斯纳在 [Unsplash](https://unsplash.com/s/photos/table?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

> **免责声明**
> 该组件基于 MaterialUI 组件部分的示例构建，因此部分代码可能相同。这只是为那些想节省一些时间，而不试图弄清楚那里做了什么的人准备的。

# 介绍

我们作为 react 开发者肯定听说过 MaterialUI。这是一个基于谷歌材料设计的 react(不确定 react native)的奇妙 UI 库。

现在，为了处理表格形式的可视化数据，它提供了一个**表**组件。但是如果你很着急的话，根据你的需要配置它可能看起来很痛苦。

# 通用表格组件

这里我在**表**组件的顶部创建了一个数据表组件。

DataTable 具有以下特性:

*   如果直接提供行数组，则可以推断列名。
*   可以处理单列级别的排序。
*   支持分页。
*   支持单元格内的自定义元素或组件。
*   最重要的是，我们可以随时根据自己的需求进行定制。

因为这是一个普通的 React 组件，您可以在下面找到源代码，并对其进行改进。

# 使用

**基本用法**

```
<DataTable
    columnData={[
        {
            id: 'name',
            name: 'Name',
            enableSort: true,
            align: "center"
        },
        {
            id: 'desc',
            name: 'Description',
            enableSort: true,
            align: "center"
        }
    ]}
    rows={[
       { name: 'First', desc: 'First Item' },
       { name: 'Second', desc: 'Second Item' }
    ]}
/>
```

DataTable 组件有两个参数， ***列数据*** 和 ***行*** 。

***columnData*** 是列的配置值数组， *id* ， *name* ，enableSort_， *align* 。

***行*** 是对象的数组，也可以说是我们要在表体中显示的数据。

分页与 MaterialUI 组件页面上的示例相同。

> 这在讨论和反馈中有待改进。

源代码如下。