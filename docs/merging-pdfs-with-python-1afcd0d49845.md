# 使用 Python 合并 pdf

> 原文：<https://medium.com/nerd-for-tech/merging-pdfs-with-python-1afcd0d49845?source=collection_archive---------0----------------------->

PyPDF2 库有助于用 python 合并 PDF 文件。它易于设置和使用。

安装:

```
pip install PyPDF2
```

我们有一个名为“PDFsToMerge”的文件夹，其中有两个 PDF 文件“first.pdf”和“second.pdf”。

```
$ pwd
/User/pmathew/PDFsToMerge
$ ls
first.pdf  second.pdf
```

在本教程中，我们将把它们合并成一个 PDF 文件。

让我们从终端中的 python shell 开始。

```
$ python
```

1.  *导入库*

```
>>> import PyPDF2
```

2.*宣布 pdf 的位置*

```
>>> first_pdf_file_location = 'User/pmathew/PDFsToMerge/first.pdf'
>>> second_pdf_file_location = 'User/pmathew/PDFsToMerge/second.pdf'
```

3.*打开文件*

函数 *open* 接受两个参数，即*文件位置*和*模式。*我们在这里指定模式为“rb ”,其中 **r** 以读取模式打开文件，而 **b** 代表二进制。

```
>>> first_pdf_file_descriptor = open(first_pdf_file_location, 'rb')
>>> second_pdf_file_descriptor = open(second_pdf_file_location, 'rb')
```

4.*创建 PdfFileReader 类的实例来读取 pfd 页面。* PdfFileReader *的构造函数将*文件描述符*作为输入参数*。

```
>>> first_pdf = PyPDF2.PdfFileReader(first_pdf_file_descriptor)
>>> second_pdf = PyPDF2.PdfFileReader(second_pdf_file_descriptor)
```

5.*合并 pdf 文件*

```
>>> # Create a new pdf instance
>>> merged_pdf = PyPDF2.PdfFileWriter()
>>> 
>>> # Add the pages from the first pdf to the new pdf
>>> for page in first_pdf.pages:
>>> ... merged_pdf.addPage(page)
>>>
>>> # Add the pages from the second pdf to the new pdf
>>> for page in second_pdf.pages:
>>> ... merged_pdf.addPage(page)
```

6.*将合并的 PDF 保存到文件*

```
>>> merged_pdf_location = 'User/pmathew/merged.pdf'
>>> merged_pdf_file_descriptor = open(merged_pdf_location, 'wb')
>>> merged_pdf.write(merged_pdf_file_descriptor)
```

7.*关闭打开的文件，退出外壳*

```
>>> first_pdf_file_descriptor.close()
>>> second_pdf_file_descriptor.close()
>>> merged_pdf_file_descriptor.close()
>>> exit()
```

> 似乎是一个漫长的过程。想走捷径吗？

尝试使用同一个库中的 **PdfFileMerger** 。

延伸阅读:

 [## PyPDF2 文档- PyPDF2 1.26.0 文档

pythonhosted.org](https://pythonhosted.org/PyPDF2/) 

参考资料:

[](https://pypi.org/project/PyPDF2/) [## PyPDF2

### 作为 PDF 工具包构建的纯 Python 库。它能够:提取文档信息(标题，作者，...)…

pypi.org](https://pypi.org/project/PyPDF2/) [](https://www.geeksforgeeks.org/working-with-pdf-files-in-python/) [## 在 Python - GeeksforGeeks 中使用 PDF 文件

### 大家一定都很熟悉 pdf 是什么。事实上，它们是最重要和最广泛使用的数字…

www.geeksforgeeks.org](https://www.geeksforgeeks.org/working-with-pdf-files-in-python/)  [## 用 Python-PythonForBeginners.com 读写文件

### 概述使用 Python 时，不需要为了读写文件而导入库。这是…

www.pythonforbeginners.com](https://www.pythonforbeginners.com/files/reading-and-writing-files-in-python)