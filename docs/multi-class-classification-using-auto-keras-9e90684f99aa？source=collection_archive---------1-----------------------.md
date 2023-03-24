# 使用自动分类器的多类分类

> 原文：<https://medium.com/nerd-for-tech/multi-class-classification-using-auto-keras-9e90684f99aa?source=collection_archive---------1----------------------->

![](img/80a833b51e32dfc0d14c2e84d3b611c9.png)

在本文中，我们将讨论如何使用 Auto Keras 来解决多类分类机器学习问题。这里的问题陈述是根据手机功能预测手机的价格范围。这反映了一个真实的世界问题，即一个企业想要以相对于具有类似产品特征的其他产品有竞争力的价格推销其产品。这里给出的解决方案通过使用 Auto Keras 库保持简单。这里用于问题的数据是从 Kaggle 获得的(见参考文献)。开始时，我们将打开 Jupyter 笔记本，接下来我们将安装并导入 auto Keras 库以及 tensorflow 库，这些库将用于获取模型以进行训练，并用于以后的预测。

![](img/8337e3c1efe520347468e1c8c5b7e9e4.png)

```
***!pip install autokera
import tensorflow as tf
import autokeras as ak***
```

在那之后，我们将进口熊猫和 numpy 图书馆。这些基本库将分别用于数据帧的操作和数学函数的使用。我们将利用的其他库很少，这些将在后面解释。

```
***import numpy as np
import pandas as pd
import matplotlib.pyplot as plt***
```

接下来，我们将导入数据集。为了简单起见，从 Kaggle 下载数据集，将其保存在本地机器中，在执行命令时运行下面给定的命令(用本地驱动器中的文件位置替换“”下的文本)。这些文件将从本地驱动器加载到 Jupyter 笔记本中。

```
***df_train = pd.read_csv('C://mobile-price-classification//train.csv')
df_test = pd.read_csv('C://mobile-price-classification//test.csv', index_col= 'id')***
```

上传的文件为 CSV 格式。要从这个 CSV 文件创建一个名为 df_train 的数据帧，请在单元中执行以下命令。当我们使用命令 df.head()时，显示新创建的数据帧用于分析。为了分析数据集，我们使用命令 df.info()和 df.describe()。由于数据已经清理完毕，我们将跳过这里的数据清理步骤。

```
***df_train.head()
df_train.info()
df_train.describe()***
```

接下来，我们将分离功能(电话功能)和标签(具体价格范围-这里分为一、二、三等。).df 数据框现在分别用于创建 X(要素)和 y(实际值)数据集。

```
***X= df_train.iloc[:,:-1]
y= df_train.iloc[:,-1:]***
```

为了继续进行，我们将把数据集(X 和 y)分成训练集和验证集。这些将分别用于训练模型，然后在验证数据集上验证模型的有效性。sklearn 库有一个 train_test_split()函数，用于帮助拆分数据集。

```
***from sklearn.model_selection import train_test_split
train_X, Val_X, train_y, Val_y = train_test_split(X,y, test_size= 0.25, random_state=14)***
```

若要继续，请使用 Auto Keras 库导入 Auto Keras 分类算法。Auto-Keras 是用于自动机器学习的开源软件库。它是由**德克萨斯 A & M 大学**的**数据实验室**和**社区贡献者**开发的。Auto-Keras 提供了自动搜索深度学习模型的架构和超参数的功能，从而增强了机器学习采用的自动化和便利化。Keras 型号 ***ak。StructuredDataClassifier()****是*导入、训练的，训练完成后模型会根据精度自动选择最合适的模型。在当前情况下，模型达到 0.87%的精度，这是相当好的预测。

```
***clf = ak.StructuredDataClassifier(overwrite=True, max_trials=3) 
clf.fit(train_X, train_y, epochs=10)
predict_Val_y= clf.predict(Val_X)
predict_Val_y=predict_Val_y.astype(int)***
```

然后，通过传递验证数据集(Val_X)，该模型用于进行预测(predict_Val_y)。接下来，我们将使用 **sklearn.metrics** 库**到**检查预测验证值(predicted_Val_y)与实际值(Val_y)的差异，从而获得分类报告。接下来是打印最佳模型的摘要——它给出了最佳的精确度。

```
***from sklearn.metrics import classification_report
print(classification_report(Val_y,predict_Val_y))******model = clf.export_model()
model.summary()***
```

为了使事情更简单，我们将创建一个新的数据帧，命名为**结果**。它将由预测值(预测值)和实际值(值)组成。最后，我们将使用 pickle 文件保存模型，以便将来可以加载模型以供使用。

```
***Result=pd.DataFrame(predict_Val_y,index= Val_X.index, columns=['Predicted'])
Result['Price']= Val_y
Result.to_csv('Predicted_Price.csv')******import pickle
Pkl_Filename = "ClF.pkl"  
with open(Pkl_Filename, 'wb') as file:  
    pickle.dump(clf, file)***
```

**参考文献**

1.  [https://www . ka ggle . com/datasets/iabhishekofficial/mobile-price-class ification](https://www.kaggle.com/datasets/iabhishekofficial/mobile-price-classification)数据集
2.  https://autokeras.com/汽车官方网站
3.  [https://www . kaggle . com/code/rajeevbhadola/automl-using-auto-keras](https://www.kaggle.com/code/rajeevbhadola/automl-using-auto-keras)用于活动笔记本，代码在 ka ggle 中

![](img/80a833b51e32dfc0d14c2e84d3b611c9.png)