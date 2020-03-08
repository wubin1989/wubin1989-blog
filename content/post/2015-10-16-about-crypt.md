---
title:       "加密解密算法小结"
subtitle:    ""
description: ""
date:        2015-10-16
author:      ""
image:       ""
tags:        ["backend"]
categories:  ["Tech"]
archives:    "2015"
---

## 单向散列算法
也称为Hash（哈希）算法。是一种将任意长度的消息压缩到某一固定长度（消息摘要）的函数（该过程不可逆）。Hash函数可用于数字签名、消息的完整性检测、消息起源的认证检测等。常见的散列算法有MD5、SHA、RIPE-MD、HAVAL、N-Hash、Tiger等。
### MD5算法
MD5消息摘要算法（Message Digest Algorithm）。对输入的任意长度的消息进行运算，产生一个128位的消息摘要。
用到4个常量初始化数据：A=01234567h，B=89abcdefh，C=fedcba98h，D=76543210h。可以使用其他数据作为变形。
### SHA算法
安全散列算法（Secure Hash Algorithm）简称SHA。有SHA-1、SHA-256、SHA-384、SHA-512几种，分别产生160位、256位、384位和512位的散列值。
#### SHA-1算法
是一种主流的散列加密算法，设计时基于和MD4相同的原理，并模仿了该算法。消息分组和填充方式与MD5相同。也用到了一些常量做初始化数据。
总结，随着密码分析技术的发展，现有的散列算法都是不安全的。如SHA-160、MD5、RIPEMD、HAVAL、Tiger在某些条件下能构造出碰撞。建议选择SHA-256//384/512，或者Whirlpool。如果在解密时碰到Hash算法，一般只需根据每种Hash算法的特征搞清楚具体哪一种Hash算法以及是否为某种算法的变形，继而通过该Hash的源代码即可破解。

## 对称加密算法
加密密钥和解密密钥是完全相同的。其安全性依赖于：1.加密算法足够强；2.密钥的秘密性。
常见的对称分组加密算法有DES（Data Encryption Standard）、IDEA（International Data Encryption Algorithm）、AES（Advanced Encryption Standard）、BlowFish、Twofish等。
### RC4流密码
现今最为流行的流密码，应用于SSL（Secure Sockes Layer）、WEP。RC4生成一种称为密钥流的伪随机流，它同明文通过异或操作相混合以达到加密的目的。解密时，同密文进行异或操作。其密钥流的生成有两部分组成：KSA（the Key-Scheduling Algorithm)和PRGA(the Pseudo-Random Generation Algorithm)。由于RC4算法加密采用XOR，所以一旦密钥序列出现重复，密文就有可能被破解。推荐使用RC4算法时，必须对加密密钥进行测试，判断其是否为弱密钥。
### TEA算法
TEA（Tiny Encryption Algorithm）算法。分组长度为64位，密钥长度为128位。采用Feistel网络。其作者推荐使用32次循环加密，即64轮。TEA算法简单易懂，容易实现。但存在很大的缺陷，如相关密钥攻击。由此提出一些改进算法，如XTEA。
### IDEA算法
IDEA（International Data Encryption Algorithm）国际数据加密算法。分组密码IDEA明文和密文的分组长度为64位，密钥长度为128位。该算法的特点是使用了3种不同的代数群上的操作。IDEA共使用52个16位的子密钥，由输入的128位密钥生成。加密过程由8个相同的加密步骤（加密轮函数）和一个输出变换组成。而解密过程与加密过程相同。解密与加密唯一不同的地方就是使用不同的子密钥。首先，解密所用的52个子密钥是加密的子密钥的相应于不同操作运算的逆元，其次，解密时子密钥必须以相反的顺序使用。
### BlowFish算法
BlowFish算法是一个64位分组及可变密钥长度的分组密码算法，该算法是非专利的。BlowFish算法基于Feistel网络（替换/置换网络的典型代表），加密函数迭代执行16轮。分组长度为64位（bit），密钥长度可以从32位到448位。算法由两部分组成：密钥扩展部分和数据加密部分。密钥扩展部分将最长为448位的密钥转化为共4168字节长度的子密钥数组。其中，数据加密由一个16轮的Feistel网络完成。每一轮由一个密钥相关置换和一个密钥与数据相关的替换组成。
### AES算法
AES（Advanced Encryption Standard，高级加密标准），用于替代DES成为新一代的加密标准。具有128比特的分组长度，并支持128、192和256比特的密钥长度，可在全世界范围内免费得到。其前身为Rijndael（读作：Rain Doll）。Rijndael算法与AES的唯一区别在于各自所支持的分组长度和密码密钥长度的反胃不同。Rijndael是具有可变分组长度和可变密钥长度的分组密码，其分组长度和密钥长度均可独立地设定为32比特的任意倍数，最小值128bit，最大256bit。而AES将分组长度固定为128位，而且仅支持128、192和256位的密钥长度，分别称为AES-128，AES-192，AES-256。

