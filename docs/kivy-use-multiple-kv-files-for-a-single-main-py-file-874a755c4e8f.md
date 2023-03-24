# Kivy:连接多个。kv 文件复制到一个 main.py 文件中。

> 原文：<https://medium.com/nerd-for-tech/kivy-use-multiple-kv-files-for-a-single-main-py-file-874a755c4e8f?source=collection_archive---------2----------------------->

以前的帖子是关于在不同的文件中分解类，在多个文件中也可以这样做。适用于单个 main.py 文件的 kv 文件。有两种方法可以做到这一点:

1.  创建一个包含所有。kv 文件，然后遍历 main.py 文件中的文件夹。我有四个。我的 kv_files 文件夹中的 kv 文件。

```
# ---- main.py ----- #from kivy.lang import Builder
from os import listdirkv_path = "./kv_files/"for kv in listdir(kv_path): 
    Builder.load_file(kv_path+kv) 
```

2.全部加载。kv 文件从一个单一的。kv 文件。在 first_file.kv 文件的顶部，使用以下语法加载一个。来自另一个的 kv 文件:

```
#: include kv_files/second_file.kv
```

您还可以链接。kv 文件在一起，例如，使用上述语法在 second_file.kv 中包含 third_file.kv。然后在 main.py 文件中，通过仅引用 first_file.kv 来加载所有三个文件:

```
from kivy.lang import BuilderBuilder.load_file("./kv_files/first_file.kv")
```