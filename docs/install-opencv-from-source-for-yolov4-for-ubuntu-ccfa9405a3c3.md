# 从 YOLOv4 for Ubuntu 的源代码安装 OpenCV

> 原文：<https://medium.com/nerd-for-tech/install-opencv-from-source-for-yolov4-for-ubuntu-ccfa9405a3c3?source=collection_archive---------4----------------------->

YOLOv4 有几个要求，其中一个用于训练自定义对象检测器的原语是 OpenCV，它必须从源代码安装，而不是通过 pip。

要了解更多关于 YOLOv4 的要求，请点击这里查看官方知识库[。](https://github.com/AlexeyAB/darknet)

让我们开始吧。

> 官方给出的要求是 OpenCV >= 2.4。

1.  使用以下命令安装 Cmake 和 gcc 编译器以构建 OpenCV

```
sudo apt-get install cmake
sudo apt-get install gcc g++
```

2.OpenCV 需要 Numpy 作为依赖项，可以通过命令为 python3 下载

```
sudo apt-get install python3-dev python3-numpy
```

python2 的下载请参考 OpenCV 官方文档[这里](https://docs.opencv.org/master/d2/de6/tutorial_py_setup_in_ubuntu.html)。

3.使用以下命令下载 OpenCV

```
git clone https://github.com/opencv/opencv.git
```

4.在克隆 OpenCV 库之后，打开一个终端到文件夹中，或者直接打开 cd 到文件夹中。然后我们需要使用命令构建 OpenCV

```
mkdir build
cd build
cmake ../
make
```

这将需要一些时间，具体取决于您的机器。

5.最后，我们需要使用命令从我们的构建中安装 OpenCV

```
sudo make install
```

就是这样。要检查 OpenCV 是否安装正确，请输入以下命令

```
opencv_version
```

这将弹出 OpenCV 的版本，如下所示

```
4.5.1-dev
```

就这样，我们完成了 OpenCV 的正确安装，使用 YOLOv4 构建了我们的自定义对象检测器。

# **如果在制作过程中出现错误**

有时，即使在安装之后，也会出现一个错误，指出在构建 darknet 期间，在 pkg-config 中没有找到 opencv。如果出现这种情况，请输入命令

```
apt-file search opencv.pc
sudo apt-get install -y libopencv-dev
pkg-config --cflags opencv
pkg-config --libs opencv
```

这应该会更改 pkg-config 并解决错误。

希望这是有益的。

如有疑问，请随时联系我[这里](https://www.linkedin.com/in/adnan-karol-aa1666179/)和[这里](/@adnanmushtaq5)获取更多我的文章。