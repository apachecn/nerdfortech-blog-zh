# 对称加密:JavaScipt 中的流密码

> 原文：<https://medium.com/nerd-for-tech/symmetric-cryptography-stream-ciphers-in-javascipt-fcda0c21f3a6?source=collection_archive---------21----------------------->

![](img/85c817c7df60a68300a694c752e7c83d.png)

数字钥匙

对称密码术，特别是加密，要求用相同的密钥对消息进行加密和解密。在这个保护伞下，我们发现流和分组密码。流密码一次处理一位，而块密码处理一组位。关于对称密码以及流密码和分组密码，已经有很多著述。本文是用现代 JavaScript 实现流密码的一个练习。重点是实现一个简单的密码和代码的可读性——不是速度也不是秘密。值得一提的是:如果您需要应用程序内部的安全性，请使用一个众所周知且经过测试的算法。然而，我鼓励你自己去试验和实现这些密码，这是一个很好的练习。

该算法的基础是 XOR，这是一种布尔运算符，当被比较的两个位不同时返回真。它的特点是

```
(a XOR b) XOR a == b
```

这是很重要的，因为同一个密钥(a)可以应用一次来加密，然后再应用一次来解密。这是带反转的 XOR(右部)的真值表(左部是真值表)。你可以把`p`想成普通位，`k`想成密钥位，`c`想成密码位。

```
p k | c  ->  c k | p
﻿---------------------
1 1 | 0  ->  0 1 | 1
﻿1 0 | 1  ->  1 0 | 1
0 1 | 1  ->  1 1 | 0
0 0 | 0  ->  1 0 | 0
```

这种方法的关键(双关语)是密钥的大小(位数)和密钥的随机性(随机性是一个研究得很好的领域)。幸运的是，随机性不会影响密码的实现，但是长度对我们的实现很重要。一次性密码本是理想的，因为密钥大小与纯文本大小相同，提供了最大的安全性。然而，使用 OTP 代表了使其不切实际的挑战(大尺寸难以交换、产生…)。我们想要一种技术，它能够在一系列比特上循环，从末端绕回起点——一个环。永久的“生成器函数”(ES6 中引入的半协同程序)将完美地满足需求。

```
const ByteKeyRing = function *(byteLength, keyBitArray) {
    *// store the length so we know when to wrap*
    const keyBitArrayLength = keyBitArray.length

    *// loop in perpetuity incrementing i*
    let i = 0
    while(true) {

        *// build a byte according to the specified length*
        const byte = []
        for(let n = 0; n < byteLength; n++)
        {
            *// i mod keyBitArrayLength in {0..keyBitArrayLength-1}*
            const index = i % keyBitArrayLength

            *// store the bit*
            const bit = keyBitArray[index]
            byte.push(bit)
            i = i + 1
        }

        *// return out a byte by joining to a string and parsing base 2*
        yield parseInt(byte.join(''),2)
    }
}
```

生成器函数接受一个字节长度和一个由 0 和 1 组成的数组。它遍历该数组，构建一个字节，并在填充后返回给调用者。重要的细节是下一个请求是从上一个请求停止的地方开始的，数组的迭代不是从头开始。模操作符为我们提供了一个很好的机制，让我们不必自己管理包装。考虑长度为 4 的输入数组:

```
i  *% 4 = x*
----------
0  *% 4﻿ = 0*
1  *% 4 = 1* 
2  *% 4 = 2*
3  *% 4 = 3*
4  *% 4 = 0*
5  *% 4 = 1*
.
.
.﻿﻿﻿﻿﻿﻿
﻿67 *% 4 = 3*
```

随着 I 的增加(永久),如果我们以 4 为模，结果将总是在集合{0，1，2，3 }中。仔细观察戒指

```
const ring = ByteKeyRing(8, [0,0,1])

const thirtySix  = ring.next()
*//=> 36 (00100100)*

const oneFourtySix  = ring.next()
*//=> 146 (10010010)*

const seventyThree  = ring.next()
*//=> 73 (01001001)*

const thirtySixAgain = ring.next()
*//=> 36 (00100100)*
```

我们用数组`[0,0,1]`给环播种，产生垂直流`001001001001001001001001001001001…001`。如果我们把

*   左 8 个最高位(`00100100`)我们有值`36`，
*   紧跟在该永续流之后的 8 个字节是(`10010010`)值`146`，
*   接下来是 8 个字节(`01001001`)的值`73`
*   然后整个序列重复。

这种方法允许我们使用一个小密钥(在本例中是三位)来加密任意大小的消息。加密/解密由少量代码执行，该代码将 xor 应用于包含明文的缓冲区，返回密文缓冲区，其中密钥和明文缓冲区是输入(readBuffer、XOR、buffer of 是在[源](https://github.com/tb01923/understanding-cryptography/blob/master/ciphers/stream-cipher.js)中定义的帮助器):

```
const cipherBuffer = (keyByte, byteBuffer) => {
    const plainByte = readBuffer(byteBuffer)
    const cipherByte = xor(keyByte, plainByte)
    const cipherByteBuffer = bufferOf(cipherByte)
    return cipherByteBuffer
}
```

考虑到节点中对流的支持，流位是相当容易的。在这种情况下，我们利用转换流:

```
const { Transform } = require('stream');

const keyIterator = (byteKeyArray) => {
    const byteSize = 8
    const byteKeyRing = ByteKeyRing(byteSize, byteKeyArray)
    return () => byteKeyRing.next().value
}

const streamCipher = (byteKeyArray) =>{

    const nextKey = keyIterator(byteKeyArray)

    return new Transform({
        transform(byteBuffer, encoding, callback) {

            const keyByte = nextKey()
            const cipherByteBuffer = cipherBuffer(
                keyByte, byteBuffer)
            this.push(cipherByteBuffer);
            callback();

        }
    });
}
```

这里，keyIterator 用于从中提取密钥环，从剩余的密码实现中提取生成器逻辑。这里真正的逻辑是这四行:

```
const keyByte = nextKey()
const cipherByteBuffer = cipherBuffer(keyByte, byteBuffer)

this.push(cipherByteBuffer);
callback();
```

本质上是三个步骤

1.  生成下一个密钥
2.  获取输入缓冲区并对其执行加密
3.  沿着流向下移动它(推送和回调是 TransformStream 的内部工作方式)

要使用它，您可以用一个密钥实例化一个 streamCipher，并通过管道传输一些文本:

简单消息被转换成某种密码(同样，这是一种不安全的简单密码)。往返的 encrypt -> decrypt 需要用相同的密钥(相同的密钥非常重要)实例化两个密码，通过一个密码(encrypt)然后通过另一个密码(decrypt)管道文本。

真的就这么简单！这里的代码是[这里的](https://github.com/tb01923/understanding-cryptography)。

*原载于*[*https://www.linkedin.com*](https://www.linkedin.com/pulse/symmetric-cryptography-approximating-stream-ciphers-javascipt-brown/)*。*