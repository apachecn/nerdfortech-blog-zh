# 序列化的概念

> 原文：<https://medium.com/nerd-for-tech/concept-of-serialization-2b35c16f64d?source=collection_archive---------1----------------------->

计算机科学新手？你并不孤单。这是给你的。

任何计算机程序的真正本质都在于我们组织数据(数据结构)的能力有多强，操纵数据(算法)的能力有多强。这就是为什么大多数公司会测试应聘者在数据结构和算法方面的技能。数据结构在决定算法的整体效率方面起着关键作用。例如，一个数组插入的线性时间复杂度，而栈或队列它只有常数时间。

例如，在一段代码中，你要在一个数据结构上做很多操作，比如一个数组或者一个 hashmap。完成后，如果您想保存数据结构的内容以便进一步分析数据，该怎么办呢？想从朋友的电脑上读取数据结构怎么办？这里出现了序列化的概念。简单来说，**序列化**是一种将复杂数据结构以可读格式保存到磁盘上的方法。相反，**反序列化**只是读回之前保存的数据。

序列化的概念是各种领域中非常常见的方法。你可能已经听说过类似`JSON, XML, YAML, Protobuf`等技术。这些都是简单而强大的技术，在不同的地方使用。Google 的 Protobuf 是一种更加通用和独立于语言的技术。毕竟，最流行的 JSON 是 NoSQL 数据库程序的基础。

**下面是一个简单的例子。**

```
Array: 
A = [ 1, 2, 3] 
B = [ 4, 5, 6] 
```

为了在 JSON 中序列化这两个数组，我们可以做的是使用数组的名称作为“key ”,这个键的值就是数组的值。我们可以将它保存在一个文件中，以便将来阅读。

`JSON: { "A" : [ 1, 2, 3], "B" : [ 4, 5, 6] }`

序列化数据的真正优势在于它的可移植性。先前保存的 JSON 文件可以传输到任何地方，最终用户可以简单地反序列化它。在 JSON 反序列化期间，任何内容`[]`都作为数组元素读取，而`" "`作为变量的键。这种序列化数据可以是独立于语言的。您可以序列化 Python 代码中的数据，并从 Java 代码中读取 JSON 文件。上面提到的一些技术旨在消除语言障碍。

**我的 Boost C++序列化工作:**

当我从事序列化工作时，我是软件工程的新手。所以，我费了很大劲才把代码编译好。我所面临的一些问题包含在这篇文章的底部。

在探索了序列化的一些概念之后，我决定使用 Boost 库。Boost 的文档并不像我在 Python 上做了大量工作后所期望的那样用户友好。小心点！

与我上面解释的 JSON 格式不同，我将用 C++代码将序列化数据写入一个文本文件。如果您不想要可移植性，并防止用户直接打开文件，那么您也可以序列化为二进制文件。和往常一样，您可以通过写入 JSON 文件在 Python 中尝试这个概念，序列化的数据是高度可移植的。

无论 STL 容器(vector、map 或 unordered_map)是什么，都可以使用重载的`&`或`<< >>`操作符来实现序列化和反序列化。就这么简单！换句话说，我们可以称这些操作为“保存&加载”。

```
/* Serialization */
// Create an output archive 
std::ofstream ofs( "file.bin" ); 
boost::archive::text_oarchive ar(ofs); 
// Save the data
ar & serialized;  // where serialized is a map or array/* Deserialization */
// Read the input archive 
std::ifstream ifs( "file.bin" ); 
boost::archive::text_iarchive ar(ifs); 
// Load the data 
ar & deserialized;  // where deserialized is a map or array
```

使用二进制文件保存和加载，可以通过以下方式完成:

```
/* Serialization */
std::string file("file.bin");
std::ofstream ofs(file.c_str(), std::ios::binary);
boost::archive::binary_oarchive ar(ofs);
ar & serialized; /* Deserialization */ 
std::string file("file.bin");
std::ifstream ifs(gold_file.c_str(), std::ios::binary);
boost::archive::binary_iarchive ar(ifs);
ar & deserialized;
```

**实际问题:** 这听起来可能很天真，但老实说，我确实面临过一些编译错误。不过，有一种方法很容易解决。

*   问题#1:
    错误的头包括。在我的案例中，我使用了地图。我只需要一个头球`map.hpp`。但是我确实有很多其他的头文件，比如`unordered_map.hpp`、`map.hpp`、`boost_unordered_map.hpp`、`set.hpp`以及其他几个文件。所以，编译器搞混了，在错误的地方寻找定义。
    幸运的是，抛出错误的 boost 头文件中有一条注释，写着 remove other includes。
*   问题 2:
    我没有以正确的方式进行序列化。我的意思是，如果我有一个存储在变量中的映射，我所要做的就是序列化`ar & var;`。但是我写了一个 for 循环，一个元素一个元素的迭代和序列化(`it->first`和`it->second`)，弄得太复杂了。确实有效。但是问题出现在反序列化期间。我如何迭代来逐元素反序列化？这些 boost 模板通过反向读取文件来完成这项工作。所以`&`操作符就是我们所需要的。

经过一段时间的努力和学习，我终于成功了。我还编写了一个小的测试用例来读取现有的二进制文件，并检查它是否与当前文件匹配。