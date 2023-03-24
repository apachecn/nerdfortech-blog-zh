# 用 Python 实现基于 ML 的颜色检测

> 原文：<https://medium.com/nerd-for-tech/ml-based-color-detection-with-python-3f067baa9e36?source=collection_archive---------5----------------------->

![](img/2f0916aa48d07fd5b972718205e19eb8.png)

在这篇文章中，我们将学习基于 ML 的颜色检测和一个基本的应用程序，它将帮助我们检测图像中的颜色。我们的应用程序的工作流程如下:

选择图像文件的路径>打开一个窗口选择部分>返回名称和 RGB 颜色代码

网页开发者和设计者都知道 RGB 值的重要性。因此，这将是一个简单但非常有用的计算机视觉和颜色识别的实现。现在，让我们开始吧…

对于我们的项目，我们将使用 OpenCV 和 Pandas 库。

# OpenCV

***OpenCV****(开源计算机视觉库)是一个主要针对实时计算机视觉的编程函数库。最初由英特尔开发，后来由 Willow Garage 然后是 Itseez(后来被英特尔收购)提供支持。该库是跨平台的，在开源 Apache 2 许可下可以免费使用。从 2011 年开始，OpenCV 为实时操作提供了 GPU 加速。*

简单来说，OpenCV 是一个帮助我们处理图像和视频的项目。许多流行的软件公司在他们的应用程序中使用它。希望有一个针对 python 的标准 OpenCV 库。你可以很容易地安装它与 pip 安装程序。

```
pip install opencv-python
```

