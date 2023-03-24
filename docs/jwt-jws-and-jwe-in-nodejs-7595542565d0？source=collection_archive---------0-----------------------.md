# nodejs 的 JWT、JWS 和 JWE

> 原文：<https://medium.com/nerd-for-tech/jwt-jws-and-jwe-in-nodejs-7595542565d0?source=collection_archive---------0----------------------->

![](img/247d71500fcaa51124f5d4fced631ec2.png)

JSON Web Token (JWT 读作“jot”)是 IETF 标准，用于以安全和紧凑的方式在相关方之间传输 JSON 格式声明。JWT 已经成为基于令牌授权的开放标准。

> JSON Web Token (JWT)是一种简洁的、URL 安全的方式，用于表示在双方之间传输的
> 声明。JWT
> 中的声明被编码为一个 JSON 对象，该对象被用作 JSON
> Web 签名(JWS)结构的有效载荷，或者作为 JSON Web
> 加密(JWE)结构的明文，使得声明能够被数字
> 签名或者用消息认证码
> (MAC)进行完整性保护和/或加密。

## JWT 的属性

*   小型的，紧凑的
*   声明是 JSON 编码的
*   URL 安全(base 64 URL 编码)
*   自包含(因此实现无状态授权是可能的)

## 使用 JWT 的授权

说到授权，有两种主要方法

1.  基于无会话令牌(JWT)
2.  基于会话(cookies)

重要的是首先要详细理解基于会话的授权机制，以确定使用无会话的基于令牌的授权的优势。基于会话的授权方法有两个部分。

1.  服务器根据会话 id 存储引用对象
2.  客户端将会话(cookie)保存在应用程序中，其中包含服务器的详细信息，如域

当客户机需要访问服务器资源时，它发送带有 HTTP 请求的会话(cookie)。服务器验证会话以授予对资源的访问权。服务器必须读取与会话相关的引用对象，以进一步识别请求者。基于 JWT 令牌的授权系统解决了这一过程的一些限制。

**基于令牌授权的优势**

*   无国籍的
*   分散授权(微服务友好)
*   混合，单页应用友好
*   移动应用友好
*   不需要集中式令牌存储

## JWT 变奏

