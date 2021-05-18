---
layout: post
title:  "Understanding Cryptography 读书笔记"
date:   2021-05-12 17:03:01 +0100
categories: Cryptography
---

最近在入门密码学，从入门资源上看觉得《Understanding Cryptography》比较适合，用这篇博文来
记录一下学习过程。

# Chapter 1 简介密码学

这一章主要有三点需要了解：
- 不要用未公开的加密算法！ 历史证明，所有私密的加密算法一旦被获取/公开后很快便被破解，有效的加密算法是指攻击者在了解除密钥外所有加密算法的信息后，仍然无法破解/没有足够资源加密信息的算法。
- 取余运算在密码学中经常使用，以此来获得“环”的效果。三条横线的等号表示等号两边的数对于N(mod N)取余的话，余数相等。
- 密钥长度在112-128bit目前被认为是可靠的，256bit目前是最可靠的。

# Chapter 2 Stream Cipher流加密

对称加密目前可以分为两种：Stream Cipher流加密和Block Cipher块加密/分组加密。流加密是将明文按bit加密，每个bit都有一个对应的密钥，可以理解为密钥的长度或者伪密钥的长度需要跟明文等长。 伪密钥是根据密钥产生的密钥。 块加密是将明文分成数据块，比如64bit为一个块，不够的要补长，然后将每个块用一个密钥加密。

要点:
- 异或运算XOR经常用在加密算法中,异或运算相当于`x+s mod 2`将明文与密钥的和对2取余。
- 流加密没有块加密流行(除了RC4?)，但有时流加密需要占用更少的资源，比较受嵌入式系统等资源受限的环境。
- One-Time-Pad 是安全的算法却不易实际应用，因为密钥的长度要和明文相同**且只能使用一次**，可以通过随机数基于密钥产生伪密钥，需要注意的是随机数生成器必须是专门为密码学设计的随机数生成器，一般标准库中的随机数函数可以预测，因此不能用。
- LFSRs Linear feedback shift registers有些类似于斐波那契数列，不同在于s1+s2 = s4。单个LFSR可以破解因为根据LFSR产生的结果是有限的，而且具有循环性。但是多个LFSRs合理地组合到一起就能具有很高的安全性，比如Trivium。

# Chapter 3 DES Data Encryption Standard

- DES是分组加密算法，在1970年代相当流行，但是随着计算机科技的发展，目前已经可以通过遍历所有密钥手段暴力破解，因为DES密钥长度只有56bit。
- DES 是 AES Advanced Encryption Standard的前身，目前也有很多高效的块加密算法，如3DES， Twofishes, Mars等。
- DES 基于Feistel对称加密结构，即将明文分成左右两部分，将左部分与轮换的密钥进行运算，运算函数f是关键，而运算函数中的S-box(替换表)是运算函数的核心，运算完后将结果作为下一轮的右部分，下一轮的左部分为上一步的右部分。

# Chapter 4 AES Advanced Encryption Standard

AES 目前应用的非常广泛，与DES不同的是，AES没有使用Feistel对称加密结构，所有的明文在每一轮都会被加密。**非常值得用C++实现一下**，以便更好的了解。

- AES 的基本运算基于Galois field算法，能够提供非常高的diffusion和confusion
- AES 应用于TLS， IPsec, WiFi等，将会在长时间内被广泛应用。
- AES 在软件和硬件的效率都很高。

# Chapter 5 Operation Modes for Block Cipher

很多运算模式可以应用到分组加密算法来使得算法更加安全，运算模式相当于一个框架，而加密算法如AES等是这个框架中的一个模块。

- 目前一共有八种NIST承认的运算模式， 5个用来加强保密性（ECB,CBC,CFB,OFB,CTR), 一个用来身份认证（CMAC）, 两个结合了保密性和身份认证（CCM, GCM）。

- 双重加密即串联加密并不能有效提高对于暴力破解的抗性，但是三重串联加密可以，3DES。
- Key whitening 扩大了DES的密钥长度而且并没有造成很大的计算负担，但key whitening并不能提高对于分析性攻击的抗性，它只能提高对于暴力破解的抗性。

# Chapter 6 Intro to Public-key Cryptography
在加密解密的流程中，只要接收方有密钥可以解密就可以了，任何人都可以用公开的密钥加密，本着这个想法，公有-私有密钥结构诞生了。

- Public-key algorithms很慢，所以一般用来加密密钥、身份认证等，具体数据还是用对称算法比较高效。
- 目前有三类广泛应用的非对称加密算法： Integer factorization(eg. RSA), Discrete logarithm, Elliptic curves。
- 费马最小定理和欧拉定理在RSA算法中很常用，确定了如何计算同余的倒数。
