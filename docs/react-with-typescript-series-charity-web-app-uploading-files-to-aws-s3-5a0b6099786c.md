# React with Typescript series(慈善网站应用程序)—将文件上传到 AWS S3(Github 页面部署的奖励部分)

> 原文：<https://medium.com/nerd-for-tech/react-with-typescript-series-charity-web-app-uploading-files-to-aws-s3-5a0b6099786c?source=collection_archive---------1----------------------->

嘿伙计们，在我们的慈善项目进一步发展之前，我想到了直接从前端实现 s3 文件上传。我们到目前为止所做的可以从这里借鉴。

**前端反应系列**

![Dimuthu Wickramanayake](img/099c92eba7c0ef5b0373ba4d36052ba2.png)

[迪姆图·维克拉马那亚克](https://billa-code.medium.com/?source=post_page-----5a0b6099786c--------------------------------)

## 用 Typescript 系列作出反应(慈善网络应用)

[View list](https://billa-code.medium.com/list/react-with-typescript-seriescharity-web-app-6f45ca471f54?source=post_page-----5a0b6099786c--------------------------------)7 stories![](img/ace19c3ad1c0f8dc12620ff352fd2b11.png)![](img/23deda9e9ca42920182e5a94707a12e7.png)![](img/16523ac1bf719cb2bc80e1cc1ec44297.png)

**后端 AWS Lambda NodeJS 系列**

![Dimuthu Wickramanayake](img/099c92eba7c0ef5b0373ba4d36052ba2.png)

迪穆图·维克拉马那亚克

## AWS Lambda(慈善网络应用)系列

[View list](https://billa-code.medium.com/list/aws-lambda-charity-webapp-series-cd878238f932?source=post_page-----5a0b6099786c--------------------------------)5 stories![](img/5e5d417a4f8e977dff5a6564ccf48d39.png)![](img/75f2267185bffd78647008c578e838b9.png)![](img/fa6bf2ff659d1783bd38eee735f02f2d.png)

作为第一步，让我们添加一个输入来上传文件。我将把它添加到模态组件的页脚。我的 StudentModalComponent.tsx 文件会这样改。

```
import React, { Component } from 'react';
import { Modal, Button, Form } from 'react-bootstrap';
import { withForm } from "../hoc/withForm";

export type StudentPropType = {
    show: boolean,
    form: any,
    studentid: string,
    config: any,
    onHide(): void,
    onSave(): void,
    **handleFileUpload(file: any): void**
}

class StudentModalComponent extends Component<StudentPropType> {
    constructor(props: StudentPropType) {
        super(props);
    }

    **handleFileUpload(e: any) {
        this.props.handleFileUpload(e.target.files[*0*])
    }**

    render(): React.ReactNode {
        return <Modal show={this.props.show}>
            <Modal.Header closeButton onClick={this.props.onHide}>
                <Modal.Title id="contained-modal-title-vcenter">
                    Details of Student {this.props.config[*1*].value}
                </Modal.Title>
            </Modal.Header>
            <Modal.Body>
                {this.props.form}
            </Modal.Body>
            <Modal.Footer>
                **<div style={{display: 'flex', width: '100%', justifyContent: 'center'}}>
                    <Form.Group controlId="formFile" className="mb-3" onChange={(e) => this.handleFileUpload(e)}>
                        <Form.Control type="file" />
                    </Form.Group>
                </div>**
                <div style={{display: 'flex', width: '45%', justifyContent: 'space-between'}}>
                    <Button onClick={this.props.onSave}>Save</Button>
                    <Button onClick={this.props.onHide}>Delete</Button>
                    <Button onClick={this.props.onHide}>Close</Button>
                </div>
            </Modal.Footer>
        </Modal>
    }
}

export default withForm(StudentModalComponent);
```

在这里，当我把页脚分成不同的部分时，我使用了 CSS flex。这比使用引导行列要容易得多。对于文件上传输入部分的 onChange 事件，我正在调用 **handleFileUpload** 方法。它调用从 props 传递的另一个方法。现在，我将转到 StudentModalButton.tsx 文件来完成其余的实现。如果你还记得的话，我已经使用了高阶组件，并使表单完全动态化。现在，为了显示添加给学生的文件，我应该更改输入框。之后，我将调用 handleFileUpload 方法作为 StudentModalComponent 的属性，并在此实现该方法。当实现文件上传功能时，我们必须有@aws-sdk/client-s3，并在下载处理程序中采用预先签名的 URL，我们需要@aws-sdk/s3-request-presigner。下面是更新后的 package.json。

```
{
  "name": "serendib-ui",
  "version": "0.1.0",
  "private": true,
  "homepage": "https://debilla.github.io/serendib-scholarship-ui",
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
    "web-vitals": "^2.1.4",
    **"@aws-sdk/client-s3": "3.194.0",
    "@aws-sdk/s3-request-presigner": "3.194.0"**
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

现在，我们必须从 react 应用程序连接到 AWS 帐户。为此，当我们创建 S3 客户端时，我们必须为 AWS 帐户传递登录令牌。如果您不知道如何获得这些值，请参考下面的文章。我大胆地迈出了步伐。

[](/nerd-for-tech/aws-certified-solution-architect-iam-8930ce442515) [## AWS 认证解决方案架构师— IAM

### IAM 的意思是身份和访问管理服务。当我们在之前的教程中创建 AWS 帐户时，我们…

medium.com](/nerd-for-tech/aws-certified-solution-architect-iam-8930ce442515) 

现在，在做任何代码更改之前，让我们在 S3 创建一个桶。为此，去你的 AWS 控制台，搜索 S3，去那里。有一个按钮叫做创建桶。单击它，然后用默认配置创建它。

![](img/78dd505d79ddb8cd47bba6d0d123ac00.png)

S3 水桶

但是这个 bucket 不能从您的本地环境访问，因此，转到创建的 bucket，这里有一个名为 permissions 的部分。

![](img/6f9088bd47ba2f14526ae78bb2c82134.png)

S3 桶权限

转到权限，当你向下滚动，你会看到一节跨源资源共享(CORS)。点击编辑按钮，粘贴以下内容。

```
[
    {
        "AllowedHeaders": [
            "*"
        ],
        "AllowedMethods": [
            "PUT",
            "POST",
            "DELETE"
        ],
        "AllowedOrigins": [
            "*"
        ],
        "ExposeHeaders": [
            "x-amz-server-side-encryption",
            "x-amz-request-id",
            "x-amz-id-2"
        ],
        "MaxAgeSeconds": 3000
    }
]
```

好的👌现在我们的 S3 存储桶已经准备好接收文件了。让我们转到 React 代码并实现它。

```
const REGION = "us-east-1";
const s3Client = new S3Client({ region: REGION, credentials: {accessKeyId: '<YOUR KEY>', secretAccessKey: '<YOUR SECRET>'} });
```

在 Handle upload 方法中，我们所做的是，在 try catch 块中，我上传文件，上传后，我用更新的文件设置状态。

```
async handleFileUpload(file: any) {
    const bucketParams = {
        Bucket: "<YOUR BUCKET NAME>",
        Key: file['name'],
        Body: file,
    };

    try {
        const data = await s3Client.send(new PutObjectCommand(bucketParams));
        console.log(data);

        let fileArr = this.state.config[this.state.config.length - *1*].array;
        fileArr.push(file['name']);
        this.setConfigState(fileArr, this.state.config.length - *1*);
    } catch(e) {
        throw e;
    }
}
```

现在，or 文件被上传到 S3 存储桶，您可以通过 AWS 控制台中的 S3 来查看。接下来是下载上传的文件。为此，我将传递文件的名称，然后为该文件创建预签名的 URL。然后，我在另一个选项卡中打开这个链接，下载它。预先设计的网址没有什么神奇的，它是一个在规定时间内公众可以访问的网址。

```
async downloadFile(fileName: any) {
    const bucketParams = {
        Bucket: "ssdww",
        Key: fileName,
        Body: "BODY"
    };

    try {
        const command = new GetObjectCommand(bucketParams);
        const signedUrl = await getSignedUrl(s3Client, command, {
            expiresIn: *3600*,
        });

        window.open(signedUrl)
    } catch(e) {
        throw e;
    }
}
```

现在让我们看看完整的代码。对 StudentModalButton.txs 文件做了许多更改，以适应新的 S3 集成。

```
import React, { Component } from 'react';
import { Button } from 'react-bootstrap';
import StudentModalComponent from '../StudentModalComponent/StudentModalComponent';
import { S3Client, PutObjectCommand, GetObjectCommand } from "@aws-sdk/client-s3";
import { getSignedUrl } from "@aws-sdk/s3-request-presigner";

const REGION = "us-east-1";
const s3Client = new S3Client({ region: REGION, credentials: {accessKeyId: 'AKIAS3A4WDWDV4XTXSUS', secretAccessKey: '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'} });

interface StudentModalButtonComponentProps {
    studentId: string,
    detail: any
}

export default class StudentModalButton extends Component<StudentModalButtonComponentProps, { show: boolean, config: any }> {
    constructor(props: StudentModalButtonComponentProps) {
        super(props);

        let config = [];
        let arr = [...Object.keys(this.props.detail)];

        for (let i = *0*; i < arr.length; i++ ) {
            let label = arr[i];
            let value = this.props.detail[arr[i]];
            let obj;

            if (label === 'files') {
                obj = {
                    label: label,
                    array: value,
                    type: 'file',
                    onChange: (e: any) => {
                        this.downloadFile(e.target.innerText);
                    }
                }
            } else {
                obj = {
                    label: label,
                    value: value,
                    type: 'text',
                    onChange: (e: any) => {
                        this.setConfigState(e.target.value, i);
                    }
                }
            }

            config.push(obj);
        }

        this.state = {
            show: *false*,
            config: config
        };
    }

    async downloadFile(fileName: any) {
        const bucketParams = {
            Bucket: "ssdww",
            Key: fileName,
            Body: "BODY"
        };

        try {
            const command = new GetObjectCommand(bucketParams);
            const signedUrl = await getSignedUrl(s3Client, command, {
                expiresIn: *3600*,
            });

            window.open(signedUrl)
        } catch(e) {
            throw e;
        }
    }

    setConfigState(value: any, i: any) {
        let arr = [...this.state.config];
        arr[i].value = value;
        this.setState({ config: arr });
    }

    setModalShow(showState: boolean) {
        this.setState({ show: showState });
    }

    saveData() {
        console.log(this.state.config);
    }

    async handleFileUpload(file: any) {
        const bucketParams = {
            Bucket: "ssdww",
            Key: file['name'],
            Body: file,
        };

        try {
            const data = await s3Client.send(new PutObjectCommand(bucketParams));
            console.log(data);

            let fileArr = this.state.config[this.state.config.length - *1*].array;
            fileArr.push(file['name']);
            this.setConfigState(fileArr, this.state.config.length - *1*);
        } catch(e) {
            throw e;
        }
    }

    render(): React.ReactNode {
        return <>
            <Button onClick={() => this.setModalShow(*true*)}>{this.props.studentId}</Button>
            <StudentModalComponent config={this.state.config} show={this.state.show} handleFileUpload={(file: any) => this.handleFileUpload(file)} onHide={() => this.setModalShow(*false*)} onSave={() => this.saveData()} studentid = {this.props.studentId} />
        </>
    }
}
```

为此，我必须更改 StudentTableComponent，这是目前的主要数据源(我们还没有与后端集成)。

```
import { render } from '@testing-library/react';
import React from 'react';
import DataTableComponent from '../../components/DataTableComponent/DataTableComponent';
import StudentModalButton from '../../components/StudentModalButton/StudentModalButton';

const columns = [
    {
        key: 'id', name: 'ID', width: *10*,
        formatter(props: any) {
            return (
                <>
                    <StudentModalButton detail={props.row} studentId={props.row.id} />
                </>
            );
        },
    },
    { key: 'name', name: 'Name' },
    { key: 'contactNo', name: 'Contact No' },
    { key: 'email', name: 'Email' },
    { key: 'university', name: 'Univeristy' },
    { key: 'course', name: 'Course of Study' },
    { key: 'startDate', name: 'Course Start Date' },
    { key: 'endDate', name: 'Course End Date' },
    { key: 'schoolEndDate', name: 'Schol. start Date' },
    { key: 'sponsor', name: 'Sponsor Name' }
];

const rows = [
    { id: *0*, name: 'Example', contactNo: '', email: '', university: '', course: '', startDate: '', endDate: '', schoolEndDate: '', sponsor: '', files: [] },
    { id: *1*, name: 'Demo', contactNo: '', email: '', university: '', course: '', startDate: '', endDate: '', schoolEndDate: '', sponsor: '', files: [] }
];

export default function StudentTableContainer() {
    return (<DataTableComponent columns={columns} rows={rows} />);
}
```

S3 上传和下载到此结束。下一步是从后端处理这个文件上传事件。我的计划是，从 Lambda 函数捕获 S3 文件上传事件，并相应地更新 DynamoDB。直到我们实现那个快乐的编码！！！

UI 回购:[https://github.com/deBilla/serendib-scholarship-ui](https://github.com/deBilla/serendib-scholarship-ui)

后端回购:【https://github.com/deBilla/serendib-scholarship-ws 

# **奖金部分**

伙计们，这是 Github 部署指南。如果你们回去看看我们以前的教程，你可能会对我们到目前为止所做的有更多的了解。在这一节中，我将教你如何从 Github 环境中检索秘密。为了支持这一点，我们必须做一些代码更改。

```
const REGION = "us-east-1";
const s3Client = new S3Client({ region: REGION, credentials: {accessKeyId: process.env.REACT_APP_ACCESS_KEY_ID as string, secretAccessKey: process.env.REACT_APP_SECRET_ACCESS_KEY as string} });
```

现在让我们在 Github 中添加密钥。转到 Github 设置，你会在那里找到一个叫做环境的部分。转到环境并添加这样的秘密。

![](img/c6ab34fc4b345c0e3a80cfca1adaaae0.png)

然后，我们必须更改我们的构建管道文件 page_build.yml。

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
    **env:
      REACT_APP_ACCESS_KEY_ID: ${{ secrets.REACT_APP_ACCESS_KEY_ID }}
      REACT_APP_SECRET_ACCESS_KEY: ${{ secrets.REACT_APP_SECRET_ACCESS_KEY }}**

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
        *# Upload entire repository* path: 'build/'
    - name: Deploy to GitHub Pages
      id: deployment
      uses: actions/deploy-pages@v1
      env:  
        GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

现在，当您创建一个从另一个分支到主分支的拉请求时，这将触发，您将获得一个将文件保存到您的 S3 存储桶的工作应用程序。