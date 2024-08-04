---
title: 混合加密 
tags: [密码学,对称加密,非对称加密]
categories: [加密算法]
---

# 在信任、性能和安全性方面，混合公钥加密是赢家

我们喜欢公钥加密。它使我们能够安全地交换机密并对数据进行数字签名。但是，在实际加密大量数据时，它并不那么有效。

为此，Bob 将他的公钥发送给 Alice，她用它加密数据并发回密文。然后，Bob 使用关联的私钥对其进行解密。这对于少量数据（例如加密 128 位或 256 位加密密钥）相当有效，但在加密大量数据时，计算成本会变得很高。当我们使用移动设备时，这一点尤其重要，因为计算量的增加通常会耗尽电池电量。我们手头加密数据的核心方法是使用 RSA，但 RSA 通常是在移动设备上实施的重度方法。

那么，解决方案是什么？我们能否将对称密钥加密与公钥加密相结合？
## 对称密钥加密与公钥加密相结合
RFC 9180 提供了一种 HPKE（混合公钥加密）[[此处](https://www.rfc-editor.org/rfc/rfc9180.html)]的解决方案：

![](https://cdn-images-1.readmedium.com/v2/resize:fit:800/0*IzDACVGmBqiHcFCC.png)

这样，我们使用对称密钥来加密数据，然后使用公钥加密来加密对称密钥。我们还可以使用私钥对数据进行签名，并且可以使用关联的公钥进行检查。通过这种方式，我们还可以将身份验证集成到传输的数据中，并且可以正确验证数据的发送者（使用发送者的公钥）。

苹果也刚刚宣布，他们的CryptoKit现在将以Beta形式支持HPKE：

![](https://cdn-images-1.readmedium.com/v2/resize:fit:800/0*ePpotlntVS7-dfzh.png)

除了提供具有附加数据的经过身份验证的加密 （AEAD） 算法外，该库还支持**密钥派生函数** （KDF） 来创建共享密钥：

- HKDF_SHA256.
	- 它使用带有 SHA-256 的基于 HMAC 的密钥派生函数。
- HKDF_SHA384.
	- 这使用基于 HMAC 的密钥派生函数和 SHA-384。
- HKDF_SHA512.
	- 这使用带有 SHA-512 的基于 HMAC 的密钥派生函数。

**密钥封装机制** （KEM） 用于传递共享密钥。为了提高效率，它使用 ECC（椭圆曲线加密）和曲线 25519 或 P256：

- Curve25519_HKDF_SHA256.
	- 这将使用带有 SHA-256 哈希的 X25519。
- P256_HKDF_SHA256.
	- 这使用带有 SHA-256 哈希的 P256 （secp256r1） 曲线。
- P384_HKDF_SHA384 .
	- 这使用带有 SHA-384 哈希的 NIST P384 曲线。
- P521_HKDF_SHA512.
	- 它使用带有 SHA-512 哈希的 NIST P521 曲线，并且具有最强的安全性。

对于对称密钥加密，有两种主要方法：具有 GCM 模式的 AES 和 ChaCha20/Poly1305：

- AES_GCM_128.
	- 它使用具有计数器模式 （GCM） 的 128 位 AES。这是一种经过验证的 AES 快速加密模式，可将分组密码转换为流密码。
- AES_GCM_256.
	- 它使用具有伽罗瓦/计数器模式 （GCM） 的 256 位 AES。
- chaChaPoly.
	- 它使用带有 Poly1305 MAC（消息验证码）的 ChaCha20 流密码。

## 混合加密

许多其他图书馆已开始采用 HPKE，包括 CIRCL 图书馆：

[https://asecuritysite.com/golang/go_hybrid](https://asecuritysite.com/golang/go_hybrid)

通过ECC（椭圆曲线加密），我们有机会同时使用公钥加密的强大功能，以及对称密钥加密的速度和安全性。因此，我们慢慢转向加密的最佳实践，其中围绕以下方面有越来越多的共识：

- 公钥加密曲线：P256、P384、P521、X25519和X448。
- 密钥派生 （HKDF） 的哈希方法：SHA256、SHA384 和 SHA512。
- 对称密钥：128 位 AES GCM 和 256 位 AES GCM。

上述所有方法都与大多数系统兼容。
为此，Bob 和 Alice 将选择一条曲线来定义他们的密钥对，然后使用给定的哈希方法来派生加密密钥。这通常是通过HKDF（HMAC密钥派生函数）实现的。对于实际加密，我们可以使用对称密钥加密，因为这是最有效的，并且比公钥加密快得多。总体而言，有了这个，总体上倾向于使用AEAD（具有附加数据的身份验证加密）。典型的模式是 GCM。因此，让我们使用 Golang 构建一种混合加密方法。

现在，假设 Bob 将向 Alice 发送加密消息。然后，Alice 将生成一个密钥对（公钥和私钥）。然后，她将公钥发送给 Bob，然后他使用它来派生加密（$S$）的对称密钥。然后，他使用 $K$ 和 AES GCM 对消息进行加密。Bob 接收到密码（$C$）和值 $R$。然后，她可以从 $R$ 中派生出私钥 $S$。使用此密钥，她可以解密密文以派生明文消息。

在这种方法中，Alice 生成一个随机私钥（$d_A$），然后在椭圆曲线（$G$）上取一个点，然后确定她的公钥（$Q_A$）：

$QA=dA×GQ_A = d_A \times GQA​=dA​×G$

因此，$G$ 和 $Q_A$ 是椭圆曲线上的点。然后，Alice 将 $Q_A$ 发送给 Bob。接下来，Bob 将生成：

$R=r×GR = r \times GR=r×G S=r×QAS = r \times Q_AS=r×QA​$

其中 $r$ 是 Bob 生成的随机数。然后，对称密钥（$S$）用于加密消息。

然后，Alice 将与 $R$ 一起接收加密消息。然后，她能够通过以下命令确定相同的加密密钥：

$S=dA×RS = d_A \times RS=dA​×R$

即：
$S=dA×(r×G)S$ 
$= d_A \times (r \times G)S$
$=dA​×(r×G) S$
$=r×(dA×G)S$ 
$= r \times (d_A \times G)S$
$=r×(dA​×G) S$
$=r×QAS$ 
$= r \times Q_AS$
$=r×QA​$
![](https://cdn-images-1.readmedium.com/v2/resize:fit:800/0*so8g9sAGdwSJStUV.png)

## 示例代码
示例运行是 [[here](https://asecuritysite.com/golang/go_hybrid)[：

```
Public key type: HPKE_KEM_P256_HKDF_SHA256
 Params kem_id: 16 kdf_id: 1 aead_id: 1
Key exchange parameters:
 Ciphersize:  65
 EncapsulationSeedSize: 32
 PrivateKeySize: 32
 PublicKeySize:  65
 SeedSize:  32
 SharedKeySize:  32
Cipher parameters:
 Key Length: 16
Key derivation function:
 Extract size: 32
Message: Testing 123
Cipher: 74268e3f6f7bc6b21c5071c1a78c8154c6cf1be7f2b93370445026
Decipher: Testing 123
```

代码基于 [[here](https://github.com/cloudflare/circl)]：

```go
package main
import (
 "crypto/rand"
// "encoding/hex"
 "fmt"
 "os"
 "strconv"
 "github.com/cloudflare/circl/hpke"
)
func main() {
 kemID := int(hpke.KEM_P256_HKDF_SHA256)
 kdfID := int(hpke.KDF_HKDF_SHA256)
 aeadID := int(hpke.AEAD_AES128GCM)
 msg := "Hello"
 argCount := len(os.Args[1:])
 if argCount > 0 {
  msg = os.Args[1]
 }
 if argCount > 1 {
  kemID, _ = strconv.Atoi(os.Args[2])
 }
 if argCount > 2 {
  kdfID, _ = strconv.Atoi(os.Args[3])
 }
 if argCount > 3 {
  aeadID, _ = strconv.Atoi(os.Args[4])
 }
 suite := hpke.NewSuite(hpke.KEM(kemID), hpke.KDF(kdfID), hpke.AEAD(aeadID))
 info := []byte("Test")
 Bob_pub, Bob_private, _ := hpke.KEM(kemID).Scheme().GenerateKeyPair()
 Bob, _ := suite.NewReceiver(Bob_private, info)
 Alice, _ := suite.NewSender(Bob_pub, info)
 enc, sealer, _ := Alice.Setup(rand.Reader)
 Alice_msg := []byte(msg)
 aad := []byte("Additional data")
 ct, _ := sealer.Seal(Alice_msg, aad)
 opener, _ := Bob.Setup(enc)
 Bob_msg, _ := opener.Open(ct, aad)
 if (kemID!=48) {fmt.Printf("Public key type:\t%s\n", Bob_pub.Scheme().Name()) }
 fmt.Printf(" Params\t%s\n", suite.String())
 fmt.Printf("\nKey exchange parameters:\n")
 fmt.Printf(" Ciphersize:\t\t%d\n", hpke.KEM(kemID).Scheme().CiphertextSize())
 fmt.Printf(" EncapsulationSeedSize:\t%d\n", hpke.KEM(kemID).Scheme().EncapsulationSeedSize())
 fmt.Printf(" PrivateKeySize:\t%d\n", hpke.KEM(kemID).Scheme().PrivateKeySize())
 fmt.Printf(" PublicKeySize:\t\t%d\n", hpke.KEM(kemID).Scheme().PublicKeySize())
 fmt.Printf(" SeedSize:\t\t%d\n", hpke.KEM(kemID).Scheme().SeedSize())
 fmt.Printf(" SharedKeySize:\t\t%d\n", hpke.KEM(kemID).Scheme().SharedKeySize())
 fmt.Printf("\nCipher parameters:\n")
 fmt.Printf(" Key Length:\t%d\n", hpke.AEAD(aeadID).KeySize())
 fmt.Printf("\nKey derivation function:\n")
 fmt.Printf(" Extract size:\t%d\n", hpke.KDF(kdfID).ExtractSize())

 fmt.Printf("\nMessage:\t%s\n", Alice_msg)
 // fmt.Printf("Cipher:\t%x\n", hex.EncodeToString(ct))
 fmt.Printf("Cipher:\t%x\n", ct)
 fmt.Printf("Decipher:\t%s\n", Bob_msg)
}
```

# 结论

虽然 OpenSSL 提供了如此多的加密方法，但它可能会使应用程序容易受到使用传统方法的攻击。除此之外，RSA加密等方法对电池的影响很大。MD5 和 SHA-1 等传统哈希方法也会使应用程序受到攻击。因此，RFC 9180 提供了一种使用最佳安全性的方法，以及有效的方法。所以，去混合动力吧！

