# OC:原始密码(移位密码，又名凯撒密码)

> 原文：<https://medium.com/nerd-for-tech/oc-original-cipher-shift-cipher-aka-caesar-cipher-fac55f1aea91?source=collection_archive---------16----------------------->

我正在加深对密码学的理解。虽然我可能应该在 20 年前(甚至在 2014 年 Bit Coin 开始制造噪音时)开始这样做，但没有像现在这样的时间来继续学习。我选择的一篇文章是[理解密码学](https://www.amazon.com/Understanding-Cryptography-Textbook-Students-Practitioners/dp/3642041000/ref=sr_1_1?ie=UTF8&qid=1503942303&sr=8-1&keywords=understanding+cryptography)。计划是让我的手脏起来，跟着文本一起编码(向自己证明最低限度的能力)。我选择的武器(在过去的三年里)是 JavaScript——虽然它是第一个密码的任务，但它也不是没有挑战。

该算法采用一个字母表、一个偏移量和文本，并用字母表中偏移了偏移量的字符数的值替换文本中的每个字符。如果偏移量超过了字母表的末尾，它将返回到开头。为了加密，我们取一个字符，找到它的数值(我们称之为 x)，加上偏移量(我们称之为 k)和模数除以字母表中的字符数(我们称之为 m)。因此

```
encrypt(x) = (x + k) mod m
```

解密是相似的

```
decrypt(x) = (x - k) mod m
```

解决方案中的琐碎问题(我首先通过在 Excel 中建模来确认)在练习进行到 10 分钟时被打破了，结果是关于负数的模有一些模糊性。有两个[实现](https://en.wikipedia.org/wiki/Modulo_operation)，如果你没有使用正确的一个，事情就不会工作。因为 javascript 模操作符并不是这个用例的正确实现。这是我第一次遇到这种模奇怪，需要一些研究来解决(感谢维基百科)。我定义了自己的模函数:

```
const quotient = (dividend,divisor) => 
      Math.floor(dividend/divisor)

const floor_modulo = (dividend,divisor) => 
      dividend - divisor * quotient(dividend,divisor)
```

在许多其他助手的帮助下(为了让我的算法易于阅读)，我最终得到了这个解决方案:

```
const {reduce, pipe, curry, find} = 
       require('../helpers/functional-bits')const {maxOr, increment, appendToObj, appendToArray, join, length,
    floor_modulo, numberOfKeys, values} = require('../helpers/common-bits')

const maxOrNegOne = maxOr(-1)

const alphabet = 'abcdefghijklmnopqrstuvwxyz'

*// arrayToAlphabetMap :: [k] -> {k: Number}*
const arrayToAlphabetMap = reduce(
    (map, a) => appendToObj(map, a, getNextValue(map)),
    {}
)

*// getNextValue :: {character: Number} -> Number*
const getNextValue = pipe([
    values,
    maxOrNegOne,
    increment
])

*// enryptCharacter  :: ({character: Number} -> Number -> Number -> 
//     character -> character) -> {character: Number} -> Number -> 
//     Number -> [character] -> [character]*
const applyWith = curry((algo, alphabetMap, k, m, xs) =>
    reduce(
        (arr, x) => appendToArray(
        arr, algo(alphabetMap, k, m, x))
        ,[]
        , xs
    )
)

*// enryptCharacter  :: {character: Number} -> Number -> Number -> 
//     character -> character*
const encryptCharacter = curry((alphabetMap, k, m, x) => {
    const rawValue = alphabetMap[x]
    const shiftValue = floor_modulo((rawValue + k), m)
    const shiftCharacter = find(alphabetMap, shiftValue)
    return shiftCharacter
})

*// decryptCharacter  :: {character: Number} -> Number -> Number -> 
//      character -> character*
const decryptCharacter = curry((alphabetMap, k, m, x) => {
    const shiftValue = alphabetMap[x]
    const rawValue = floor_modulo((shiftValue - k), m)
    const rawCharacter = find(alphabetMap, rawValue)
    return rawCharacter
})

*// shiftCipher :: string, Number -> 
//      { encrypt :: string -> string, decrypt :: string -> string }*
const shiftCipher = (alphabet, k) => {
    const alphabetArray = alphabet.split('')
    const alphabetMap = arrayToAlphabetMap(alphabetArray)
    const m = numberOfKeys(alphabetMap)

    return {
        encrypt: pipe([
            applyWith(encryptCharacter, alphabetMap, k, m),
            join
        ]),
        decrypt: pipe([
            applyWith(decryptCharacter, alphabetMap, k, m),
            join
        ]),
    }
}

const raw = 'attack'
const cipher = shiftCipher(alphabet, 17)

const secret = cipher.encrypt(raw)
*//=> rkkrtb*

const raw2 = cipher.decrypt(secret)
*//=> attack*
```

请注意:忍住痒——这不是一个安全的加密算法。但是请玩代码(你可以在这里找到[)。还拿到代码了？我很想收到你的来信，给我写封短信吧！](https://github.com/tb01923/understanding-cryptography)

## 关于我

[](https://www.linkedin.com/in/tb02118/) [## Todd Brown——副总裁&创新和敏捷工程高级总监——Liberty…

### Todd 在软件行业有 20 多年的经验，专注于架构、安全和…

www.linkedin.com](https://www.linkedin.com/in/tb02118/) 

*原载于*[*https://www.linkedin.com*](https://www.linkedin.com/pulse/oc-original-cipher-shift-aka-caesar-todd-brown/)*。*