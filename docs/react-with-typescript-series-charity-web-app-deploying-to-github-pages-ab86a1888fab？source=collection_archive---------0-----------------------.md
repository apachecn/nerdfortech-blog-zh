# 使用 Typescript 系列(慈善网络应用程序)作出反应—部署到 Github 页面

> 原文：<https://medium.com/nerd-for-tech/react-with-typescript-series-charity-web-app-deploying-to-github-pages-ab86a1888fab?source=collection_archive---------0----------------------->

嘿，伙计们，在上一个教程中，我们用一个漂亮的表单完成了前端部分(带有保存功能)。

[](https://billa-code.medium.com/react-with-typescript-series-charity-web-app-form-state-management-a411552097ff) [## 用 Typescript 系列(慈善 Web 应用程序)作出反应—表单状态管理

### 大家好，在我们之前的教程中，我们学习了如何使用高阶组件创建。

billa-code.medium.com](https://billa-code.medium.com/react-with-typescript-series-charity-web-app-form-state-management-a411552097ff) 

在集成后端更改之前，在本教程中，我将向您展示如何使用 Github 操作将这个 react 应用程序部署到 Github 页面。通常我们可以创建另一个名为 gh-pages 的分支，然后在那里添加我们的构建代码，并在 Github 中获得一个运行的 UI。但在本教程中，我会给你一个全自动的方法来做到这一点。但是要记住的最重要的事情是去你的 repo 并从那里启用 Github 操作。如果没有，您将看不到它。

我不会深入到什么是 Github 动作或者我们如何创建。因为有成千上万的资源可供你参考。在这里，我将向您介绍我实现的操作，并给出我们以这种方式使用它的原因。首先在名为的文件夹中创建一个名为 page_build.yml 的文件。github/workflows/。在那里添加此代码。(如果您在 repo 中进入 Github actions 并点击 new workflow，也可以从那里创建)。

```
name: React Build

on:
  pull_request:
    branches: [ "main" ]

env:
  CI: false

jobs:
  build:
    runs-on: ubuntu-latest
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    permissions:
      contents: 'read'
      id-token: 'write'
      pages: 'write'
      actions: 'write'
      checks: 'write'
      deployments: 'write'
    strategy:
      matrix:
        node-version: [18.x]

    steps:
    - uses: actions/checkout@v3

    - name: Use Node.js ${{ matrix.node-version }}
      uses: actions/setup-node@v3
      with:
        node-version: ${{ matrix.node-version }}

    - name: Build
      run: |
        npm install
        npm run build

    - name: Setup Pages
      uses: actions/configure-pages@v2
    - name: Upload artifact
      uses: actions/upload-pages-artifact@v1
      with:
        *# Upload build directory content* path: 'build/'
    - name: Deploy to GitHub Pages
      id: deployment
      uses: actions/deploy-pages@v1
      env:  
        GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

第一个是显而易见的配置。这是动作的名称。

```
name: React Build
```

接下来我们有触发器。我们可以定义这应该在什么情况下触发。在这种情况下，我们将它定义为在主分支上创建拉请求时触发。如果你想在推送中触发它。那么你也可以加上这样的东西。

```
on:
 **push:
  branches: [ "main" ] ** 
 pull_request:
  branches: [ "main" ]
```

接下来，我们有了整个动作的环境变量。这里 CI 被设为 false，否则它会将警告视为错误，构建将会失败。

```
env:
  CI: false
```

接下来，我们有一组正在运行的作业。我已经把所有的东西都放在构建工作中了。如果你愿意，你可以把它分开。在构建作业下，我们的第一个配置是运行环境。

```
runs-on: ubuntu-latest
```

接下来是 Github 回购的环境。

```
environment:
  name: github-pages
  url: ${{ steps.deployment.outputs.page_url }}
```

接下来，我们必须添加权限。

```
permissions:
  contents: 'read'
  id-token: 'write'
  pages: 'write'
  actions: 'write'
  checks: 'write'
  deployments: 'write'
```

接下来最重要的是步骤。这些第一步签出节点来运行和设置环境。

```
- uses: actions/checkout@v3

- name: Use Node.js ${{ matrix.node-version }}
  uses: actions/setup-node@v3
  with:
    node-version: ${{ matrix.node-version }}
```

然后我们构建代码

```
- name: Build
  run: |
    npm install
    npm run build
```

在下一组步骤中，我们在复制构建文件夹内容后部署到 Github 页面。

```
- name: Setup Pages
  uses: actions/configure-pages@v2
- name: Upload artifact
  uses: actions/upload-pages-artifact@v1
  with:
    *# Upload build directory content* path: 'build/'
- name: Deploy to GitHub Pages
  id: deployment
  uses: actions/deploy-pages@v1
```

最后我们有了 GITHUB_TOKEN env secret。这是必须的。

```
env:  
  GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

这就是我们所需要的。现在，对于我的回购以下是这个将被部署到的 URL。

 [## React 应用

### 使用 create-react-app 创建的网站

debilla.github.io](https://debilla.github.io/serendib-scholarship-ui/) 

但是如果你现在去这个网址。您将得到一个 404 错误。原因是我们没有为 React 应用程序正确定义主页 URL。它会将 URL 的第一部分视为 href 基根。要解决这个问题，我们必须更改 package.json 文件。新的应该是这样的。

```
{
  "name": "serendib-ui",
  "version": "0.1.0",
  "private": true,
  **"homepage": "https://debilla.github.io/serendib-scholarship-ui",**
  "dependencies": {
    "@testing-library/jest-dom": "^5.16.4",
    "@testing-library/react": "^13.1.1",
    "@testing-library/user-event": "^13.5.0",
    "@types/jest": "^27.4.1",
    "@types/node": "^16.11.31",
    "@types/react": "^18.0.8",
    "@types/react-dom": "^18.0.0",
    "bootstrap": "^5.1.3",
    "react": "^18.1.0",
    "react-bootstrap": "^2.3.1",
    "react-data-grid": "7.0.0-beta.15",
    "react-dom": "^18.1.0",
    "react-scripts": "5.0.1",
    "typescript": "^4.6.3",
    "web-vitals": "^2.1.4"
  },
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
  },
  "eslintConfig": {
    "extends": [
      "react-app",
      "react-app/jest"
    ]
  },
  "browserslist": {
    "production": [
      ">0.2%",
      "not dead",
      "not op_mini all"
    ],
    "development": [
      "last 1 chrome version",
      "last 1 firefox version",
      "last 1 safari version"
    ]
  }
}
```

现在，如果你创建一个 PR 到主分支，动作将被触发，你将在 Github 页面看到部署的 UI 应用程序。快乐的编码家伙:p。下次我们将会见一个方法来创建这个应用程序的后端。