# 驯服一群电子表格

> 原文：<https://medium.com/nerd-for-tech/taming-a-herd-of-spreadsheets-b8d05d06d88d?source=collection_archive---------26----------------------->

介绍使用操作系统库的生产化 ETL(提取、转换、加载)过程。

![](img/ff76db0c5ee80042e4b99025e0933fcc.png)

厌倦了将多个不同电子表格中的数据复制粘贴到一个 Excel 文档中吗？希望有一种方法可以批量自动化这一过程？不要害怕！pandas 数据框架和操作系统库的简单组合可以节省您的时间，更不用说减少手动数据输入导致的人为错误了。

# Python 操作系统模块

Python 的 [OS 库](https://docs.python.org/3/library/os.html)允许开发者以许多与使用命令行相同的方式与他们的操作系统交互。它提供了许多有用的功能来创建和删除文件夹，检索它们的内容，以及导航系统的文件目录。出于本示例的目的，我们将使用以下命令:

```
**import** **pandas** **as** **pd**
**import** **numpy** **as** **np**
**import** **os**# Generate a list of the files in a folder
os.listdir('FOLDER_NAME')# Read in an Excel sheet to a pandas dataframe
pd.read_excel('EXCEL_FILE', sheet_name = 'SHEET_NAME')# Merge pandas dataframes
df1.merge(df2, how='JOIN', on='COLUMN_NAME')# Create a new folder
os.mkdir('FOLDER_NAME')# Write dataframe to csv
merged_df.to_csv('FILE_NAME.csv')
```

在我们开始之前，需要注意的是，这些命令可能偶尔会抛出 OSError 或 FileNotFoundError，所以为了安全起见，您可能需要仔细阅读 Try/Except 语句！

# 零售数据集样本

在这个演练中，我将使用 Kaggle 上的[样本零售数据集](https://www.kaggle.com/manjeetsingh/retaildataset)中的一些电子表格。首先，将当前目录中的所有文件读入一个列表。

```
files = os.listdir('.')
```

然后，将每个 excel 文件导入到它自己的数据框架中。如果没有指定 sheetname，pandas 将导入第一张工作表。您可以通过索引(从 0 开始)或工作表名称(“工作表 1”)来引用工作表。

```
stores = pd.read_excel('stores.xlsx', sheet_name=0)
sales = pd.read_excel('sales.xlsx', sheet_name=0)
```

接下来，我们将合并“Store”列上的数据帧。与 SQL 类似，您必须指定哪种类型的联接(左联接、右联接、外联接、内联接)以及要联接的列名。

```
df_merge = sales.merge(stores, how='left', on='Store')
```

现在我们将按商店解析合并的数据，并为每个商店保存一个新的电子表格。首先，让我们定义一个函数，为每个作为参数传递的商店号返回一个数据帧。

```
def pull_store(df, store_no):
    return df[df['Store']==store_no]
```

让我们也创建一个“输出”文件夹来保存所有这些新文件。

```
os.mkdir('output')
```

现在，我们只需要遍历商店编号，保存一个新的。csv 文件复制到每个分组的输出文件夹中。

```
for store in df_merge['Store'].unique():
    store_df = pull_store(df_merge, store)
    store_df.to_csv('output/store_' + str(store) + '.csv')
```

仅此而已！这个循环应该生成 45 个新的。csv 文件放在您刚刚创建的“输出”文件夹中。干得好！

> 这些脚本可能会变得非常复杂，尤其是当每个 Excel 文件有多个工作表，或者需要将文件名映射到标准命名约定时。

下面是一个完整的 ETL 脚本示例，用于杂货店销售及其对应的 10 个城市的 PLU 代码: