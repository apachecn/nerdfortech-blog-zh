# SWAP()函数

> 原文：<https://medium.com/nerd-for-tech/swap-function-1793f4acf10e?source=collection_archive---------17----------------------->

![](img/861ed732be48b8fa7a113cac10d96cf6.png)

> **交换** () **函数**用于**交换**两个数。

# 交换功能是如何在机器级别发生的？

步骤 01: 首先，你要创建一个目录。

![](img/bb24f70c8fc1ad9b559b7539604e85f1.png)

步骤 02: 创建一个交换。c 文件。

![](img/cc383fbb7c69a0bc9ae2541ae10d029e.png)

当我们描述上面的交换代码时。

首先，我们需要使用 scanf 语句接受两个数字。现在，您需要将两个变量的地址传递给函数。参数传递方案称为按指针传递。现在看一下函数定义，我们已经将两个变量的地址从主函数传递给了交换函数。所以我们需要一个变量容器来存储整型变量的地址，即:整型指针。因此，第一个数字的地址将被收集在“a”指针变量中，而第二个数字将被收集在“b”指针变量中。

**步骤 03:** 运行并编译 swap.c 代码。

![](img/b183a3c7694f8f9501125ed2dafd521d.png)![](img/7a6a522911ab5f5d00dd44c890bcbbfa.png)

**步骤 04:** 下图 5 显示了删除编译交换文件。

![](img/f4fd0e8f844fb51423437d87c9f545a8.png)

**步骤 05:** 编译 32 位程序。我的机器是 64 位机器，我已经处理了 32 位。

我用了“gcc swap . c-o swap-fno-stack-protector-32”

然后我得到一个错误“无法定位 libc6-dev-i386 包

我用代码“sudo apt-get install libc 6-dev-i386”解决了这个错误

![](img/002144425dbacc1f7f11607e8d2c2ad1.png)

**步骤 06:** 之后再次编译 32 位代码。

![](img/792902da056e1a26139db7d41c37cf12.png)

**步骤 07:** 执行带“”的代码。/swap”。

![](img/eea7682fe72b5a4867c4dbacf82a8578.png)

**步骤 08:** 用“gcc -s swap.c”组装代码，得到文件 a.out

![](img/9fb0340ededa31a2091dea3720018616.png)

**步骤 09:** 然后我用 objdump 得到了反汇编的代码。

![](img/252ecb16985a2d4d88ea1faef040317f.png)![](img/27933fcf5a816e53fc8248859477d8cf.png)

**我们来解释一下流程。**

080483b4 <swap>:</swap>

80483b4: 53 推送%ebx

80483b5: 8b 54 2408 mov 0x8(%esp)，%edx

80483b9: 8b 44 240c mov 0xc(%esp)，%eax

80483bd: 8b 0a mov (%edx)，%ecx

80483bf: 8b 18 mov (%eax)，%ebx

80483c1: 89 1a mov %ebx，(%edx)

80483c3: 89 08 mov %ecx，(%eax)

80483c5: 5b pop %ebx

80483c6: c3 ret

![](img/80589a6891964c56105b41b50bd26afd.png)![](img/ce8b2fc3fa26f37fde8ff9fbb53347d1.png)![](img/7ae0f31c702c4c4220c387a75734369f.png)![](img/7cf5663481f7aa683158472354bdc479.png)![](img/f2f70c885d368bac3492467b173fbed2.png)![](img/fa2472df8a564bb435f055153259ae84.png)![](img/c5c4718868b7332fc23b935d5480b233.png)![](img/a07cdc99af59a9e2af4b64e5802817eb.png)![](img/872fc8cc73ef85641e9437699395a2df.png)