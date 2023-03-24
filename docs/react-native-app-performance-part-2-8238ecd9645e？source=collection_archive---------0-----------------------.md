# 反应原生应用性能-第 2 部分

> 原文：<https://medium.com/nerd-for-tech/react-native-app-performance-part-2-8238ecd9645e?source=collection_archive---------0----------------------->

![](img/3e9e1405f4a9150be078fd1dfff4beda.png)

图片由 [alan9187](https://pixabay.com/users/alan9187-2347/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=534120) 来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=534120)

好的性能对每个 app 都很重要。使用 React Native 时，您可能会经常遇到应用程序性能方面的问题。这就是为什么您需要在开发过程中关注 React 本机应用程序的最佳实践和性能改进，以便您可以修复这些问题，并向最终用户提供完美的体验。我将在下面分享一些最佳实践。

# 避免使用 ScrollView 来呈现巨大的列表

在 React Native 中，有几种方法可以显示带有可滚动列表的项目。在 React Native 中实现列表的最常见方式是使用`ScrollView`和`FlatList`组件。

`ScrollView`实现起来很简单。它通常用于迭代有限数量的条目列表，如下所示。但是，它会一次渲染所有子对象。当列表中的项目数量很少时，这种方法很好。另一方面，使用数据量大的`ScrollView`会直接影响 React Native app 的整体性能。

```
<ScrollView>
  {items.map(item => {
    return <Item key={item.name.toString()} />;
  })}
</ScrollView>
```

为了处理列表格式的大量数据，React Native 提供了`FlatList`。`FlatList`中的项目是惰性加载的。因此，该应用程序确实使用了过多或不一致的内存量。`FlatList`可以如下图所示使用:

```
<FlatList
  data={elements}
  keyExtractor={item => `${items.name}`}
  renderItem={({ item }) => <Item key={item.name.toString()} />}
/>
```

> 你也可以尝试使用 [recyclerlistview](https://github.com/Flipkart/recyclerlistview) 来提升大型列表的性能

# 避免将内联函数作为道具传递

当将函数作为属性传递给组件时，避免内联传递该函数，如下所示:

```
function Card(props) {
  return(
    <Button title=' Make Beverage' onPress={props.onPress}/>

  )
}
export default function Main() {

  return (
    <View style={styles.container}>
      <Card
        onPress={()=> console.log('clicked on card')}
      />
    </View>
  );
}
```

不推荐使用上面的方法，因为每当父元素重新呈现一个新的引用时，这个函数就会被重新创建。这意味着即使道具根本没有改变，子组件也会重新渲染。

解决方案是将函数声明为类方法或函数组件中的函数，这样引用就消除了任何交叉呈现的可能性。

```
export default function Main() {
  function handleCard(){
    console.log('clicked on card')
  }
  return (
    <View style={styles.container}>
      <Card
        onPress={handleCard}
      />
    </View>
  );
}
```

# 避免更新`componentWillUpdate`中的`state`或`dispatch`动作

您应该利用`componentWillUpdate`生命周期方法来准备更新，而不是触发另一个更新。如果你的目标是设置一个状态，你应该使用`componentWillReceiveProps`来代替。为了安全起见，使用`componentDidUpdate`而不是`componentWillReceiveProps`来调度任何冗余动作。

# 避免不必要的重新渲染

React Native 以类似于`React.js`的方式处理组件的渲染。因此，对 React 有效的优化技术也适用于 React 原生应用。一种优化技术是避免主线程上不必要的渲染。在功能组件中，这可以通过使用`React.memo()`来完成。

`React.memo()`用于处理记忆化，这意味着如果任何组件不止一次地接收同一组属性，它将使用先前缓存的属性，并且只渲染一次功能组件返回的 JSX 视图，从而节省渲染开销。

下面显示了一个很好的例子，其中`Card`组件有一个名为`count`的状态变量，每当按钮被按下时，该变量就增加 2。当按钮被按下时，`CardCount`组件被重新渲染，即使它的文本属性不会随着每次渲染而改变。它没有对其父组件`Card`做任何特殊的事情，只是显示文本。这可以通过用`React.memo()`包装`CardCount`组件的内容来优化。

```
// Card.js
const Card = () => {
  const [count, setCount] = useState(0);  return (
    <View style={{ flex: 1, justifyContent: 'center', alignItems: 'center' }}>
      <Button title='Press me' onPress={() => setCount(count + 2)} />
      <CardCount text='text' />
    </View>
  );
};// CardCount.js
const CardCount = React.Memo(({ text }) => {
  return <Text>{text}</Text>;
});
```

# 将 nativeDriver 与动画库一起使用

在 React 本地应用中渲染动画最流行的方法之一是使用[动画](https://reactnative.dev/docs/animated.html)库。

在动画在屏幕上开始之前，它使用`nativeDriver`通过本机桥发送动画。这有助于动画独立于阻塞的 JavaScript 线程执行，从而带来更流畅、更丰富的体验，而不会出现闪烁或丢帧。要将`nativeDriver`与`Animated`一起使用，可以将其值设置为`true`，如下图所示:

```
<ScrollView
  showsVerticalScrollIndicator={ false }
  scrollEventThrottle={ 1 }
  onScroll={Animated.event(
    [{ nativeEvent: { contentOffset: { y: animatedValue } } }],
    { useNativeDriver: false }
  )}
>
</ScrollView>
```

# 避免箭头功能

箭头函数是浪费重渲染的常见原因。不要在函数中使用箭头函数作为回调来呈现视图。使用 arrow 函数，每个渲染都会生成该特定函数的一个新实例，因此当协调发生时，React Native 会比较差异，因为函数引用不匹配，所以它无法重用旧引用。

```
class ArrowClass extends React.Component {
  // ...
  addTodo() {
    // ...
  }
  render() {
    return (
      <View>
        <TouchableHighlight onPress={() => this.addTodo()} />
      </View>
    );
  }
}class CorrectClass extends React.Component {
  // ...
  addTodo = () => {
    // ...
  }
  render() {
    return (
      <View>
        <TouchableHighlight onPress={this.addTodo} />
      </View>
    );
  }
}
```

# 利用 InteractionManager

[InteractionManager](https://reactnative.dev/docs/interactionmanager) 允许您在任何交互或动画完成后，在 JavaScript 线程上安排任务的执行。特别是，`InteractionManager`可以让 JavaScript 动画流畅运行。

可以使用以下代码安排任务在交互后运行:

```
InteractionManager.runAfterInteractions(() => {
  // ...long-running synchronous task...
});
```

# 渲染时避免匿名函数

在`render()`中创建函数是一种不好的做法，会导致一些严重的性能问题。每次组件重新呈现时，都会创建一个不同的回调。对于简单和较小的组件来说，这可能不是问题，但是对于`PureComponents`和`React.memo()`或者当函数作为道具传递给子组件时，这是个大问题，这会导致不必要的重新渲染。

# 利用内存优化

本机应用程序有许多进程在后台运行。你可以在 Xcode 的帮助下找到不必要的来提高性能。在 Android Studio 中，有一个 Android 设备监视器，用于监视应用程序中的漏洞。使用像`FlatListSectionList`或`isVirtualList`这样的滚动列表是提高性能的可靠方法。

您可以使用以下代码来监控性能并识别内存泄漏:

```
Import PerfMonitor from ‘react-native/libraries/Performance/RCTRenderingPerf’;
perfMonitor.toggle();
PerfMonitor.start();
SetTimeout ( () => {
perfMonitor.stop();
}; 2000);
}; 5000);
```

# 使用 Flipper 调试速度更快

对于每个开发人员来说，调试都是最具挑战性的任务之一。当一切看起来都正常的时候，引入一个新的特性是比较容易的，但是发现哪里出了问题是非常令人沮丧的。

时间是调试过程中的一个重要因素，我们通常必须快速解决问题。然而，React Native 中的调试并不十分简单，因为您试图解决的问题可能发生在 JavaScript 或本机代码中。

[Flipper](https://fbflipper.com/) 是一个手机 app 的调试平台。它还广泛支持 React Native。

# 结论

[React Native](https://reactnative.dev/) 是一个用于创建跨平台移动应用的开源框架。JavaScript 是它的核心，它有构建界面和功能的组件。这是一个流行的高性能框架，只要您从一开始就考虑性能，它就能提供无缝的大规模体验。

> **你可以看看我以前的博客，了解应用程序性能的基本变化——**
> 
> [https://medium . com/nerd-for-tech/react-native-app-performance-part-1-ea 218 DAC 23 ee](/nerd-for-tech/react-native-app-performance-part-1-ea218dac23ee)