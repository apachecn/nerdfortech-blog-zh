# DevOps 项目

> 原文：<https://medium.com/nerd-for-tech/devops-project-b0e6fa278b3a?source=collection_archive---------14----------------------->

# 创建多级管道并使用构建管道插件进行查看

项目链接= >【https://github.com/AbhishekYuvi/SampleWebApp 

第一步

在你的系统上安装 Git，Jenkins，Tomcat，Ant。

第二步

*   在 c 盘创建“gitworkspace”文件夹

![](img/5bc454f42a7bbf505ca1c39b835f566c.png)

*   在 gitworkspace 中，从 github 克隆存储库

![](img/4004f900b4ff7d4823146d02043b91f8.png)

*   使用“dir”进行检查

![](img/8639974907ddc58649ef0c903f85a4b8.png)

第三步

创建一个提取代码的作业

*   管理 Jenkins->管理插件-> github->无需重启安装。

![](img/ed97c36b45f5f52169f1520b0c8c16e5.png)

*   新工作-> github 拉->自由式项目->确定

![](img/24cade8ff24f20142973aadd21147850.png)

*   Github pull->配置->常规->高级->使用自定义工作区

`${JENKINS_HOME}/workspace/samplewebapp`

![](img/b8ee936911a13e7353dfe2c81d8a3aa8.png)

*   SCM-> git->复制 github repo url 并粘贴

![](img/890dcce6e3f78aba77de8cbd96c0d27d.png)

第四步

持续构建->构建、代码评审和发布结果

*   管理 Jenkins->管理插件-> Ant 和警告下一代插件。

![](img/8b6230e98984a9095223fd8c4eca8316.png)![](img/faacdbac7cb95c680bbc218e1708aa33.png)

*   新项目->构建和代码审查->自由格式项目->确定

![](img/e839906b60b161f63ddde54cef538ab5.png)

*   常规->高级->使用自定义工作空间

`${JENKINS_HOME}/workspace/samplewebapp`

![](img/c26c3de2bad0372f8f7f7a7ff2842f9d.png)

*   SCM-> git->从 github 粘贴 Samplewebapp 的 URL

![](img/98e230a9df749f73178856c4974df5d4.png)

*   构建->调用 ant

![](img/a71043a9e4997e2fffa48350e8b19e2a.png)

*   后期生成操作->发布结果分析-> checkstyle_error.xml->保存

![](img/596aeafb52901908b1691ca07e4721c9.png)

第五步

持续测试——运行测试并发布 JUNIT 测试报告

*   安装 Junit 实时测试报告程序插件

![](img/36bc9363878a3960314f57b2c7302a81.png)

*   新项目->单元测试->自由式项目->确定

![](img/8742d51a0fa0f6c33d37d0b07f82510a.png)

*   常规->高级->使用自定义工作空间

`${JENKINS_HOME}/workspace/samplewebapp`

![](img/7c7230cf870e646019aed8a1d389e5ed.png)

*   SCM-> git->从 github 粘贴 URL

![](img/c162ec39da71e7cd5cdc41b4ca868190.png)

*   构建->调用 Ant

![](img/74e9ea04dde9f1e66b15b5e9a8480d37.png)

*   后期构建操作->发布 Junit 测试结果->测试计算器->保存

![](img/6fe8f4127428ca07037294337c80755d.png)

第六步

设置新的部署服务器

转到 Tomcat/conf/server.xml(在您的系统上)

连接器端口“8090”

![](img/25dc60c1de726b902587a88552dfa4d7.png)

*   转到 Tomcat/conf/tomcat-users.xml

*<user . name = " admin " password = " admin " roles = " manager-GUI，manager-script，admin"/ >*

![](img/7b92d5375dbe451b0db48470de607c3e.png)

第七步

持续部署将代码部署到生产环境中

*   安装“部署到容器”插件

![](img/357ec92831ac445e7f655585385ff927.png)

*   新建项目->部署->自由格式项目->确定

![](img/75fe1f144ceb66dca960739ef74831e5.png)

*   常规->高级->使用自定义工作空间

`${JENKINS_HOME}/workspace/samplewebapp`

![](img/7c5932941b803d8de55b0b0fa1e4e8d1.png)

*   SCM-> git->从 github 粘贴 URL

![](img/659e35b7a21ca79a6dbbcee1e5c0b266.png)

*   构建->调用 Ant，目标- WAR

![](img/2f477faff93778d34aa316499cb25958.png)

*   后期生成操作->将 war/ear 部署到容器(war/ear 文件)

添加容器-> Tomcat 9.0

网址:-[https://localhost:8090](https://localhost:8090)->保存

![](img/f376c898f7a3610f586bde633638c046.png)

停止并启动 Tomcat 9。exe 文件

![](img/10c0f0cd0593819c160c7c79f5ad3742.png)

第八步

*   安装“构建管道”插件

![](img/f897c4831328f2ecfb2b40640eebd181.png)

*   转到 Githubpull->配置->后期生成操作-生成其他项目-> BuildandCodeReview->保存

![](img/ded6288953615535d8706f8619c067be.png)

*   转到 buildanddereview->配置->后期生成操作->生成其他项目->单元测试->保存

![](img/0346850c7ea53076daa2b62d2934a4f8.png)

*   转到单元测试->配置->后期生成操作-生成其他项目->部署->保存

![](img/f7bbb4374de7f31019398478f79fbd5b.png)

*   点击“+”->完成管道->构建管道视图->确定

![](img/f1e7491ae482477dd32a729c0edfdbd1.png)![](img/e153bb8e1ebe354f6afc476af14849cd.png)

*   选择初始作业-> Githubpull->确定

![](img/e30018ddcc8d30240fe6ad1f68dd82cb.png)

*   转到 Githubpull->配置->轮询 SCM-> * * * * *

![](img/d9c66ee42899745788e1ae8271f28500.png)

*   您将被重定向到您的管道视图。

绿色:执行完毕，蓝色:尚未开始，黄色:正在执行，红色:失败

![](img/13766c4ca8d0e0a416c6fad9e661f0aa.png)![](img/ec052d27a9e66fe223422908c2e76f0d.png)