## 公开密钥加密算法
又称为非对称加密算法（Asymmetric Key Cryptography），即使用公钥（Public Key）加密，使用私钥（Private Key）解密。
### RSA算法
RSA是第一个既能用于数据加密也能用于数字签名的算法，易于理解和操作，应用广泛。RSA的安全性依赖于大整数因子分解。目前来看，攻击RSA算法最有效的方法便是分解模n。一般认为RSA需要1024位或更长的魔术才有安全保障。
### ElGamal公钥算法
其完全依赖于在有限域上计算离散对数的困难性。ElGamal的一个不足之处是密文的长度是明文的两倍。而另一种签名算法，Schnorr签名系统的密文比较短，这是由其系统内的单向散列函数决定的。
### DSA数字签名算法
是在借鉴了ElGamal及Schnorr签名算法的基础上，公布的数字签名标准（Digital Signature Standard），该标准采用的算法为DSA（Digital Signature Algorithm）。其安全性同样基于有限域的离散对数问题。目前DSA的应用越来越广泛。
### 椭圆曲线密码编码学（Elliptic Curve Cryptography）
相比RSA等公钥算法，使用较短的密钥长度而能得到相同程度的安全性。预测未来ECC（Elliptic Curve Cryptography）将会取代RSA成为主流的密钥算法。

## 其他算法
### CRC32算法
CRC（Cyclic Redundancy Checksum 或者 Cyclic Redundancy Check），是对数据的校验值，中文为“循环冗余校验码”，常用语校验数据完整性。最常见的CRC是CRC32，即数据校验值为32位。CRC32代码量小，容易理解，所以目前应用十分广泛。但同时CRC32并不是一个安全的加密算法。如果需要更安全的完整性校验算法，建议使用数字签名技术。
### Base64编码
Base64编码是将二进制数据编码为可现实的字母和数字，用于传送图形、声音和传真等非文本数据。常用于MIME电子邮件格式中。其使用含有65个字符的ASCII字符集（第65个字符为“=”，用于对字符串的特殊处理过程），并用6个进制位表示一个可显示字符。
把数据编码为Base64，将第一个字节放置于24位缓冲区的高8位，第二个字节放置于中间的8位，第三个字节放置于低8位。如果是少于3个字节的数据，相应的缓冲区置0。然后对24位缓冲区以6位为一组作为索引，高位优先，从字符串“ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghigklmnopqrstuvwxyz0123456789+/”中取出相应的元素作为输出。如果仅有一个或两个字节输入，那么只使用输出的两个或三个字符，其余的用“=”填充。
解码过程是编码的逆过程。首先得到Base64字符串的每个字符在Base64码表中的索引，然后将这些索引的二进制连接起来，重新以8位为一组进行分组，即可得到源码。
Base24码表：
BCDFGHJKMPQRTVWXY2346789
Base32码表：
ABCDEFGHIJKLMNOPQRSTUVWXYZ234567
Base60码表：
0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghigklmnopqrstuvwx
Windows产品序列号就是使用Base24编码。实际使用时，码表会和标准码表不一样，但原理相同。

## 常见的加密库
### Miracl大数运算库
Miracl（Multiprecision Integer and Rational Arithmetic C/C++ Library），多精度整数和有理数算术运算C/C++库。它是一个大数库，实现了设计使用大数的加密技术的最基本的函数。支持RSA公钥系统、Diffie-Hellman密钥交换、DSA数字签名系统及基于GF（p）和GF（2m）的椭圆曲线加密系统。其提供了C和C++两种接口，使用方便，速度快，开源。
### FGInt
FGInt（Fast Gigantic Integers），是用于Delphi的一种可以实现常见的公钥加密系统的库。
### freeLIP
最初设计用于进行RSA-129挑战大数计算的大数库，采用2的30次方进制来表示大数，速度不及miracl。
### Crypto++
一个实现了相当数量的加密算法的加密库。使用了C++的高级语法，文档较少，不易上手。
### LibTomCrypt
一款相当不错的加密算法库。包括了常见的散列算法、对称算法及公钥加密系统。接口友好，非常适合C程序员使用。
### GMP
全称GNU Multiple Precision Arithmetic Library。其核心采用汇编实现，速度非常快，超过miracl，常用来实现大整数因子的分解。
### OpenSSL
主要用于网络安全，也包含了一些加密算法的实现。如对称算法中的BlowFish、IDEA、DES、CAST，公钥中的RSA、DSA，散列中的MD5、RIPEMD、SHA等。
### DCP和DEC
DCP全称Delphi Cryptography Package，DEC全称Delphi Encryption Compendium，都是用于Delphi的一种加密算法库。这两个算法库实现了大部分常见的散列算法及对称算法，使用方便。
### Microsoft Crypto API
是微软为了方便程序员在软件中进行数字签名、数据加密的开发而提供的一套加密系统。接口友好方便。
### NTL
是一个可以用于数论相关计算的库。提供了非常友好的C++接口，用于实现有符号的、算术整数的运算，以及向量、矩阵、基于有限域和整数的多项式运算。在密码学中，有限域的应用相当广泛，如AES、twofish、ECC等都涉及有限域。