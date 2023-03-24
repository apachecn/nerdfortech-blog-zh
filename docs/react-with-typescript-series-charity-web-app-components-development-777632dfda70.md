# React with Typescript 系列(慈善网络应用)-组件开发

> 原文：<https://medium.com/nerd-for-tech/react-with-typescript-series-charity-web-app-components-development-777632dfda70?source=collection_archive---------2----------------------->

嗨，伙计们，在这个 React with typescript 系列中，到目前为止，我们已经创建了初始应用程序，然后在上一个教程中，我们为 UI 创建了一些线框设计。

[](https://billa-code.medium.com/react-basic-series-charity-web-app-planning-the-ui-99da0a3a4dcc) [## React 基础系列(慈善网络应用)-规划用户界面

### 大家好，在上一个教程中，我们已经了解了如何用 typescript 创建一个新的 React 项目。在这个…

billa-code.medium.com](https://billa-code.medium.com/react-basic-series-charity-web-app-planning-the-ui-99da0a3a4dcc) [](https://towardsdev.com/react-basic-series-charity-web-app-creating-a-react-app-with-typescript-32154afb3f19) [## React with Typescript 系列(慈善网站应用程序)-使用 Typescript 创建 React 应用程序

### 大家好，我最近开始了一个简单的项目，为一个慈善组织创建一个 web 应用程序。他们所做的是，他们…

towardsdev.com](https://towardsdev.com/react-basic-series-charity-web-app-creating-a-react-app-with-typescript-32154afb3f19) 

在本教程中，我们将看到如何通过创建可重用组件来实现这些设计。

因此，正如我们在上一个教程中讨论的，我们必须创建一个 DataTableComponent，然后为学生和赞助商的详细信息创建两个模态组件。除此之外，我们还需要一个导航栏。第一步，我们将创建 DataTableComponent。

要创建我正在使用的 react-data-grid 库的 DataTableComponent，请使用“npm i react-grid”命令安装它。这个库有很多很酷的特性，而且很容易上手。我将在 src/components/datatable component 文件夹中创建一个名为 DataTableComponent.tsx 的新文件，如下所示。

```
import React, { Component } from 'react';
import DataGrid from 'react-data-grid';

interface DataTableProps {
    columns: columnType[],
    rows: rowType[]
}

type columnType = {
    key: string;
    name: string;
};

type rowType = {
    id: number;
    name: string;
};

export default class DataTableComponent extends Component<DataTableProps> {
    constructor(props: DataTableProps) {
        super(props);
    }

    render(): React.ReactNode {
        return <DataGrid style={{height: '100vh'}} columns={this.props.columns} rows={this.props.rows} />;
    }
}
```

现在让我们试着理解我们在这里写的东西。因此，DataTableComponent 是通过扩展 React Component 创建的，这使我们能够访问构造函数和所有其他生命周期方法。如果我们把它作为函数来做，那么我们可以使用钩子来做我们的工作，但是在这个特别的教程系列中，我只使用组件(主要是因为像这样的教程比较少)。

因为我们使用 typescript，所以我们必须为代码中使用的每个变量定义类型。所以我定义了 columnType 和 rowType 的类型，然后定义了一个接口作为组件的 props 类型。相反，我可以在我们的代码中使用“any”类型，但这更专业、更正确。所以表组件是可以的。现在让我们编写一个单元测试来检查它是否可以渲染。我将在同一个文件夹中创建另一个名为 DataTableComponent.test.tsx 的文件，如下所示。

```
import { render } from "@testing-library/react";
import React from "react";
import { describe, it, expect } from "@jest/globals";
import DataTableComponent from "./DataTableComponent";

describe("Card", () => {
  it("renders", () => {
    const wrapper = render(<DataTableComponent columns={[]} rows={[]} />);
    expect(wrapper.container).toMatchSnapshot();
  });
});
```

在这里，我们渲染组件并检查它自动创建的快照是否与当前实现相同。这是非常有价值的，当我们把后来的变化添加到应用程序中时，因为我们总是可以检测到是否有什么变化。

现在让我们为学生详细信息创建一个模态弹出窗口。在此之前，我们必须安装 react-boostrap，在命令行中键入“npm i react-bootstrap”。我们将实现这种方式，用户可以从表行中的按钮打开弹出窗口。首先让我们对 StudentModalComponent 进行编码。我将在 src/components/StudentModalComponent 文件夹中创建这个文件，如下所示。

```
import React, { Component } from 'react';
import { Modal, Button } from 'react-bootstrap';

interface ModalPropType {
    show: boolean,
    studentid: string,
    onHide(): void
}

export default class StudentModalComponent extends Component<ModalPropType> {
    constructor(props: ModalPropType) {
        super(props);
    }

    render(): React.ReactNode {
        return <Modal {...this.props}>
            <Modal.Header closeButton>
                <Modal.Title id="contained-modal-title-vcenter">
                    Details of Student {this.props.studentId}
                </Modal.Title>
            </Modal.Header>
            <Modal.Body>

            </Modal.Body>
            <Modal.Footer>
                <Button onClick={this.props.onHide}>Save</Button>
                <Button onClick={this.props.onHide}>Delete</Button>
                <Button onClick={this.props.onHide}>Close</Button>
            </Modal.Footer>
        </Modal>
    }
}
```

现在，如果我们回顾我在这里所做的，我只是使用 react-boostrap 库创建一个模型。所以你应该想知道所有的显示和隐藏是如何工作的。我们将使用下一个组件的 react 状态来处理它，处理它的方法将通过一个 prop 发送。这就是为什么我们有 onHide()和 show 作为我们的道具。道具“show”将用于此行{…this.props}的模态组件中。“onhide()”将用于关闭模式。现在让我们也为此编写单元测试。

```
import { render } from "@testing-library/react";
import React from "react";
import { describe, it, expect } from "@jest/globals";
import StudentModalComponent from "./StudentModalComponent";

describe("StudentModalComponent", () => {
  it("renders", () => {
    const wrapper = render(<StudentModalComponent show={*true*} studentid={"1"} onHide={jest.fn()} />);
    expect(wrapper.container).toMatchSnapshot();
  });

  it("not renders", () => {
    const wrapper = render(<StudentModalComponent show={*false*} studentid={"1"} onHide={jest.fn()} />);
    expect(wrapper.container).toMatchSnapshot();
  });
});
```

现在让我们创建按钮组件来处理学生模式的显示隐藏。我将在 src/components/StudentModalButton 文件夹中创建这个文件。

```
import React, { Component } from 'react';
import { Button } from 'react-bootstrap';
import StudentModalComponent from '../StudentModalComponent/StudentModalComponent';

interface StudentModalButtonComponentProps {
    studentId: string
}

export default class StudentModalButton extends Component<StudentModalButtonComponentProps, { show: boolean }> {
    constructor(props: StudentModalButtonComponentProps) {
        super(props);

        this.state = {
            show: *false* };
    }

    setModalShow(showState: boolean) {
        this.setState({ show: showState });
    }

    render(): React.ReactNode {
        return <>
            <Button onClick={() => this.setModalShow(*true*)}>{this.props.studentId}</Button>
            <StudentModalComponent show={this.state.show} onHide={() => this.setModalShow(*false*)} studentid = {this.props.studentId} />
        </>
    }
}
```

在这里，我们所做的是保持一个名为 show 的状态，并将其发送给 StudentModalComponent 以显示和隐藏它。现在让我们也为此编写单元测试。

```
import { render } from "@testing-library/react";
import React from "react";
import { describe, it, expect } from "@jest/globals";
import StudentModalButton from "./StudentModalButton";

describe("Card", () => {
  it("renders", () => {
    const wrapper = render(<StudentModalButton studentId={"1"} />);
    expect(wrapper.container).toMatchSnapshot();
  });
});
```

现在，我们将通过使用上面创建的 DataTableComponent 和 ModalButton 组件，使用该组件创建 StudentTableComponent。为此，我将在 src/components/StudentTableComponent 中创建一个名为 StudentTableComponent.tsx 的文件，如下所示。

```
import React, { Component } from 'react';
import DataTableComponent from '../../components/DataTableComponent/DataTableComponent';
import ModalButtonComponent from '../../components/ModalButtonCompnent';

const columns = [
    {
        key: 'id', name: 'ID', width: *10*,
        formatter(props: any) {
            return (
                <>
                    <ModalButtonComponent studentId={props.row.id} />
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
    { key: 'scholEndDate', name: 'Schol. start Date' },
    { key: 'sponsor', name: 'Sponsor Name' }
];

const rows = [
    { id: *0*, name: 'Example' },
    { id: *1*, name: 'Demo' }
];

export default class StudentTableComponent extends Component {
    render(): React.ReactNode {
        return <DataTableComponent columns={columns} rows={rows} />;
    }
}
```

在这里，我已经为行使用了一些虚拟数据，但在我们实现后端后，这些将被更改为从后端获取数据。现在我们也可以测试这个组件了，让我们编写 StudentTableComponent.test.tsx 文件。

```
import { render } from "@testing-library/react";
import React from "react";
import { describe, it, expect } from "@jest/globals";
import StudentTableComponent from "./StudentTableComponent";

describe("StudentTableComponent", () => {
  it("renders", () => {
    const wrapper = render(<StudentTableComponent />);
    expect(wrapper.container).toMatchSnapshot();
  });
});
```

现在，我们几乎已经完成了开发学生组件和与之相关的单元测试。现在，让我们在 src/containers/landing container 文件夹中创建 LandingContainer.tsx。

```
import React, { Component } from 'react';
import StudentTableComponent from '../../components/StudentTableComponent/StudentTableComponent';
import { Navbar, Container, Nav, Button, Tab, Col, Row } from 'react-bootstrap';

interface LandingContainerProp { }

const btn = { backgroundColor: '#212529' };

export default class LandingContainer extends Component<LandingContainerProp> {
    constructor(props: LandingContainerProp) {
        super(props);
    }

    render(): React.ReactNode {
        return <>
            <Navbar bg="dark" variant="dark">
                <Container fluid>
                    <Navbar.Brand>Serendib Foundation</Navbar.Brand>
                </Container>
            </Navbar>
            <Container style={btn} fluid>
                <Tab.Container id="left-tabs-example" defaultActiveKey="first">
                    <Row style={btn}>
                        <Col sm={*1*}>
                            <Nav variant="pills" className="flex-column">
                                <Nav.Item>
                                    <Nav.Link eventKey="first">Student</Nav.Link>
                                </Nav.Item>
                                <Nav.Item>
                                    <Nav.Link eventKey="second">Sponsors</Nav.Link>
                                </Nav.Item>
                            </Nav>
                        </Col>
                        <Col sm={*11*}>
                            <Tab.Content>
                                <Tab.Pane eventKey="first">
                                    <StudentTableComponent />
                                </Tab.Pane>
                                <Tab.Pane eventKey="second">
                                    <StudentTableComponent />
                                </Tab.Pane>
                            </Tab.Content>
                        </Col>
                    </Row>
                </Tab.Container>
            </Container>
        </>
    }
}
```

这段代码将创建一个很酷的引导导航组件，以给出我们在上一个教程中计划的精确外观。之后，我们将 StudentTableComponent 包含在其中。现在让我们为此编写单元测试。

```
import { render } from "@testing-library/react";
import React from "react";
import { describe, it, expect } from "@jest/globals";
import LandingContainer from "./LandingContainer";

describe("LandingContainer", () => {
  it("renders", () => {
    const wrapper = render(<LandingContainer />);
    expect(wrapper.container).toMatchSnapshot();
  });
});
```

现在我们要做的是像这样把这个容器添加到 App.tsx 文件中。

```
import LandingContainer from "./containers/LandingContainer/LandingContainer";

export default function App() {
  return (
    <LandingContainer />
  );
}
```

并更新 App.test.tsx 文件

```
import React from 'react';
import { render, screen } from '@testing-library/react';
import App from './App';

test('renders learn react link', () => {
  render(<App />);
  const linkElement = screen.getByText(/Serendib Foundation/i);
  expect(linkElement).toBeInTheDocument();
});
```

对于这一步，现在一切都好了。我们已经有了我们设计的工作原型。让我们在下一个教程中见面，学习模式有趣的概念。要创建可赞助的相关组件，请尝试与我们现在相同的方法。快乐编码:D！！！！

注意:代码在这里[https://github.com/deBilla/serendib-scholarship-ui](https://github.com/deBilla/serendib-scholarship-ui)