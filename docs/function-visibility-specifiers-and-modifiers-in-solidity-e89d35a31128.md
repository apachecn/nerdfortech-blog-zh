# Solidity 中的函数可见性说明符和修饰符

> 原文：<https://medium.com/nerd-for-tech/function-visibility-specifiers-and-modifiers-in-solidity-e89d35a31128?source=collection_archive---------0----------------------->

这就是你需要知道的…

![](img/662b8a10d514ffb87a9250c11db5336e.png)

由 [Milad Fakurian](https://unsplash.com/@fakurian?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

可见性说明符和修饰符定义了可靠性智能契约的功能的可见性和行为。通过限制对功能和数据成员的访问，这些也可以被认为是安全措施。

# 可视性说明符

这些非常类似于其他面向对象语言(如 c++和 java)的访问修饰符/说明符。这些定义了给定函数相对于其他契约的可访问性。

Solidity 中有 4 种可见性说明符。这些是`public`、`private`、`internal`和`external`。

注意:这些说明符并不意味着你的`private`代码将对世界隐藏。它仍然可以在区块链上看到。它只是限制从同一个/另一个契约对该函数的调用。

## 公众的

这是限制最少的说明符。`public`说明符意味着可以从同一个合同以及其他合同中访问`function`。

当从同一个契约中调用函数时，它使用简单的机器码跳转来到达那个函数。如果来自另一个契约的函数被称为 EVM 消息调用。第一种比第二种更省油。

```
contract First { function myFunction() public returns (bool) { return true; } function internalFunction() public { myFunction(); // works! }} function outerFunction() { new First().myFunction();}contract Second { function mySecondFunction() public { new First().myFunction(); // works! }
  function useOuterFunction() public { outerFunction(); // works! }}
```

## **私人**

该说明符将函数的调用限制在同一个协定中。它不能从任何其他契约外部调用。

这里，`this`关键字也不起作用，因为用`this`调用的函数被视为外部 EVM 消息调用。

```
contract First { function myFunction() private returns (bool) { return true; } function internalFunction() public { myFunction(); // works

    this.myFunction(); // TypeError: Member "myFunction" not found or not visible }}contract Second { function mySecondFunction() public { new First().myFunction(); // TypeError: Member "myFunction" not   found or not visible }}function outerFunction() { new First().myFunction(); // TypeError: Member "myFunction" not found or not visible}
```

## 内部的

内部函数仅指定同一协定或派生/继承协定的可见性。

```
contract First { function myFunction() private returns (bool) { return true; } function internalFunction() public { myFunction(); // works

    this.myFunction(); // TypeError: Member "myFunction" not found or not visible }}contract FirstDerived is First { function derivedFunction() public { myFunction(); // works }}contract Second { function mySecondFunction() public { new First().myFunction(); // TypeError: Member "myFunction" not   found or not visible }} function outerFunction() { new First().myFunction(); // TypeError: Member "myFunction" not found or not visible}
```

## 外部的

只能从不派生当前协定的其他协定中调用外部函数。这和内部函数完全相反(咄！)

```
contract First { function myFunction() external returns (bool) { return true; } function internalFunction() public { myFunction(); // DeclarationError: Undeclared identifier. this.myFunction(); // works }} contract FirstDerived is First { function derivedFunction() public { myFunction(); // DeclarationError: Undeclared identifier. }} contract Second { function mySecondFunction() public { new First().myFunction(); // works }} function outerFunction() { new First().myFunction(); // works}
```

# 修饰语

可见性说明符定义了协定的可见性，而修饰符定义了协定的行为以及它们与数据成员和其他协定的关系。

## 纯的

这个修饰符不允许对任何`storage`变量的读/写访问。读/写访问仅针对`memory`或`calldata`变量。

```
address owner;uint id = 1;function pureFun(uint data) public pure { data = 10; // works! uint newData = data + 100; // works! id = 2; // TypeError: Function cannot be declared as pure because  this expression (potentially) modifies the state address newOwner = owner; // TypeError: Function declared as pure, but this expression (potentially) reads from the environment or state and thus requires "view"}
```

## 视角

带有`view`修饰符的函数只允许只读操作。

```
address owner;uint id = 1;function viewFun(uint data) public view { data = 10; // works! uint newData = data + 100; // works! id = 2; // TypeError: Function cannot be declared as view because this expression (potentially) modifies the state. address newOwner = owner; // works!}
```

## 应付的

`payable`允许函数接收消息调用中的`ether`，并使用`msg.value`访问发送的以太网。此外，发送的`ether`被添加到接收合同的余额中。

这也不限制对`storage`变量的任何读/写。

```
contract Payee { address sender; uint value; function payableFun() external payable { sender = msg.sender; value = msg.value; } function nonPayableFun() external { sender = msg.sender; value = msg.value; }} contract Payer { function payer() public { Payee payee = new Payee(); payee.payableFun{value: 5}(); // works! payee.nonPayableFun{value: 5}(); // TypeError: Cannot set option "value" on a non-payable function type }}
```

## 虚拟和覆盖

这些修饰符分别指定一个函数是否应该被重写，以及一个函数是否正在重写一个函数

```
contract Parent { function overrideThis() public virtual { } function dontOverrideThis() public { // TypeError: Trying to override non-virtual function. Did you forget to add "virtual"? }}contract Child is Parent { function overrideThis() public override { } function dontOverrideThis() public override { } function doesntOverride() public override { // TypeError: Function has override specified but does not override anything. }}
```

这就是所有的人！请留下👏如果你学到了什么？下一集再见！

# 有用的链接:

 [## 坚固性-坚固性 0.8.16 文件

### 部署合同时，您应该使用最新发布的 Solidity 版本。除了特殊情况，只有…

docs.soliditylang.org](https://docs.soliditylang.org/en/v0.8.16/index.html) 

编辑用:[https://remix.ethereum.org/](https://remix.ethereum.org/)