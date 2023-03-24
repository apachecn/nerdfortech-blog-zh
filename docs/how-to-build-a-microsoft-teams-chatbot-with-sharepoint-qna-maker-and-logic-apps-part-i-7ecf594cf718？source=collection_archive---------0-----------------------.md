# 如何用 Sharepoint、QnA Maker 和 Logic Apps 构建微软团队聊天机器人—第一部分

> 原文：<https://medium.com/nerd-for-tech/how-to-build-a-microsoft-teams-chatbot-with-sharepoint-qna-maker-and-logic-apps-part-i-7ecf594cf718?source=collection_archive---------0----------------------->

## 一个聊天机器人，当被询问有关 SharePoint 网站中的文件时，它返回文件信息。

![](img/7284d25b95a66f6f5f0fce4334d3bc1e.png)

# 第 1 部分:从 Sharepoint 网站获取嵌套文件夹路径和详细信息

![](img/e3b609aa5e55780e8c2058252cedbaa1.png)

逻辑应用流程

## Powershell 操作手册代码

此脚本扫描 SharePoint 文件夹及其子文件夹，并返回信息。

```
#Connect to SPO
$sitename = "[https://xxxx.sharepoint.com/sites/xxxx/](https://kuharan.sharepoint.com/sites/NewSite/)"
$username = "[xxxx@xxxx.onmicrosoft.com](mailto:kuharan@kuharan.onmicrosoft.com)"
$password = "xxxx"
$encpassword = convertto-securestring -String $password -AsPlainText -Force
$cred = new-object -typename System.Management.Automation.PSCredential -argumentlist $username, $encpassword
Connect-PnPOnline -Url $sitename -Credential $cred #Target multiple lists 
$allLists = Get-PnPList | Where-Object {$_.BaseTemplate -eq 101}#Store the results
$results = @()
foreach ($row in $allLists) {
    $allItems = Get-PnPListItem -List $row.Title -Fields "FileLeafRef"

    foreach ($item in $allItems) {
            $results += New-Object psobject -Property @{
                FullName            = $item["FileLeafRef"]
                FullPath            = "[https://xxxx.sharepoint.com](https://kuharan.sharepoint.com)"+$item["FileRef"].replace(" ","%20")
            }
    }
}#Export the results
$results | ConvertTo-Json
```

## 您也可以尝试其他领域:

```
$allItems = Get-PnPListItem -List $row.Title -Fields "FileLeafRef", "SMTotalFileStreamSize", "FileDirRef", "FolderChildCount", "ItemChildCount"

    foreach ($item in $allItems) {
            $results += New-Object psobject -Property @{
                FileType          = $item.FileSystemObjectType 
                RootFolder        = $item["FileDirRef"] 
                LibraryName       = $row.Title
                FullName            = $item["FileLeafRef"]
                FullPath            = "[https://xxxx.sharepoint.com](https://kuharan.sharepoint.com)"+$item["FileRef"].replace(" ","%20")
                FolderSizeInMB    = ($item["SMTotalFileStreamSize"] / 1MB).ToString("N")
                NbOfNestedFolders = $item["FolderChildCount"]
                NbOfFiles         = $item["ItemChildCount"]
            }
    }
```

为了运行这个 PowerShell 脚本，我必须在 automation 帐户中安装这两个模块。

![](img/26bd4f66276186558c425827567e9b43.png)![](img/efa739eae36aa1abd9bc67f896d82297.png)

创造一份工作

![](img/4e98e535f02eee3d75cb310869962208.png)

获取作业输出

## 解析 JSON

![](img/061a43bf741e9c30abbc3edb588fd0fb.png)

解析 JSON

下面是活动的输入模式

```
{
    "items": {
        "properties": {
            "FullName": {
                "type": "string"
            },
            "FullPath": {
                "type": "string"
            }
        },
        "required": [
            "FullName",
            "FullPath"
        ],
        "type": "object"
    },
    "type": "array"
}
```

## 创建 CSV

创建 CSV 表。

![](img/b3556b86995621510877eb970bd43b8b.png)

创建 CSV 表

## 皈依 TSV 教

为什么？ *QnA 制造商要求*

![](img/25cb1e578cd7b000182075b44f809101.png)

用制表符替换逗号

## 动态表达式代码

```
replace(body('Create_CSV_table_2'),',','    ')
```

## 创建一个斑点

![](img/fe13c57a39d8115c3bf0975fd11115f1.png)

存储帐户

当它运行时，它是绿色的:)

![](img/81356cc516c26fe3a36980f75a5afcf3.png)

逻辑应用输出

接下来，

# 第 2 部分:QnA maker 服务和聊天机器人

# 第 3 部分:团队部署

# 第 4 部分:实现自动化！

敬请期待！