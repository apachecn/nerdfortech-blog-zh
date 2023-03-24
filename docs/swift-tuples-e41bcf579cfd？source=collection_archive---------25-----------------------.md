# 快速元组

> 原文：<https://medium.com/nerd-for-tech/swift-tuples-e41bcf579cfd?source=collection_archive---------25----------------------->

![](img/e8b628f4aeda886909dbd43046cf718a.png)

元组是一种允许我们将具有不同数据类型的相关数据分组在一起的东西，它像简单的字典一样方便，但它给了我们保存不同数据类型的灵活性，并且它不像创建结构或类那样费力。

```
import Foundationlet touple1 = ("Nadia" , 7)print(touple1.0)//this prints Nadia________________________________________________________Even though this format is short but it's not preferred.Instead we the following format._________________________________________________________let touple2 = (name : "Nadia" , faveNum : 7)print(touple2.name)//this also prints Nadia
```

还有另一种创建元组的方法。

```
touple3: (name : String , faveNum: Int)//creates an emptytoupletouple3.name = "Nadia"touple3.faveNum = 7touple3 = (name : "Nadia" , faveNum: 7)
```

**结论**

有了元组，我们能够组织相对较小的相关数据片段，并且能够非常快速地创建这些数据片段。