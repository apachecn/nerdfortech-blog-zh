# 关于 Android 绑定您应该知道的一切，第 2 部分

> 原文：<https://medium.com/nerd-for-tech/everything-you-should-know-about-binding-in-android-part-2-8fd0c7fd3dfd?source=collection_archive---------0----------------------->

![](img/f7859913e4cd3c9a3974273750ed6806.png)

照片由[埃米尔·佩伦](https://unsplash.com/@emilep?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/binding-code?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄

在第一部分中，我们了解了什么是视图绑定，什么是数据绑定。两者有什么不同。和绑定的基本功能。如果您尚未查看，请点击此处查看:

[](/@karishma.agr1996/everything-you-should-know-about-binding-in-android-a8a70bc882f0) [## 关于 Android 绑定你应该知道的一切

### 你好朋友们。我希望你喜欢我的写作，如果你有任何建议让我知道。所以这周，我们要去…

medium.com](/@karishma.agr1996/everything-you-should-know-about-binding-in-android-a8a70bc882f0) 

在本文中，我们将讨论如何为嵌套布局应用绑定。如何在视图上显示之前使用 BindingAdapter 自定义值，然后使用双向数据绑定。

# **包括**

Include 用于通过绑定向包含的布局传递数据。例如，你有 diff-diff 屏幕，它使用一个公共布局，包含一个编辑文本、标签、图标和按钮。我们希望在 diff-diff 位置包含该布局。

```
<layout xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:bind="http://schemas.android.com/apk/res-auto">
   <data>
       <variable name="user" type="com.example.User"/>
   </data>
```

假设在主布局中你有一个类对象，你想在包含的布局中传递它。

使用`bind`作为属性，然后是您在包含的布局中使用的数据对象的名称。

您可以传递任何具有相同数据对象名称的内容。

# 绑定适配器

它帮助你创建一个合适的框架来设置价值观。例如

![](img/e72e7867c53066878d8aa13c6f4dc65a.png)

在给定的图案中展示文字，我们可以做 2 件事:
1。使用给定样式的 2 diff 文本视图
2。使用单一文本视图并在运行时动态设置样式

使用绑定适配器，我们将使用绑定做第二件事。

步骤 1:创建绑定适配器类。文件名可以是任何东西。

步骤 2:创建一个方法，在名字的地方用
`@BindingAdapter(value = [**“<name>”**])` 注释它，你可以定义一个变量名，我们需要在我们的 XML 文件中使用它来显示数据，或者使用适配器定制它。

```
**fun** bindSleepProgressText(textView: TextView, sleepProgressData: SleepProgressData) {
.........
}
```

作为方法参数，首先我们将拥有视图 id，第二个是我们需要显示的值。

所以对于我们给定的设计方法会是这样的:

它将以定制的形式返回文本。

步骤 3:在布局文件中使用:

`sleepProgressData`哪个被注释的名字将被用作你的视图中的属性并传递参数。查看它将自动获取的参数。

# 绑定方法

对于名为`example`的属性，库会自动尝试寻找接受兼容类型作为参数的方法`setExample(arg)`。在搜索方法时，不考虑属性的名称空间，只使用属性名称和类型。

即使不存在具有给定名称的属性，数据绑定也能工作。然后，您可以使用数据绑定为任何 setter 创建属性。例如，支持类`[DrawerLayout](https://developer.android.com/reference/androidx/drawerlayout/widget/DrawerLayout)`没有任何属性，但是有很多 setters。下面的布局自动使用`[setScrimColor(int)](https://developer.android.com/reference/androidx/drawerlayout/widget/DrawerLayout#setScrimColor(int))`和`[setDrawerListener(DrawerListener)](https://developer.android.com/reference/androidx/drawerlayout/widget/DrawerLayout#setDrawerListener(android.support.v4.widget.DrawerLayout.DrawerListener))`方法分别作为`app:scrimColor`和`app:drawerListener`属性的设置器:

# 使用 LiveData 向 UI 通知数据更改

您可以使用`LiveData`对象作为数据绑定源，自动通知 UI 数据的变化。

要使用带有绑定的动态数据，我们需要定义生命周期所有者来定义动态数据的范围。

```
binding.setLifecycleOwner(this)
```

每当你的动态数据发生任何更新，它将改变用户界面。

您可以使用一个`ViewModel`组件，将数据绑定到布局。

# 将 ViewModel 绑定到更新 UI

将`ViewModel`组件与数据绑定库一起使用允许您将 UI 逻辑移出布局并移入组件，这样更容易测试。

# 双向数据绑定

使用单向数据绑定，您可以在属性上设置一个值，并设置一个对该属性的更改做出反应的侦听器。双向数据绑定为这一过程提供了一条捷径。

在双向绑定中，我们使用`@={}`符号，它接收对属性的数据更改，同时监听用户更新。

**例如:**您正在填充用户配置文件，因此首先我们使用绑定获取已经保存的数据。当用户更新表单中的信息时，我们更新 ViewModel 中的实时数据，所以当用户单击保存按钮时，实时数据将有更新信息。

`android:text=”@={viewModel.name}”`，它将从 ViewModel 获取用户名，当用户更新该值时，它也将更新实时数据。

由于在我们的应用程序中灵活绑定数据的健壮性，数据绑定可以在我们的应用程序中有选择地使用。您可以选择在一个布局文件中使用数据绑定，也可以选择在另一个布局中完全避免使用数据绑定，因为它变得很难管理而且非常混乱。这完全取决于你。

感谢您的阅读。👏我真的希望这篇文章对你有所帮助。非常感谢你的鼓掌帮助其他人找到这篇文章😃。

![](img/bf024829742f625ee65e4c09cdc00380.png)