---
layout:     post
title:      "What is digital certificate"
subtitle:   ""
date:       2020-04-21 16:21:00
author:     "Yuanbo"
header-img: "img/home-bg-o.jpg"
catalog:    true

tags:
    - Tech
    - Interview
    
---


## What is digital certificate


数字证书就是网络通讯中标志通讯各方身份信息的一系列数据，其作用类似于现实生活中的身份证。它是由一个权威机构发行的，人们可以在互联网上用它来识别对方的身份。

证书的格式遵循ITUTX.509国际标准

一个标准的X.509数字证书包含以下一些内容：

        证书的版本信息；
        
        证书的序列号，每个证书都有一个唯一的证书序列号；
        
        证书所使用的签名算法；
        
        证书的发行机构名称，命名规则一般采用X.500格式；
        
        证书的有效期，现在通用的证书一般采用UTC时间格式，它的计时范围为1950-2049；
        
        证书所有人的名称，命名规则一般采用X.500格式；
        
        证书所有人的公开密钥；
        
        证书发行者对证书的签名。

### 1.密码学

在密码学中，有一个五元组：{明文、密文、密钥、加密算法、解密算法}，对应的加密方案称为密码体制（或密码）。

明文：是作为加密输入的原始信息，即消息的原始形式所有可能明文的有限集称为明文空间。

密文：是明文经加密变换后的结果，即消息被加密处理后的形式，所有可能密文的有限集称为密文空间。

密钥：是参与密码变换的参数，一切可能的密钥构成的有限集称为密钥空间。

加密算法：是将明文变换为密文的变换函数，相应的变换过程称为加密，即编码的过程。

解密算法：是将密文恢复为明文的变换函数，相应的变换过程称为解密，即解码的过程。

**对称算法：**

加密和解密使用同一密钥

主要算法包括：DES、3DES、RC2、RC4

加/解密速度快，但密钥分发问题严重



**非对称算法**：

不需要首先共享秘密

两个密钥同时产生：一把密钥（公钥）是公开的，任何人都可以得到。另一把密钥（私人密钥）只有密钥的拥有者知道。

用其中一把密钥（公钥或者是私钥）加密的信息，只能用另一把打开。

RSA算法，ECC（椭圆曲线）算法

由已知的公钥几乎不可能推导出私钥

强度依赖于密钥的长度

很慢-不适合加密大数据

公钥可以广泛地、开放地分发

用于加密，签名/校验，密钥交换

**数字摘要：**

把可变输入长度串转换成固定长度输出串的一种函数。称输出串为输入串的指纹，或者数字摘要；

不同明文的摘要相同的概要是非常非常小的；而同样明文其摘要必定一致；

明文的长度不固定，摘要的长度固定。

没有密钥

不可回溯

典型算法：MD5,SHA-1

用于完整性检测，产生数字签名


**最好的解决方案：**

- 使用对称算法加密大的数据

- 每次加密先产生新的随机密钥

- 使用非对称算法交换随机产生的密钥

- 用非对称算法做数字签名（对散列值即摘要做数字签名）

`四条基本规则`

- 先签名，然后加密

- 加密之前先压缩

- 用你自己的私钥做签名

- 用接收者的公钥做加密


## 数字签名

数字签名采用公钥体制（PKI），即利用一对互相匹配的密钥进行加密、解密。每个用户自己设定一把特定的仅为本人所知的私有密钥(私钥)，用它进行解密和签名；同时设定一把公共密钥(公钥)并由本人公开，为一组用户所共享，用于加密和验证签名。当发送一份保密文件时，发送方使用接收方的公钥对数据加密，而接收方则使用自己的私钥解密，这样信息就可以安全无误地到达目的地了。通过这种手段保证加密过程是一个不可逆过程，即只有用私有密钥才能解密。在公开密钥密码体制中，常用的一种是RSA体制。其数学原理是将一个大数分解成两个质数的乘积，加密和解密用的是两个不同的密钥。即使已知明文、密文和加密密钥(公开密钥)，想要推导出解密密钥(私密密钥)，在计算上是不可能的。按现在的计算机技术水平，要破解目前采用的1024位RSA密钥，需要上千年的计算时间。公开密钥技术解决了密钥发布的管理问题，商户可以公开其公开密钥，而保留其私有密钥。购物者可以用人人皆知的公开密钥对发送的信息进行加密，安全地传送给商户，然后由商户用自己的私有密钥进行解密。

用户也可以采用自己的私钥对信息加以处理，由于密钥仅为本人所有，这样就产生了别人无法生成的文件，也就形成了数字签名。采用数字签名，能够确认以下两点：

(1)保证信息是由签名者自己签名发送的，签名者不能否认或难以否认；

(2)保证信息自签发后到收到为止未曾作过任何修改，签发的文件是真实文件。

### 数字签名具体做法是：

(1)将报文按双方约定的HASH算法计算得到一个固定位数的报文摘要。在数学上保证：只要改动报文中任何一位，重新计算出的报文摘要值就会与原先的值不相符。这样就保证了报文的不可更改性。

(2)将该报文摘要值用发送者的私人密钥加密，然后连同原报文一起发送给接收者，而产生的报文即称数字签名。这样就保证了签发者不可否认性。

(3)接收方收到数字签名后，用同样的HASH算法对报文计算摘要值，然后与用发送者的公开密钥进行解密解开的报文摘要值相比较。如相等则说明报文确实来自所称的发送者。


### 数字证书是如何生成的？

数字证书是数字证书在一个身份和该身份的持有者所拥有的公/私钥对之间建立了一种联系，由认证中心（CA）或者认证中心的下级认证中心颁发的。根证书是认证中心与用户建立信任关系的基础。在用户使用数字证书之前必须首先下载和安装。


数字证书颁发过程如下：用户产生了自己的密钥对，并将公共密钥及部分个人身份信息(称作P10请求)传送给一家认证中心。认证中心在核实身份后，将执行一些必要的步骤，以确信请求确实由用户发送而来，然后，认证中心将发给用户一个数字证书，该证书内附了用户和他的密钥等信息，同时还附有对认证中心公共密钥加以确认的数字证书。当用户想证明其公开密钥的合法性时，就可以提供这一数字证书。


链接：https://www.jianshu.com/p/42bf7c4d6ab8

### END

