# 向 Office 365 添加新域

> 原文：<https://medium.com/nerd-for-tech/add-domain-to-office-365-865ec65943af?source=collection_archive---------2----------------------->

![](img/d57569a57bbc357354dbb4d7050ce8f8.png)

Office 365 是微软广受欢迎的生产力套件 Microsoft Office 的基于云的订阅模型版本。Office 365 包含与传统版本 Office 相同的核心应用程序，包括 Word、Excel、PowerPoint、Outlook、OneNote，根据购买的计划，还可能包括其他应用程序和服务，如 Publisher、Planner、OneDrive、Exchange、SharePoint、Access、Skype、Yammer、Intune 和 Microsoft Teams。

许可 Office 365 时有许多不同的软件包可用，包括供个人使用、多用户家庭、学生、企业、非营利组织和教育机构使用的版本。当 Office 365 用于商业目的时，可以添加一个域并拥有定制的电子邮件，例如如果我们添加域**"*【maneeshatest 39 . tk "***可以拥有一个定制的电子邮件，如***" cl0ud @ maneeshatest 39 . tk "***这种基于组织域的定制电子邮件在每个组织中都使用，以通过 Office 365 提供的服务来管理他们的电子邮件。

## **创建商业账户**

进入 [Office 365](https://www.office.com/) 在右上角有一个购买 Office 的按钮，点击它可以看到所有的订阅套餐，点击商务选择一个套餐。这些软件包可以使用一个月的试用期。

![](img/c57e98af5b7fe92d5a1a24f86505052f.png)![](img/de42dd0a385bcb8f64b00acd65acf248.png)

> 选择试用一个月后，您将被提示进入如下页面，在这里您可以看到试用亮点和您可以享受的服务。

![](img/5da71f0786ba2873b2b38b8ed4d46cfd.png)

> 这里的第一部分是设置帐户，您需要提供您的个人 outlook 电子邮件，如果该电子邮件没有商业计划，它会要求您使用公司名称创建一个新的商业帐户。

![](img/ee19ddd1490ebd65c925de9d710417cd.png)![](img/48027829e5d59fd1158de25a8abe7b0b.png)![](img/fd30ad8e168994021009ce20e2320c1d.png)

> 点击发送验证码和验证帐户，然后新帐户创建成功，然后他们请求创建企业身份。

![](img/64fd2ed3053e687b530416e16303906e.png)![](img/ff4e129d7cb0f38843f0dd8ac3f2948b.png)

> 在这里，您必须创建您的域的管理员帐户，该帐户在这里***【cl 0 udwso 2 . on Microsoft . com】***，创建商业帐户时提供的所有主域都将带有***【on Microsoft . com】***后缀。
> 
> 创建管理员帐户，并添加付款细节，你会看到这样的消息。您的帐户已设置，您的组织的管理员用户是您之前创建的用户。

![](img/b627ceff8ed858579beba51873965a76.png)

> 点击转到设置，你会被提示到你的管理门户，在那里你可以设置你的域和其他细节，但现在，让我们跳过它。单击右下角的退出设置。

![](img/ad140e6000ddf8a1d7c96f89a18ff3cc.png)

> 退出设置后，单击用户，然后单击活动用户，以查看贵组织正在管理的用户。这是一个新的组织，因此您将只能看到您购买的许可证的管理员帐户。

![](img/b27994fbe37ad9f25dba2feb551048ba.png)

## **向 Office365 添加自定义域**

在 Office365 中，您可以添加您的组织已经拥有的自定义域，并使用该域创建用户。在 Office365 菜单中可以进入**设置**中的**域名**，可以看到你已经拥有的域名。当您创建您的企业帐户时创建的主域应该已经存在。

![](img/3b4941ab70d8ec821c4e57102c6c16f3.png)

要添加一个新的域名，首先我们需要有一个自己的域名，有很多域名提供商，让我们使用一个免费的域名，我们可以从**[**freenom**](https://www.freenom.com)**获得。****

**![](img/9f7d825e08619486908ad3bf4ae78260.png)**

**在这里检查您想要的域名，并查看可用性，大多数情况下，域名有 ***”。tk"*** 将是你可以拥有 3 个月的免费域名。**

**![](img/4e9eb33836e656424802fa958f95f0f1.png)****![](img/a3454d6e0009510319777d503cf8ccbe.png)**

> **对于域名结帐，您需要在 freenom 中创建一个帐户，因为需要更改**域名服务器，管理您购买的域名的**。**

**![](img/cf45e4b22d18f70062e1b1c463922e53.png)**

> **现在转到 Office365，点击添加域，添加你的 freenom 域。**

**![](img/943610c6e859822c89f19b4fb8eee104.png)****![](img/7c284e870bc902823e11b175e21744fd.png)**

> **输入您的 freenom 域名，然后单击使用此域。然后，它会说，以确认这个域名属于你添加一个 **TXT 记录或 MX 记录在您的 DNS。**选择添加一个 TXT 记录并继续，然后它会提供一些值添加到您的域名的 DNS 中。**

**![](img/218f4a7088af19d1c436cc217859492b.png)****![](img/7164c911bd3543eaf3eab28eb8c0d5a6.png)****![](img/fb9ad433dbc6586dbfa0595f74f657a7.png)****![](img/047b515a1e9650cb1b054eed71da52f2.png)****![](img/0b71c6ed41f3a416633f95d897e22761.png)**

**在这里，这表明您的 DNS 提供商是 freenom 和您需要添加的 TXT 记录的详细信息。将它们添加到 freenom 后，单击“验证”来验证您的域名。等待几分钟，让 DNS 将记录添加到名称服务器中，然后在 Office365 验证域中单击验证。验证后，它会要求你添加一些记录到 DNS 或给他们访问管理域。最简单的方法是把记录交给微软来管理。**

**![](img/62b9dbb21e84086d54ccf9fdff4596a0.png)****![](img/18adb94bf88cf39989376bffb164b1d3.png)****![](img/0399ba0e55c65fc5f2d3f2d89a06146c.png)**

**选择所有服务并按继续，然后微软将为您提供域名服务器指向您的域。去 *freenom* 把你的域名指向那些域名服务器，等几分钟，然后点击继续。**

**![](img/546fcea20d5b0c9f345b471d27d90425.png)****![](img/b9f1821452f3ae281fda8172b6d07622.png)****![](img/27525afc30f79b6eb12b54b7907afc3f.png)****![](img/87bfbf311744925517cec0647c2483d3.png)****![](img/07083db30776d76c19fe2a0e4480a72e.png)**

**在获取域设置完成后，您可以在“设置”中的“域”中查看您的域。它将被指定为默认域。所以请点击***【on Microsoft . com】***域并设为默认。**

**![](img/f0dcf5a4b58d4f69c422bbc2931c0374.png)****![](img/c52b7e93c4419c0c4f1523dbfa0898c8.png)**

**这样，Office365 的自定义域设置就完成了，有关 Office365 和自定义域的更多详细信息，请查看以下链接。**

**[](https://support.microsoft.com/en-us/office/connect-your-domain-to-office-365-cd74b4fa-6d34-4669-9937-ed178ac84515) [## 将您的域连接到 Office 365

### 在您设置了 Microsoft 365 并从 G Suite 中转移了您的数据后，您可以将您的域连接到 Microsoft 365…

support.microsoft.com](https://support.microsoft.com/en-us/office/connect-your-domain-to-office-365-cd74b4fa-6d34-4669-9937-ed178ac84515) [](https://docs.microsoft.com/en-us/microsoft-365/admin/setup/add-domain?view=o365-worldwide) [## 将域添加到 Microsoft 365 - Microsoft 365 admin

### 如果您没有找到您要找的内容，请查看域名常见问题。要添加、修改或删除域，您必须是全局…

docs.microsoft.com](https://docs.microsoft.com/en-us/microsoft-365/admin/setup/add-domain?view=o365-worldwide)**