以下是摘自(https://OpenID . net/specs/draft-Jones-JSON-we B- signature-04 . html)的 JWT 示例。这有三个部分，由句点(.)是 base64 URL 编码的纯文本值。

> eyj 0 exaioijkv 1 qilcjhbgcioijiuzi 1 nij 9 . eyjpc 3 mioijqb 2 uilchlehaiojezmda 4 MTK zodasimh 0 DHA 6 ly 9 legftcgxllmnvbs 9 PC 19 Yb 290 ijp 0 cnvlfq . KD 7k 33 MK 6 GL 5 nlsy 6-C5 xzkvbzxcf 4 l5 S6 umwtnsg

jwt.io 用于解码这个令牌并检查内容。

![](img/b29cd11e182d9a01859a0bbd653f5dbf.png)

来自 jwt.io 的 JWT 示例

## 令牌的“三个部分”

**Header: { "typ": "JWT "，" alg": "HS256"}**

这个头被称为 **JOSE** (Javascript 对象签名和加密)包含有关生成令牌的签名和/或加密算法的信息。上面的例子是用对称算法 **HS256** (HMAC 和 SHA-256)签名的。

**有效载荷:**

有效载荷包含两种类型的声明，称为*注册声明*和*自定义声明* *。*RFC 中明确定义了注册声明。其中有 **iss** (发行人) **exp** (到期时间) **sub** (主体) **aud** (受众)。更多详情可在 RFC [文件](https://tools.ietf.org/html/rfc7519#section-4.1) [中找到。](https://tools.ietf.org/html/rfc7519#section-4.1.)注册声明可用作令牌的额外验证。自定义声明可以是基于应用程序需求定义的任何键/值对。

**签名:**

签名是通过将签名算法应用于报头和有效载荷组合来创建的。这可以防止在令牌发布给客户端后对其进行修改。

![](img/a308590391a684252ae61a67ebd03df2.png)

jwt.io 的签名部分

## JWT 变奏曲 JWS 和 JWE

**JWT** 被称为 **JWS** (JSON Web Signature)当令牌按照上面的例子签名时。 **JWS** 令牌具有完整性(不可能在不使签名无效的情况下修改令牌)。使用 base64 编码器可以很容易地解码 **JWS** 令牌，因此它不是传输敏感数据的好选择。JWE(JSON Web Encryption)通过将加密引入 JWT 来解决这个问题。JWS 是 JWT 实现的著名变体。有库和大量的样本可用于验证，解码和生成 JWS 令牌。因此，我们将使用示例代码详细讨论 JWE。

## JWE

JWE 是加密 JWT 令牌声明的标准方式。代码段解释了如何加载密钥、加密令牌和解密令牌。这些代码是使用 cisco [node-jose](https://github.com/cisco/node-jose#readme) 库在 node js 中编写的。

node-jose 库提供了一个 JWK 名称空间来生成、导入和导出密钥。在此示例中，非对称密钥对用于加密有效负载。

**使用开放 SSL 命令生成密钥对**

`openssl genrsa -des3 -out private.pem 2048`T39`openssl rsa -in private.pem -outform PEM -pubout -out public.pem`T40`openssl rsa -in private.pem -out plain_private.pem`

`cat plain_private.pem`

![](img/ff852d2948de8fcaef857e9b9140335d.png)

私人密钥

`cat public.pem`

![](img/934841755b6d58f0f35688dcc0f26121.png)

公开密钥

**关键导入样本代码:**

```
const { JWK } = require('node-jose');
let makeKeys = async () => {
    let privateKey = `-----BEGIN RSA PRIVATE KEY-----
MIIEowIBAAKCAQEAwiSxcx5xFsNCjfDTOXz0M6RivgPXTTKb/PH6/x4vYl1xg/vI
rG1yDGyFuyAizxNaGtA+qo2CvM3iATyDWQg3/8vDWiD4cIvUD2WE3XXewjyeGzQs
o26DceRxopI2tlSgvznjifetGCp+oj4BWrTCqLF1AsR2ioUS7vuXMlpxXFhJfyEG
oWuQaZDwoz7CZv1KrQwlRTtkqtn4IeXpVcgWhg/1r0iRsvNJDokyiMVY8hrrvza2
j31JGaKYqCNYSC8Jf5EV7ONQhYGgQeRVxn8jS8UhLOFXDdNtgHwKi02tF1bmb3Ko
/s3vzPvFB8C7CsYRtHAzcx/xor2/CbT0QGYfUQIDAQABAoIBADgdoQj0UJ3SvKcI
aBViz7cpmbzwoUfYDAx16SXalLmq5sfOfDeSvvdmWtU2ubj/D/lWHfbwRkzKebv/
wLt4S69tNz8S7pyhXh9BKcFVc5jTKqQUVZ67r9S4wjvKZXQYTApZ8jIL3AVzKv08
TOnp+6YE/RxaqyToyAs5v383czLMdSM9bThDw+IZHfKFcl2iyUI4XVaPLDZPzUlG
MW13ypwFQXbdro9pnMs9v2573WumIVvbiLqIdxK1anag0KbCnCY2hDmoLNv7Mohz
1pFiqEStI6DQsYfEm/geIIL9nLhA8QJ28B9DtLKqRN7oWl2w5BiMEDCrmVK/4niM
lsqF1ZECgYEA5zVsjoJzRCBgZ5dRx9BOu9MKXQ+h00LS3+JaApEzW+QV0DplTvb5
aRUHhTIhUig2WdQVrEiBVXKg9eYmOBXOpX5pgDO5XJ16KcjdHcwMwoYAcUPIT5Y2
tMgoX+jCgTt95cnLKhAnSlH9cCyq+nmcdfDNuvNx8HJO7P33MaVDHCUCgYEA1vXX
cBWM+sGkUxpJNwlgA8bO7xq2kpbM3gqa0z6SKQhxnnmPyoKsZSHdwQMUCKIWBLjU
n27STmXm2SblT1cT39MCED2t0U7N2D1KUQfZkFaN4gImF77iZBWIfDLObAom6JKg
gER0glakAjbDMKq90mVzrKrR6XsD3az+AJE1eL0CgYBbHKB04E9QD3ouGGFv3lTI
i3fQCHL676Bt/aL+0/1rPsyhzAFURtsuX80g3gpnd9VDPOJ1i/T7mTp47IMvItjW
OqSUks1/A8e6Y59POLPmjCvsdoufYVCZmS7f7LeJeco5HXZkUw1Iqlq3M8MFBZt0
lrpb68eAu2sC1WnuNHPnyQKBgQC1LFb7vO4pmnOpJwp2PIyUIkfe9qDSRA8/Rajg
Smhd9SPt8X7jq+cpBbYlKzcDX7k3GaD7DyhpszEx7LpweG/jwbCHh7SsKMMNcfrk
+LzCDnFe/3ijotqkiBGUvC2GmbfZZVupQAkCoYtx4j35Eq8SWTd5XC+3nVrQxzD9
wWVT8QKBgBylVSsVeU7H/RT3RZ/qgWnpllBzGJic8X6lfyTHVD/9dO9XMzuD7R+/
VFc9Ib71QPzyq39BX+YiCI+xwDGARgO3GGA4vxvF46Tq0rYUpZ1LiC4raO4I5xrl
NxZCLVlhLWU27FR7xu8YN18mRCCwz1FCcsEtNU4nrBK2IIIwGLCf
-----END RSA PRIVATE KEY-----`;
    let publicKey = `-----BEGIN PUBLIC KEY-----
MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAwiSxcx5xFsNCjfDTOXz0
M6RivgPXTTKb/PH6/x4vYl1xg/vIrG1yDGyFuyAizxNaGtA+qo2CvM3iATyDWQg3
/8vDWiD4cIvUD2WE3XXewjyeGzQso26DceRxopI2tlSgvznjifetGCp+oj4BWrTC
qLF1AsR2ioUS7vuXMlpxXFhJfyEGoWuQaZDwoz7CZv1KrQwlRTtkqtn4IeXpVcgW
hg/1r0iRsvNJDokyiMVY8hrrvza2j31JGaKYqCNYSC8Jf5EV7ONQhYGgQeRVxn8j
S8UhLOFXDdNtgHwKi02tF1bmb3Ko/s3vzPvFB8C7CsYRtHAzcx/xor2/CbT0QGYf
UQIDAQAB
-----END PUBLIC KEY-----`
    const jwKeys = await Promise.all([
        JWK.asKey(privateKey, "pem"),
        JWK.asKey(publicKey, "pem")
    ]);
    let keystore = JWK.createKeyStore();
    await keystore.add(jwKeys[0]);
    await keystore.add(jwKeys[1]);
}
makeKeys();
```

方法`JWK.asKey`用于从 PEM 格式的字符串中导入密钥。密钥格式可以作为第二个参数传递给该方法，它返回 JWK 格式的密钥作为输出。可以将导入的密钥添加到密钥库中，稍后在加密和解密 JWE 令牌时传递。

**JWE 代样本代码:**

```
const { JWK, JWE } = require('node-jose');
encrypt = async (raw, format = 'compact', contentAlg = "A256GCM", alg = "RSA-OAEP") => {
    let _publicKey = `-----BEGIN PUBLIC KEY-----
MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAwiSxcx5xFsNCjfDTOXz0
M6RivgPXTTKb/PH6/x4vYl1xg/vIrG1yDGyFuyAizxNaGtA+qo2CvM3iATyDWQg3
/8vDWiD4cIvUD2WE3XXewjyeGzQso26DceRxopI2tlSgvznjifetGCp+oj4BWrTC
qLF1AsR2ioUS7vuXMlpxXFhJfyEGoWuQaZDwoz7CZv1KrQwlRTtkqtn4IeXpVcgW
hg/1r0iRsvNJDokyiMVY8hrrvza2j31JGaKYqCNYSC8Jf5EV7ONQhYGgQeRVxn8j
S8UhLOFXDdNtgHwKi02tF1bmb3Ko/s3vzPvFB8C7CsYRtHAzcx/xor2/CbT0QGYf
UQIDAQAB
-----END PUBLIC KEY-----`
    let publicKey = await JWK.asKey(_publicKey, "pem");
    const buffer = Buffer.from(JSON.stringify(raw))
    const encrypted = await JWE.createEncrypt({ format: format, contentAlg: contentAlg, fields: { alg: alg } }, publicKey)
        .update(buffer).final();
    return encrypted;
}
let raw = {
    "mobileNumber": "1234567890",
    "customerId": "000000000",
    "sessionId": "3a600342-a7a3-4c66-bbd3-f67de5d7096f",
    "exp": 1645544094,
    "jti": "f3902a08-0e24-4dcc-bed1-f4cd9611bfad"
};
encrypt(raw);
```

JWE 是用加载的公钥加密的，下面一行可以用来解释加密过程。理解加密过程以弄清楚这些参数的用法是很重要的

```
const encrypted = await JWE.createEncrypt({ format: format, contentAlg: contentAlg, fields: { alg: alg } }, publicKey).update(buffer).final()
```

***格式*** :序列化格式，JWE 有两种序列化格式叫做*紧凑序列化*和 *JSON 序列化*，代表加密的有效载荷。

***contentAlg*** :内容加密算法(A256GCM)

***alg*** : CEK 密钥加密算法(RSA-OAEP)

## 加密过程:

***contentAlg***(a 256 GCM)是内容加密算法，是 **AES** (高级加密标准*)**GCM**(伽罗瓦计数器模式)算法，密钥为 256 位。这是一个对称密钥，在大型内容加密中速度更快。 **CEK** (内容加密密钥)是由加密有效载荷的库生成的随机 256 位密钥。加密密钥也使用由**算法**属性定义的算法加密。*

*非对称加密使用的***algo***(RSA-OAEP)RSA 算法与 **OAEP** (最优非对称加密填充)方法。加密的 CEK 也放在 JWE 令牌中，接收者可以用它来解密有效载荷。这种方法称为密钥加密或密钥包装。更多细节可以在 [RFC](https://tools.ietf.org/html/draft-ietf-jose-json-web-algorithms-40#section-4.1) 中找到*

***上面的代码产生的 JWE 令牌:***

> *eyjhbgcioijsu 0 ett 0 ffucisimvuyyyi 6 ikeyntzhq 00 ilcrawqioijjuhz 0 tfn 3s 29 bmtj 1 aw 9 xc 3 izewhhdwnjwg 12 rvubwnucxk 1 QS 1 ebvzzin 0 . ai 5j 5k 43 skqp lwr 0 Cu 1 ziywotuplfkcn 5 cuzzxhdp 0 exqjylgpj 8 jywu 8 ytu 9 rwzqen 9 ey 0 _ 81 hqhxezmqgfcsrm 0 hxjcew' HRhA5nn8HLsvYf8F-BzQew '*

*生成的*紧凑序列化*令牌由五个 base64 URL 编码部分组成，由句点(.).*

***那些是…***

1.  *JOSE header: {"alg":"RSA-OAEP "，" enc":"A256GCM "，" kid ":" gpvtlswkoa 12 uiowsr 3 yhaucixmvdutmct qy5a-DmVs " }*
2.  *加密 CEK*
3.  *加密有效载荷(CEK)*
4.  *初始化向量*
5.  *认证标签*

***等价的 JSON 序列化如下图所示。***

*![](img/89dfe813f2e76b48fd03ad36cd7b4bf6.png)*

*JSON 序列化令牌*

***解密和验证:***

*示例代码解密并返回来自前面步骤中生成的*紧凑序列化* JWE 令牌的纯文本声明。观察如何使用私钥来解密使用公钥加密的令牌。*

```
*const { JWK, JWE, parse } = require('node-jose');
decrypt = async (encryptedBody) => {
    let _privateKey = `-----BEGIN RSA PRIVATE KEY-----
MIIEowIBAAKCAQEAwiSxcx5xFsNCjfDTOXz0M6RivgPXTTKb/PH6/x4vYl1xg/vI
rG1yDGyFuyAizxNaGtA+qo2CvM3iATyDWQg3/8vDWiD4cIvUD2WE3XXewjyeGzQs
o26DceRxopI2tlSgvznjifetGCp+oj4BWrTCqLF1AsR2ioUS7vuXMlpxXFhJfyEG
oWuQaZDwoz7CZv1KrQwlRTtkqtn4IeXpVcgWhg/1r0iRsvNJDokyiMVY8hrrvza2
j31JGaKYqCNYSC8Jf5EV7ONQhYGgQeRVxn8jS8UhLOFXDdNtgHwKi02tF1bmb3Ko
/s3vzPvFB8C7CsYRtHAzcx/xor2/CbT0QGYfUQIDAQABAoIBADgdoQj0UJ3SvKcI
aBViz7cpmbzwoUfYDAx16SXalLmq5sfOfDeSvvdmWtU2ubj/D/lWHfbwRkzKebv/
wLt4S69tNz8S7pyhXh9BKcFVc5jTKqQUVZ67r9S4wjvKZXQYTApZ8jIL3AVzKv08
TOnp+6YE/RxaqyToyAs5v383czLMdSM9bThDw+IZHfKFcl2iyUI4XVaPLDZPzUlG
MW13ypwFQXbdro9pnMs9v2573WumIVvbiLqIdxK1anag0KbCnCY2hDmoLNv7Mohz
1pFiqEStI6DQsYfEm/geIIL9nLhA8QJ28B9DtLKqRN7oWl2w5BiMEDCrmVK/4niM
lsqF1ZECgYEA5zVsjoJzRCBgZ5dRx9BOu9MKXQ+h00LS3+JaApEzW+QV0DplTvb5
aRUHhTIhUig2WdQVrEiBVXKg9eYmOBXOpX5pgDO5XJ16KcjdHcwMwoYAcUPIT5Y2
tMgoX+jCgTt95cnLKhAnSlH9cCyq+nmcdfDNuvNx8HJO7P33MaVDHCUCgYEA1vXX
cBWM+sGkUxpJNwlgA8bO7xq2kpbM3gqa0z6SKQhxnnmPyoKsZSHdwQMUCKIWBLjU
n27STmXm2SblT1cT39MCED2t0U7N2D1KUQfZkFaN4gImF77iZBWIfDLObAom6JKg
gER0glakAjbDMKq90mVzrKrR6XsD3az+AJE1eL0CgYBbHKB04E9QD3ouGGFv3lTI
i3fQCHL676Bt/aL+0/1rPsyhzAFURtsuX80g3gpnd9VDPOJ1i/T7mTp47IMvItjW
OqSUks1/A8e6Y59POLPmjCvsdoufYVCZmS7f7LeJeco5HXZkUw1Iqlq3M8MFBZt0
lrpb68eAu2sC1WnuNHPnyQKBgQC1LFb7vO4pmnOpJwp2PIyUIkfe9qDSRA8/Rajg
Smhd9SPt8X7jq+cpBbYlKzcDX7k3GaD7DyhpszEx7LpweG/jwbCHh7SsKMMNcfrk
+LzCDnFe/3ijotqkiBGUvC2GmbfZZVupQAkCoYtx4j35Eq8SWTd5XC+3nVrQxzD9
wWVT8QKBgBylVSsVeU7H/RT3RZ/qgWnpllBzGJic8X6lfyTHVD/9dO9XMzuD7R+/
VFc9Ib71QPzyq39BX+YiCI+xwDGARgO3GGA4vxvF46Tq0rYUpZ1LiC4raO4I5xrl
NxZCLVlhLWU27FR7xu8YN18mRCCwz1FCcsEtNU4nrBK2IIIwGLCf
-----END RSA PRIVATE KEY-----`; let keystore = JWK.createKeyStore();
    await keystore.add(await JWK.asKey(_privateKey, "pem"));
    let outPut = parse.compact(encryptedBody);
    let decryptedVal = await outPut.perform(keystore);
    let claims = Buffer.from(decryptedVal.plaintext).toString();
    return claims;
}decrypt('eyJhbGciOiJSU0EtT0FFUCIsImVuYyI6IkEyNTZHQ00iLCJraWQiOiJHUHZ0TFN3S29BMTJ1aW9Xc3IzeWhhdWNJWG12RHVUbWNUcXk1QS1EbVZzIn0.ai5j5Kk43skqPLwR0Cu1ZIyWOTUpLFKCN5cuZzxHdp0eXQjYLGpj8jYvU8yTu9rwZQeN9EY0_81hQHXEzMQgfCsRm0HXjcEwXIny-FH8N2WtYCjWrQubAIbnrqnNP-8TeCNZaShEMwYcVLUls8Yik2s6LiHTb9FTNfbMxyBlaQ4v8Q2rnKQ4tto5IG7D_dRC59WDmN2XkVfw3DWNbOxb34wgKHq0Zb7YcflQyXYxqkOAU9Xeuv_urFAhUyCzHt8W2vbqUtSosmHGoLf64QKUwqKietOVEI8tAruj-JTHUTR4P_-ZHy9jEgTNQLfZlkBDEDnXme2tcYqHj9XiidiXpw.thh69dp0Pz73kycQ.eF5bjuqtF60gW8O8cXKiYyDsBPX8OL0GQfhOxwGWUmYtHOds7FJWTNoSFnv5E6A_Bgn_2W2JUD1kniYUR1yOPnEJfHQ_SXMLJECZ8mBpHlYZWl3-39nGESxfdx1oXP1pO_ahP9eWtBCeS3X8DgPV0sLu1N-0ox6iZxTQ_PU314yTiUobhemNfT9ORghnOSApHi_HCKk9G1ABklbP82flcBfzA1tWKBw.HRhA5nn8HLsvYf8F-BzQew');*
```

## *结论*

*这篇文章解释了 JWT 的基本知识，主要是 JWE 如何工作。添加的代码示例有硬编码的密钥和证书，主要是为了解释事情是如何工作的。不建议在代码中硬编码敏感信息，请采取必要的措施，如 KMS 或保险库来存储和访问密钥/证书。*