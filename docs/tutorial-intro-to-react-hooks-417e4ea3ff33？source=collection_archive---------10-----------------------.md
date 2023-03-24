# 教程:React 挂钩简介

> 原文：<https://medium.com/nerd-for-tech/tutorial-intro-to-react-hooks-417e4ea3ff33?source=collection_archive---------10----------------------->

本教程假设您了解 React 状态和生命周期的概念。

![](img/9902c90cc4d4b55f0b9de2d2d703740d.png)

照片由[西格蒙德](https://unsplash.com/@sigmund?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 在我们开始教程之前

在本教程中，我们将构建一个小游戏。这是一种习惯于使用钩子构建 react 功能组件的实用方法。我们将带着代码片段浏览本教程的每一部分，这样你就可以在构建游戏的时候跟着做了。

本教程分为以下几个部分:

*   **教程**的设置将为你提供启动代码
*   **概述**将探究钩子的一些历史基础
*   构建游戏将使用 React 开发中最常见的钩子
*   **添加时间限制**将延长游戏添加时间限制
*   **总结**将讨论扩展并得出结论

你可以继续下去，直到你建立了一个游戏的基本版本，通过一些实践来了解钩子。

## 我们在建造什么？

在本教程中，我们将使用 React hooks 构建一个交互式 hangman 游戏。

Hangman 是一个经典游戏，玩家必须一次猜一个单词的一个字母。你可以[玩](https://www.hangmanwords.com/play)来适应这个游戏。

有几个规则可以应用到游戏中来增加更多的复杂性，但我们将专注于完成游戏的第一次迭代。我们鼓励您针对扩展部分中建议的更复杂的用例来试验和扩展这个解决方案。

## 先决条件

我们将假设您以前使用过 React，并且熟悉创建组件、状态管理和生命周期方法。
我们也在使用 ES6 的特性——箭头函数、常量、let 语句。你可以查一下[巴别塔 REPL](https://babeljs.io/repl/#?presets=react&code_lz=MYewdgzgLgBApgGzgWzmWBeGAeAFgRgD4AJRBEAGhgHcQAnBAEwEJsB6AwgbgChRJY_KAEMAlmDh0YWRiGABXVOgB0AczhQAokiVQAQgE8AkowAUAcjogQUcwEpeAJTjDgUACIB5ALLK6aRklTRBQ0KCohMQk6Bx4gA) 了解 ES6 编译成什么。
注意，我们在本教程中使用钩子，因为钩子已经在 React 版本 16.8 中引入，所以你需要将 16.8 作为最小值。本教程的 React 版本。

# 教程的设置

让我们开始吧。
我们想首先创建一个 react 应用程序。我们可以从头开始创建它，或者使用 create-react-app 来减少样板代码。在本教程中，我们将使用[创建-反应-应用](https://reactjs.org/docs/create-a-new-react-app.html)。

```
npx create-react-app react-hangman
cd react-hangman
npm start
```

上面的代码片段将创建一个带有简单应用程序组件的 React 应用程序。对于本教程，我们不会关注组件的样式和测试，所以让我们继续删除`App.css`和`App.test.js`文件。现在我们可以简单地编辑`App.js`来包含`Hangman`组件。`Hangman.jsx`是我们在本教程中要重点构建的东西。

```
//App.js
import React from 'react';
import Hangman from './Hangman';

const App = () => <Hangman />

export default App;
```

[查看此时的完整代码](https://github.com/meenakshi-dhanani/react-hangman/commit/3df118d7a3cbf0bf63466d9ad17a2abb39ac9a23)

# 概观

现在你已经准备好了，让我们先来了解一下 React 钩子。

## 什么是 React 钩子？

在 16.8 之前，React 中的类组件用于管理状态，并且具有跨生命周期方法分布的逻辑。功能组件被用来提取一些常见的用户界面。使用 React 挂钩，您现在可以挂钩到您的功能组件状态和逻辑，这些状态和逻辑以前会跨生命周期方法传播。相关的逻辑现在可以在一个地方，而不是被分割。通过构建定制的钩子，逻辑也可以跨组件共享。

# 构建游戏

作为第一次迭代的一部分，我们希望显示一个秘密单词，假设我们用 __ 屏蔽了所有字母，我们需要列出所有字母 A-Z，以便玩家可以选择一个字母，如果该字母是秘密单词的一部分，它就会显示出来。

假设暗号是“刽子手”。那么下面的表达式应该将秘密单词屏蔽为:

_ _ _ _ _ _ _

```
"HANGMAN".split("").fill("_").join(" ")
```

让我们从一个基本布局开始:

```
//Hangman.jsx
import React from 'react';

export default function Hangman() {
    const word = "HANGMAN";
    const alphabets = ["A", "B", "C", "D", "E", "F", "G",
        "H", "I", "J", "K", "L", "M", "N", "O", "P", "Q", "R",
        "S", "T", "U", "V", "W", "X", "Y", "Z"];

    return  <div>
            <p>{word.split("").fill("_").join(" ")}</p>
            {alphabets
            .map((alphabet, index) => 
            <button key={index}>{alphabet}</button>)}
            </div>
}
```

在这种状态下，如果你点击按钮，没有任何行动发生。我们的下一步将是点击一个字母表，并猜测该字母是否是单词的一部分。如果字母确实是单词的一部分，它就会显示出来，如果不是，它就不会显示出来。为此，我们需要保存所有正确猜测的字母，以便它们作为秘密单词的一部分显示出来。现在我们有了一个跨组件持久化数据的用例。这就需要国家。让我们看看如何在 React 中使用状态钩子注入状态。

## 州钩

我们可以使用状态挂钩将状态注入 React 中的功能组件。该状态将在组件的重新呈现过程中保留。`useState`是一个我们可以使用的钩子。`useState`返回一对状态的当前值和一个允许您设置状态的函数。在类组件中，我们曾经用`this.setState`做过类似的事情。对于需要保留的不同值，可以在一个组件中使用多个`useState`。

我们需要为 Hangman 组件保持正确的猜测。让我们使用 useState 钩子。我们修改了这个单词，对所有尚未猜到的字母显示 __。

```
import React, {useState} from 'react';

export default function Hangman() {
    const word = "HANGMAN";
    const alphabets = ["A", "B", "C", "D", "E", "F", "G",
        "H", "I", "J", "K", "L", "M", "N", "O", "P", "Q", "R",
        "S", "T", "U", "V", "W", "X", "Y", "Z"];
    const [correctGuesses, setCorrectGuesses] = useState([])    

    const maskedWord = word.split('').map(letter => 
    correctGuesses.includes(letter) ? letter : "_").join(" ");

    return  <div>
            <p>{maskedWord}</p>
            {alphabets
            .map((alphabet, index) => 
            <button key={index} onClick={() => {
                if (word.includes(alphabet)) {
                    setCorrectGuesses([...correctGuesses, alphabet])
                }
            }}>{alphabet}</button>)}
            {!maskedWord.includes("_") && <p>You won!</p>}
            </div>
}
```

# 添加时间限制

现在我们有了一个公平可行的解决方案，让我们给这个游戏添加一些规则。我们最多有 2 分钟的时间来猜这个单词，如果在 2 分钟内没有猜中，我们将显示“游戏结束”。

在这种情况下，我们需要注入一个超时。超时会影响这场比赛的结果。让我们看看效果挂钩，以了解如何在组件中添加超时逻辑。

## 效果挂钩

效果挂钩是 React 中另一个最常用的挂钩。它接受一个函数(效果),当它的任何一个因变量改变时，这个函数就会运行。effect(副作用的缩写)钩子，用于管理组件上的任何副作用——操作 DOM 元素、获取数据、订阅等。在我们的例子中，我们将使用`useEffect`来设置超时。默认情况下，`useEffect`为每个组件渲染运行，除非我们提到`[]`作为它的参数，在这种情况下，它只在组件的第一次渲染期间运行。

```
import React, { useEffect, useState } from 'react';

export default function Hangman({duration = 120000}) {
    const word = "Hangman".toUpperCase();
    const alphabets = ["A", "B", "C", "D", "E", "F", "G",
        "H", "I", "J", "K", "L", "M", "N", "O", "P", "Q", "R",
        "S", "T", "U", "V", "W", "X", "Y", "Z"];
    const [correctGuesses, setCorrectGuesses] = useState([])
    const [timeUp, setTimeUp] = useState(false);

    useEffect(() => {
        const timeout = setTimeout(() => {
            setTimeUp(true);
        }, duration);

        return () => clearTimeout(timeout);
    }, [])

    const maskedWord = word.split('').map(letter => correctGuesses.includes(letter) ? letter : "_").join(" ");
    return (
        <div>
            <p>{maskedWord}</p>
            {alphabets.map((alphabet, index) => <button key={index} onClick={() => {
                if (word.includes(alphabet)) {
                    setCorrectGuesses([...correctGuesses, alphabet])
                }
            }}>{alphabet}</button>)}
            {timeUp ? 
            <p>You lost!</p> : 
            !maskedWord.includes("_") &&  <p>You won!</p>}
        </div>
    );
}
```

注意我们是如何使用`useState`来保存 timeUp 的状态的。在`useEffect`的第二个参数中我们提到了`[]`，所以超时只在 Hangman 第一次渲染的时候设置。最后，当组件在游戏结束后卸载时，我们在`return () => clearTimeout(timeout)`中清除效果。这可以用来退订，清理所用资源的效果。

# 包扎

恭喜你！你有一个刽子手游戏:

*   我们来玩刽子手吧
*   有一个时间上限让你猜

我们希望你已经掌握了基本的钩子。

本教程试图让你开始使用 react 钩子。我们将进一步鼓励你探索更多的钩子，例如，使用上下文，使用历史，创建你自己的定制钩子。等等。点击查看挂钩[的详细说明。](https://reactjs.org/docs/hooks-overview.html)

有很多规则可以应用，游戏可以进一步扩展。对你来说，用钩子试试这些功能部件是一个很好的练习。

*   最多允许猜测 6 次
*   显示计时器的剩余时间
*   限制对元音的猜测
*   获取基于主题的单词列表

你可以在[这个](https://github.com/meenakshi-dhanani/react-hangman)资源库中找到游戏的完整源代码。