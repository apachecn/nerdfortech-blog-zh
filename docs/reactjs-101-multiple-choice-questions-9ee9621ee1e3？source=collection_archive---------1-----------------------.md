# ReactJS 101 选择题

> 原文：<https://medium.com/nerd-for-tech/reactjs-101-multiple-choice-questions-9ee9621ee1e3?source=collection_archive---------1----------------------->

## 5 个 ReactJS 面试问题

# 序言

随着四月的到来，我即将迎来第一次学习反应技能的第六个月。我目前正在准备任何可能的面试问题，并意识到有很多我不知道的！我认为这将是你我通过一些有趣的测验来学习反应的一种有趣的方式。我们开始吧！

![](img/69719a9622870cbf7800f6797d48c139.png)

信用:[米奇·盖文](https://mitchgavan.com/react-quiz/)

# 问题

**Q1。什么是和解？**

A.React 删除 DOM 的过程。

B.React 更新和删除组件的过程。

C.这是一个设定状态的过程。

D.React 更新 DOM 的过程。

.

.

.

.

.

**答:D！！**

说明:协调是 React 更新 DOM 的过程。使用“不同”算法进行反应，以便组件更新可预测且更快。当有组件更新时，React 将首先计算真实 DOM 和 DOM 副本之间的差异。一旦计算完成，新的更新就会反映在真实的 DOM 上。

Q2。组件生命周期的正确阶段是什么？

A.挂载:`getDerivedStateFromProps()`；更新:`componentWillUnmount()`；卸载:`shouldComponentUpdate()`

B.挂载:`componentWillUnmount()`；更新:`render()`；卸载:`setState()`

C.挂载:`componentDidMount()`；更新:`componentDidUpdate()`；卸载:`componentWillUnmount()`

D.挂载:`constructor()`；更新:`getDerivedStateFromProps()`；卸载:`render()`

.

.

.

.

.

Ans: **C！！！**

说明:值得一提的是，在对 DOM 应用更改时，React 内部有一个阶段的概念，包括**渲染**、**预提交**和**提交**。`componentDidMount()`、`componentDidUpdate()`、`componentWillUnmount()`属于“提交”阶段。这里有一个[交互版](https://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/)，展示了每个阶段的每个生命周期方法。

**Q3。受控组件与非受控组件**

A.受控组件:每个状态突变都有一个相关的处理函数；不受控制的组件:在内部存储它们自己的状态

B.受控组件:在内部存储它们自己的状态；不受控制的组件:每个状态突变都有一个相关的处理函数

C.受控组件:如此擅长自我控制的组件；不受控制的组件:不知道如何控制自己的组件

D.受控组件:每个状态突变都没有关联的处理函数；不受控制的组件:不在内部存储它们自己的状态

.

.

.

.

.

**答！！！**

解释:只是澄清一下，因为这个定义可能有点难以理解。受控组件的示例:

```
handleChange(events) {
  this.setState({ value: event.target.value.toUpperCase() })
}
```

在这种情况下，每个值都将以大写字母打印，这就是`toUpperCase()`，一个相关的函数处理程序。

非受控组件示例(取自 React Doc):

```
class NameForm extends React.Component {
  constructor(props) {
    super(props);
    this.handleSubmit = this.handleSubmit.bind(this);
    this.input = React.createRef();  
  }

  handleSubmit(event) {
    alert('A name was submitted: ' + this.input.current.value);    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Name:
          <input type="text" ref={this.input} />        
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}
```

在这种情况下，值的输入存储在`ref`中，可用作表单的参考。
不过，React 推荐总是使用受控组件来实现表单，这由 React 组件来处理，而不受控组件由 DOM 来处理。

**Q4。状态 vs 道具**

A.道具是家长不需要的东西，决定扔在其他家长中间；状态是父母最喜欢的孩子，也是组件想要培养的东西。

B.使用命名约定将属性传递给组件，比如函数参数；状态是在组件中管理的，它包含一些在组件的生命周期中可能会改变的信息。

C.道具是真实 DOM 的副本；状态是真正 DOM 的定义。

D.Prop 是在组件中管理的，它保存一些在组件的生命周期中可能会改变的信息；状态像函数参数一样传递给组件

.

.

.

.

.

Ans: **B！！！**

解释:我们可以使用`setState()`更新组件内的状态，然后我们可以将状态传递给其子组件。它的孩子将使用`this.props`从它的父母那里获得任何状态。

示例:

```
// State.js <- pass the state "name" to Name.js
class Form extends React.Component {
  state = {
    name = 'Megan';
  }

  render() {
    <Name name={this.state.name} />
}// Name.js <- it got the state "name" from State.js
class Name extends React.Component { 
  render() {
    <div>
      <p>{this.props.name}</p>
    </div>
  }
}
```

**Q5。什么是“关键”道具？**

A.“关键”道具只是看起来很漂亮，没有任何好处。

B.“Key”prop 是 React 在“diffing”算法中识别列表中新添加的项目并进行比较的一种方法。

C.它是 HTML 中的属性之一。

D.我只知道在数组中不常用。

.

.

.

.

.

Ans。 **B！！！**

解释:如答案中所述，我们会得到“丢失键”警告的原因是 React 需要“键”属性来识别重新渲染时列表中的任何新项目。这是为了更高的性能和效率。这是我写的一篇文章,详细介绍了背后的目的。

# 结论

我现在就知道这么多了！希望你得到了所有正确的答案！我需要道歉，我不擅长写小测验。但是这里有一些你必须知道的 React 的基础知识。我希望将来能制作第二部分，让我们所有人对面试有更多的了解和准备:)

最后但同样重要的是…

![](img/476d07ccbd1e826325dca703ec523f57.png)

感谢您一如既往地成为一名出色的读者！