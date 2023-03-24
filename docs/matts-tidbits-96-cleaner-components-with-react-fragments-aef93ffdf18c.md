# Matt 的花絮#96 —含 React 碎片的清洁剂成分

> 原文：<https://medium.com/nerd-for-tech/matts-tidbits-96-cleaner-components-with-react-fragments-aef93ffdf18c?source=collection_archive---------29----------------------->

![](img/38aac84142469fb3e19295382d341213.png)

上一次，我写了关于在 TypeScript 中定义常量。这周我想分享一下我对 React 片段的了解！

对于我的许多 Android 开发人员读者来说，从构建 Android UI 的角度来看，你已经了解了所有关于片段的知识。然而，在 React/React 本地世界中，片段服务于完全不同的目的。

首先，一个例子—假设您想要定义一个返回某些元素的方法(或组件)，例如:

```
const Stack = createStackNavigator()const SomeComponent = () => {
  return (
    <Stack.Navigator>
      getScreens()
    </Stack.Navigator>
  );
};
```

`getScreens()`最有可能的第一个实现是返回一个数组(至少，我一开始是这样做的):

```
const getScreens = () => {
  return [
    <Stack.Screen name="Screen1" component={Screen1Component} />,
    <Stack.Screen name="Screen2" component={Screen2Component} />,
    ...
  ];
};
```

不幸的是，这种方法会产生一个编译器警告:

```
Warning: Each child in a list should have a unique "key" prop
```

[这样做的原因是 React 规定列表中的每个元素都必须有一个唯一可识别的“key”属性](https://reactjs.org/docs/lists-and-keys.html)——主要是为了帮助比较同一个列表的两个版本——是添加了新元素，还是删除了一些元素，或者现有元素在列表中的位置发生了变化？这是一个很好的特性，但是对于我们的屏幕列表来说，为每个项目定义一个“key”属性有点多余。我们已经有了一个惟一的“键”(`name`字段)，此外，我们不期望这个列表会随着时间而改变。

谢天谢地，React 为这个问题提供了一个更干净的解决方案——[片段](https://reactjs.org/docs/fragments.html)！

如果我们使用一个片段，下面是`getScreens()`的样子:

```
const getScreens = () => {
  return (
    <React.Fragment>
      <Stack.Screen name="Screen1" component={Screen1Component} />
      <Stack.Screen name="Screen2" component={Screen2Component} />
    </React.Fragment>
  );
};
```

瞧啊。这同样有效，我们不需要在每一行的末尾添加逗号，最重要的是，我们不需要在每一项上定义一个“key”属性。

你可以使用一种更简洁的简写方式，那就是用短语法`<>`替换`<React.Fragment>`，以进一步清理这种情况，并清楚地表明该片段实际上只是一个包装器。

另一种方法是将元素包装在 React `<div>`或 React Native `<View>`元素中，但是这有一些缺点:

*   您实际上是向视图层次结构添加了一个额外的项目——片段在呈现过程中会消失。
*   有些场景，比如上面的 React 导航示例，不允许将除了`<Stack.Screen>`之外的任何元素直接嵌入到`<Stack.Navigator>`中——将片段作为唯一可行的选项。

我希望您学到了一些关于片段的有用知识，这将有助于改进您的 React 代码！你用其他/独特的方式使用片段吗？我很想在评论中听到它！

有兴趣和我一起在埃森哲出色的数字产品团队工作吗？我们在招人！