它将安装最新版本的 Opencv 库。如果您的电脑中尚未安装 pip，请访问此链接[https://www . geeks forgeeks . org/how-to-install-pip-on-windows/](https://www.geeksforgeeks.org/how-to-install-pip-on-windows/)

# 熊猫

" ***pandas*** *是一款快速、强大、灵活且易于使用的开源数据分析和操纵工具，
构建在 Python 编程语言之上。*

Panda 是一个用于数据分析的开源库。这是一个在专家和专业开发人员中非常受欢迎的工具。我们要用它来分析数据集。您可以在 pip 安装程序的帮助下安装它

```
pip install pandas
```

在我们的应用程序中实现 OpenCV 库之前，最好先了解一下它的基础知识。因此，我们将使用 OpenCV 构建一个相机应用程序。首先，我们将 cv2 导入到库中，然后我们将使用它的 VideoCapture()函数来捕获视频。然后我们将使用 while 循环读取每一帧，并在默认的 OpenCV 窗口中显示它。用户必须按 Esc 键结束程序。让我们一起写。

```
#importing OpenCV
import cv2 as opencv#capturing video
capture = opencv.VideoCapture(0)while(1):#reading every frame of video
r, f = capture.read()#showing each frame in a OpenCV Window
opencv.imshow('frame',f)#defining exit key as Esc as it's code is 27
if opencv.waitKey(1) & 0xFF== 27:breakcapture.release()#destroying OpenCV windows
opencv.destroyAllWindows()
```

我认为代码通过注释就能很好地自我解释。让我们跳到实际的程序

我们需要一个数据集来识别颜色名称和十六进制代码。出于演示目的，我们将使用一个包含 1000 个示例的 csv 文件。你可以从[这里](https://github.com/satadeep3927/color-recognization/blob/master/dataset.csv)下载。

1.  导入所需的库，如 NumPy、Pandas 和 OpenCV。

```
import numpyimport pandasimport cv2 as opencv
```

2.获取图像文件位置的用户输入，并使用 OpenCV imread()函数读取它。

```
image = opencv.imread(input("Input a File location: "))
```

3.我们将非结构化数据集作为 csv 文件。让我们用熊猫 read_csv()函数让它更具可读性。为了使用它，我们必须定义一个带有列名的列表。然后我们将这个列表作为 read_csv()函数的一个参数传递。

```
#defining column names
index = ["color","color_name","hex","R","G","B"]#reading dataset file and structuring it
dataset = pandas.read_csv("dataset.csv", names=index, header=None)
```

4.现在我们要为函数准备一些全局变量。

```
clickstate = Falser=0g=0b=0position_x = 0position_y = 0
```

5.我们将创建一个函数，它接受 R，G，B 值的参数，并为这些值找到一个名称。熊猫 loc()函数将帮助我们找到颜色名称。

```
def recognizer(R,G,B):minimum = 1000for i in range(len(dataset)):d = abs(R - int(dataset.loc[i,"R"])) + abs(G - int(dataset.loc[i, "G"])) + abs(B - int(dataset.loc[i, "B"]))if(d<=minimum):minimum = dcname = dataset.loc[i, "color_name"]return cname
```

6.我们需要创建一个鼠标点击处理程序来处理用户双击图像并获得点击的位置。

```
def clickhandler(event, x, y,f,p):if event == opencv.EVENT_LBUTTONDBLCLK:*global* b,g,r,position_x,position_y, clickstateclickstate = Trueposition_x = xposition_y = yb,g,r = image[y,x]b = int(b)g = int(g)r = int(r)
```

7.我们将创建一个 OpenCV 窗口来打开图像，并将 clickhandler 函数设置为鼠标单击处理程序，它将由用户的双击触发。

```
opencv.namedWindow('Color Recognition Application')opencv.setMouseCallback('Color Recognition Application', clickhandler)
```

8.现在我们将编写一个 while 循环来检测图像的颜色。注释是自我解释的。

```
while(1):opencv.imshow("Color Recognition Application",image)if (clickstate):*#opencv.rectangle(image, startpoint, endpoint, color, thickness)-1\. It fills entire rectangle*opencv.rectangle(image,(20,20), (750,60), (b,g,r), -1)*#Creating a text string to display color name and RGB value*text = recognizer(r,g,b) + "  RGB("+str(r)+","+str(g)+","+str(b)+")"*#opencv.putText() function will write data on image*opencv.putText(image, text,(50,50),2,0.8,(255,255,255),2,opencv.LINE_AA)*#For light colors we will choose black color*if(r+g+b>=600):opencv.putText(image, text,(50,50),2,0.8,(0,0,0),2,opencv.LINE_AA)clickstate=False*#Ends the function when Esc is clicked*if opencv.waitKey(20) & 0xFF ==27:break
```

9.用 destroyAllWindows()函数关闭 OpenCV 窗口。

```
opencv.destroyAllWindows()
```

所有这些看起来像…

```
import numpyimport pandasimport cv2 as opencvimage = opencv.imread(input("Input a File location: "))index = ["color","color_name","hex","R","G","B"]dataset = pandas.read_csv("dataset.csv", names=index, header=None)clickstate = Falser=0g=0b=0position_x = 0position_y = 0def recognizer(R,G,B):minimum = 1000for i in range(len(dataset)):d = abs(R - int(dataset.loc[i,"R"])) + abs(G - int(dataset.loc[i, "G"])) + abs(B - int(dataset.loc[i, "B"]))if(d<=minimum):minimum = dcname = dataset.loc[i, "color_name"]return cnamedef clickhandler(event, x, y,f,p):if event == opencv.EVENT_LBUTTONDBLCLK:*global* b,g,r,position_x,position_y, clickstateclickstate = Trueposition_x = xposition_y = yb,g,r = image[y,x]b = int(b)g = int(g)r = int(r)opencv.namedWindow('Color Recognition Application')opencv.setMouseCallback('Color Recognition Application', clickhandler)while(1):opencv.imshow("Color Recognition Application",image)if (clickstate):*#opencv.rectangle(image, startpoint, endpoint, color, thickness)-1\. It fills entire rectangle*opencv.rectangle(image,(20,20), (750,60), (b,g,r), -1)*#Creating a text string to display color name and RGB value*text = recognizer(r,g,b) + "  RGB("+str(r)+","+str(g)+","+str(b)+")"*#opencv.putText() function will write data on image*opencv.putText(image, text,(50,50),2,0.8,(255,255,255),2,opencv.LINE_AA)*#For light colors we will choose black color*if(r+g+b>=600):opencv.putText(image, text,(50,50),2,0.8,(0,0,0),2,opencv.LINE_AA)clickstate=False*#Ends the function when Esc is clicked*if opencv.waitKey(20) & 0xFF ==27:breakopencv.destroyAllWindows()
```

现在运行程序，看看魔术。

# 结论

这篇文章描述了一个非常基本的强大的 OpenCV 和 Pandas 库的应用。我们可以对它进行研究，并用这些库创建更强大、更棒应用程序。

编码快乐！！！