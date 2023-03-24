# 如何用 Jest 测试 TypeScript

> 原文：<https://medium.com/nerd-for-tech/testing-typescript-with-jest-290eaee9479d?source=collection_archive---------0----------------------->

[TypeScript](https://www.typescriptlang.org/) 是一种非常流行的语言，表现为 JavaScript 的类型化超集。当您使用 TypeScript 编写新的 Node.js 项目或将现有的 JavaScript 代码升级到 TypeScript 时，您可能想知道如何测试代码。

我有机会在 Node.js 项目中使用 TypeScript 编写 lambda 代码。当我试图用 TypeScript 编写单元测试时，我遇到了一些困难，我希望你在读完这篇文章后不会遇到这些困难。

在这篇文章中，我将展示使用一个流行的 JavaScript 测试框架 [Jest](https://jestjs.io/) 测试您的类型脚本代码的必要步骤，并提供您在编写单元测试时可能会遇到的一些常见问题的解决方案。对于下面提供的示例命令，我将使用 npm 作为包管理器。
我的项目安装了下面提到的包的以下版本:
-@ types/jest:" ^26.0.20 "
-" jest ":" ^26.6.3 "
-" ts-jest ":" ^26.4.4 "
-" typescript ":" ^3.7.5 "

# 先决条件

> *通过运行以下命令将* `*jest*` *和* `*typescript*` *安装到您的项目中:* `*npm i -D jest typescript*`

# **第一步**

> *通过运行以下命令将* `*ts-jest*` *和* `*@types/jest*` *安装到您的项目中:* `*npm i -D ts-jest @types/jest*`

# 第二步

> *通过运行以下命令，在与* `*package.json*` *相同的级别创建一个名为* `*jest.config.js*` *的配置文件:*  *该文件应该有以下代码:*

```
module.exports = {
  preset: "ts-jest",
  testEnvironment: "node"
};
```

# 第三步

> *在* `*package.json*` *的同一层创建一个名为* `*tests*` *的文件夹，并将您的测试文件放在这个文件夹下。测试文件应该遵循* `*{file_name}.test.ts*` *的命名约定。通过运行以下命令执行测试:* `*npm t*`

# **常见问题解答**

> **问:***我如何模仿一个导入的类？(用例:类 A 导入类 B，我想在测试类 A 时模仿类 B)。*
> 
> **答:**

```
// file: class_a.test.jsimport { ClassB } from "../src/class_b";
jest.mock("../src/class_b");it("should mock class B", () => {
  const functionNameMock = jest.fn();
  ClassB.mockImplementation(() => {
    return {
      functionName: functionNameMock
    };
  });
});
```

> *然而，这不能用于 TypeScript。它将显示类似以下内容的编译错误:“类型‘type of class b’上不存在属性‘mock implementation’”。ts”。而是可以在* `*ClassB.prototype*` *上使用* `*jest.spyOn*` *。*

```
// file: class_a.test.jsimport { ClassB } from "../src/class_b";
jest.mock("../src/class_b");it("should mock class B", () => {
  const functionNameMock = jest.fn();
  jest.spyOn(ClassB.prototype, "functionName").mockImplementation(functionNameMock);
});
```

> **问:**
> *如何模拟导入类的静态函数？*
> 
> **答:**
> *上面展示的用来模仿导入类的函数的方法对静态函数不起作用。相反，你可以使用* `*jest.Mocked<typeof ClassB>*` *来模仿静态函数。*

```
// file: class_a.test.jsimport { ClassB } from "../src/class_b";
jest.mock("../src/class_b");it("should mock static function named 'staticFuncName' of class B", () => {
  const staticFuncNameMock = jest.fn();
  const MockClassB = ClassB as jest.Mocked<typeof ClassB>;
  MockClassB.staticFuncName.mockImplementation(staticFuncNameMock);
});
```

> **问:**
> *如何模拟异步函数？*
> 
> **答:**
> *你既可以模拟异步函数的结果，也可以模拟异步函数本身，这取决于你想测试什么。*

```
// file: class_a_test.js
import { ClassA } from "../src/class_A";
jest.mock("../src/class_a");it("should mock result of async function of class A, async () => {
  const classInstance = new ClassA();
  jest.spyOn(classInstance, "asyncFunction").mockImplementation(async () => "");
});it("should mock async function of class A, async () => {
  const classInstance = new ClassA();
  const asyncFunctionMock = jest.fn().mockResolvedValue("");
  jest.spyOn(classInstance, "asyncFunction").mockImplementation(asyncFunctionMock);
});
```

> **问:**
> *如何用无效的参数类型测试一个函数的行为？(用例:函数 A 需要一个接口类型为 B 的参数，当我传递一个与接口 B 不匹配的参数时，我想测试函数 A 的行为。例如，用户向触发 lambda 函数的 API 发送一个带有正文的 HTTP 请求，而您想测试您的 lambda 函数如何处理来自用户的无效输入。)*
> 
> **A:***根据 TypeScript 的性质，将无效类型作为参数传递给函数 A 会引发编译错误，因为预期的和实际的参数类型不兼容。但是，如果您想通过传递无效类型来测试函数 A，您可以将参数类型强制转换为* `*any*` *以避免编译错误。*

```
// file: class_a.ts
export Class A {
  functionA(data: TypeB) { ... }
} // file: class_a.test.tsit("should test invalid input, () => {
  const data = { ... } as any;
  const response = classA.functionA(data);
});
```

如果您在测试 TypeScript 时遇到任何其他问题，请随时直接联系我。我很乐意帮助解决你们的问题，并学习更多关于测试 TypeScript 的知识！