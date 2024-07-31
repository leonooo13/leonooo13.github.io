---
title: 什么是SSL证书
tags: [SSL,WebSec]
categories: [NetSec]
---

## SSL证书的工作原理

SSL（安全套接字层）以及此协议的升级版本，称为TLS（传输层安全性），是一种构建[加密](https://us.norton.com/blog/privacy/what-is-encryption)Web 浏览器和服务器之间的连接。

您可以将用户和网站视为峡谷两侧的两座建筑物。为了让用户访问网站，反之亦然，需要有一个桥梁。SSL证书就是那座桥梁——它是安全的，并允许信息安全地从一侧传输到另一侧。

在访问具有SSL证书的网站的几毫秒内，会发生许多重要的过程：

- 您的浏览器会向网站的服务器发送请求，要求提供安全页面。
- 服务器将SSL证书与公钥一起传输。公钥对数据进行加密并验证数字签名。
- 您的浏览器会验证数字签名的合法性，并在地址栏中显示挂锁图标。
- 然后，您的浏览器会使用密钥将加密数据传输到站点的服务器。
- 使用私有解密密钥，服务器读取数据并访问密钥。
- 只要连接就位，浏览器和服务器就会使用秘密解密密钥来回共享安全数据。

如果您在线并访问没有 SSL 加密的网站，您可能会被警告”[您的连接不是私有的](https://us.norton.com/blog/how-to/your-connection-is-not-private).”这意味着网络犯罪分子可以拦截您在该网站上分享的任何内容。

![Illustrated chart covering how SSL certificates work.](https://us.norton.com/content/dam/blogs/images/norton/am/what-is-an-ssl-certificate-01.png)

## SSL 的类型

SSL 有三种主要类型：扩展验证、组织验证和域验证。其中有一些变体作为亚型存在，它们包括通配符、MD/SAN 和 UCC。

主要区别在于需要哪些信息来保护每种类型。扩展验证证书需要的信息最多，而域验证证书需要的信息最少。

这通常意味着信息量更大的证书更值得信赖，因为获得证书所需的信息深度。

在这三种主要类型中，有更专业的版本，通常是为企业或其他大型组织设计的。

每个 SSL 证书都包含以下信息：

- 域名
- 拥有证书的公司、个人或设备
- 子域名
- 证书颁发机构 （CA）
- CA 的数字签名
- 发行日期
- 有效期
- 公钥（私钥是保密的）

### 扩展验证证书 （EV SSL）

**EV SSL是所有SSL证书中经过最广泛审查和检查的证书。**对于要获得 EV SSL 的网站，它必须完成一个 16 步过程，验证有关网站所有权的详细信息。其中一些详细信息包括确认：

- 域
- 网站所有者
- 申请人的实际住址
- 开展业务的合法权利

这些SSL通常由大型公司和任何需要向公众展示最高可信度的机构使用，包括银行和支付处理器。当您访问其中一个网站时，EV SSL 表明域名所有者已采取重大措施[保护数据隐私的步骤](https://us.norton.com/blog/how-to/ten-ways-to-keep-your-data-private).

![Illustrated chart covering what an Extended Validation Certificate (EV SSL) is.](https://us.norton.com/content/dam/blogs/images/norton/am/what-is-an-ssl-certificate-02.png)

### 组织验证证书 （OV SSL）

获得 OV SSL 比申请 EV SSL 更容易。**对于颁发 OV SSL 的证书颁发机构，他们仅对实体执行基本审查。**他们检查组织或企业是否存在，以及申请证书的实体是否拥有域名。

OV SSL 的最常见用途是用于需要安全性但不面向公众的站点。例如，OV SSL 非常适合需要为内部系统提供安全登录页面或作为 Intranet 安全性的公司。

![Illustrated chart covering what an Organization Validated Certificate (OV SSL) is.](https://us.norton.com/content/dam/blogs/images/norton/am/what-is-an-ssl-certificate-03.png)

### 域验证证书 （DV SSL）

DV SSL是最基本的SSL证书类型。**证书颁发机构仅确认域由请求证书的个人或实体控制。** DV SSL 可以快速发布，但它提供与 EV 和 OV SSL 相同的加密级别。[网络安全](https://us.norton.com/blog/malware/what-is-cybersecurity-what-you-need-to-know)立场。因为它们相对容易获得并且可以提供安全的错觉，所以有时会在不安全的网站上找到 DV SSL。

这些 SSL 适用于小型企业网站、个人网站和博客，因为它们可以加密流量并允许人们在无需提供身份验证的情况下安全访问。

![Illustrated chart covering what a Domain Validation Certificate (DV SSL) is.](https://us.norton.com/content/dam/blogs/images/norton/am/what-is-an-ssl-certificate-04.png)

### 通配符SSL证书

**通配符 SSL 涵盖网站上的子域，而无需为每个子域提供单独的 SSL 证书。** 证书使用一个字符（通常是星号）作为多个其他字符（通常是其他页面或子域的名称）的替换。通配符在 DV 和 OV SSL 中都可用。

通配符证书对于在同一服务器上具有多个子域的实体非常有用。它们比为每个子域购买证书更实惠，并且它们允许您随着时间的推移添加和删除子域。

![Illustrated chart covering what a wildcard SSL is.](https://us.norton.com/content/dam/blogs/images/norton/am/what-is-an-ssl-certificate-05.png)

### 多域和主题备用名称 SSL 证书 （MD/SAN）

**MD 或 SAN SSL 证书使用单个证书认证多个域和子域。** 这些证书中的大多数可以同时用于多达 250 个不同的域。这些证书以 EV、OV 和 DV SSL 的形式提供。

对于拥有许多不同域的公司或组织，MD 或 SAN SSL 是保护所有这些域的最快、最简单的方法。

![Illustrated chart covering what a Multi-Domain Validation Certificate (MDC SSL) is.](https://us.norton.com/content/dam/blogs/images/norton/am/what-is-an-ssl-certificate-06.png)

#### **统一通信证书 （UCC）**

**UCC 是一种 SAN 证书，它允许在一个证书下保护三个或更多服务器上的多个域和子域。** UCC 还具有专门用于保护 Microsoft Exchange、Live 和通信服务器类型的附加功能。它们以 EV、OV 或 DV SSL 的形式提供。

对于在多个服务器上具有多个域和子域的大型组织，以及具有 Microsoft Exchange 服务器的组织，UCC 可以更轻松地管理 SSL 证书。

![Illustrated chart covering what a Unified Communication Certificate (UCC) is.](https://us.norton.com/content/dam/blogs/images/norton/am/what-is-an-ssl-certificate-07.png)
## 为什么网站需要SSL证书

网络攻击是一种令人惊讶的常见威胁。[**网络安全统计**](https://us.norton.com/blog/emerging-threats/cybersecurity-statistics) **表明超过一半的互联网用户在过去一年中直接经历过网络犯罪。这意味着人们比以往任何时候都更需要保护自己的数据，并且[了解他们访问的网站是否安全](https://us.norton.com/blog/how-to/how-to-know-if-a-website-is-safe).

SSL证书是更安全浏览墙中的一块重要砖块，因为它让您知道您在网站上共享的信息受到加密保护。如果没有加密，从用户传输到站点的数据就不会受到保护，从而使您面临[中间人攻击](https://us.norton.com/blog/wifi/what-is-a-man-in-the-middle-attack)以及其他类型的网络攻击。

如果您是企业主或负责组织的网站，SSL 可以为您的客户和其他网站用户提供额外的保护层，让您高枕无忧。虽然这些SSL安全证书之一不足以阻止或阻止所有形式的[黑客](https://us.norton.com/blog/emerging-threats/what-is-a-hacker)或者信息盗窃本身，这是一个重要的步骤，可以帮助保护组织的数据和公众认知。

![Illustrated chart explaining a few of the main reasons why websites should have SSL certificates.](https://us.norton.com/content/dam/blogs/images/norton/am/what-is-an-ssl-certificate-08.png)

## 如何获取SSL证书

如果您拥有一个网站，您可能想知道如何利用SSL证书附带的额外信任。继续阅读以了解如何为您的网站获取一个。

### 1. 确认您需要的信息

每个证书颁发机构都有不同的要求，您需要先满足这些要求，然后才能发出 SSL。考虑到这一点，无论您是申请 EV、OV 还是 DV SSL，**每个 CA 都会要求您提供一些标准信息**，包括：

- 证明您拥有该域名
- 证明您是申请SSL的人

随着验证级别的提高，您需要提供更多信息。

### 2. 选择从何处获取 SSL 证书

**全球有数十个不同的证书颁发机构验证和提供网站 SSL。** 在搜索 CA 时，请考虑查找符合以下条件的 CA：

- 提供您需要的 SSL 类型
- 满足颁发证书所需的所有最低标准
- 具有清晰的定价结构
- 为您提供所需的客户支持

### 3.考虑SSL的成本

虽然大多数SSL证书要求您向证书颁发机构付费，但也有免费选项。通常，免费的SSL仅限于域验证证书，这意味着对于具有多个域和子域的大型实体或组织来说，它们并不理想（甚至可能可用）。

- **如果您有个人网站或小型企业的简单网站**，那么免费的SSL证书可能会为您提供所需的一切。
- **适用于处理敏感问题的大型企业或组织**[**个人信息**](https://us.norton.com/blog/privacy/what-personal-information-should-you-safeguard) **与财务或医疗数据一样**，与可以跨一个或多个服务器颁发涵盖多个域或子域的证书的 CA 通常是一个好主意。

### 4.掌握SSL证书续订情况

对于小企业主或博主来说，如果您的SSL证书过期并且您忘记续订几天，这可能不是一个大问题。

但是，如果您是一个每天处理大量外部或内部流量的大品牌，那么失效的证书可能会花费您很多钱并损害您的声誉。

### 什么是证书颁发机构？

证书颁发机构 （CA） 是颁发 SSL 证书的组织。CA 的工作是使用证书验证站点所有者的身份，然后存储和签署这些证书。他们必须满足严格的准则，以便设备、操作系统和浏览器信任他们的证书。

### 什么是公钥/私钥对？

公钥和私钥使授权用户能够发送和接收加密数据。

公钥由证书生成，任何使用站点的人都可以使用。私钥是秘密的，在建立连接后由用户的浏览器创建。由于加密数据是在证书所有者和用户之间发送的，因此除了公钥的颁发者和私钥的持有者之外，任何人都无法读取数据。

### SSL证书的有效期是多久？

大多数SSL证书的有效期为一年，但有些CA提供更长的覆盖期限，例如两到三年。

### 什么是安全证书？

安全证书（包括 SSL 或 TLS 证书）是一个小型数据文件，用于向用户证明站点的真实性以及[创建安全连接](https://us.norton.com/blog/privacy/ssl-vpn)使用加密。

### SSL证书可以在多台服务器上使用吗？

是的，使用多域证书，您可以在多个服务器上使用一个 SSL。

### HTTPS 是如何工作的？

![alt text](<../assets/img/Pasted image 20240131171202.png>)