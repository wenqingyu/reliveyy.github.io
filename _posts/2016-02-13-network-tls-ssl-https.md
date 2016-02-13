---
layout: post
title: 网络协议：TLS/SSL，HTTPS
categories: network
comments: true
tags: ssl, tls, https
---

* TLS 全称 Transport Layer Security 维基百科上归类为应用层协议
* SSL 全称 Secure Sockets Layer，后继者是 TLS 协议
* SSL 最早是 NetScape 公司开发的一个非公开的安全协议，用于实现浏览器 HTTPS 访问
* 1999年，互联网标准化组织ISOC 接替 NetScape 公司，发布了 SSL 的升级版 TLS 1.0 
* 现在 HTTPS 普遍流行的是 TLS 1.2 在 2011 年的修订版（RFC 6176）
* HTTPS 就是依赖 TLS/SSL 协议来实现 HTTP 数据的加密传输
* TLS/SSL 在加密传输数据之前，需要通信双方先协商好后面加密传输过程中使用何种加密算法，根据需要交换必要的凭证。
* 2014 年的 “心脏出血” 漏洞不是协议的问题，而是 OpenSSL 实现的问题。

| 协议 | 年份 | 公开 | 
|-----|------|-----|
| SSL 1.0 | 1994 | 否 |
| SSL 2.0 | 1995 | 否 |
| SSL 3.0 | 1996 | 是 |
| TLS 1.0 | 1999 | 是 | 
| TLS 1.1 | 2006 | 是 |
| TLS 1.2 | 2008 | 是 |
| TLS 1.3 | 草案 | 是 |

## 非对称加密（公开密钥加密）
**非对称加密**（asymmetric cryptography）也称为**公开密钥加密**（public-key cryptography），是一种密码学算法的类型，这种类型的算法总是基于一个解答耗时的数学问题而构建。例如 RSA 算法根据的就是，从两个大素数的乘积中分解出原来的两个素数比较困难。这种类型算法中都包括公钥和私钥，公钥加密的内容只有配对的私钥才能解密（加密传输）；同样私钥签名的内容使用配对的公钥便能验证其正确性和完整性（数字签名）。

非对称加密在计算机领域实际运用过程中，最常见的是各种证书，例如主流操作系统内都会有一堆信任的根证书。**证书**就是对公钥的一种封装。我们可以使用对方的证书，给对方传送加密信息（只有证书持有者才能解密，因为他有私钥）；同样我们也能用对方的证书来验证对方传递过来的信息是否真实完整（前提是对方给信息做了数字签名）


## 术语

**X.509** 是一种证书标准，规定了证书里应该有哪些内容。X.509 证书可以有不同的编码格式，常见的有 **PEM** 和 **DER** 两种。

**PEM**（Privacy Enhanced Mail）是文本文件，以 `-----BEGIN ... -----` 开头，`-----END ... -----` 结尾，内容用 *Base64* 编码。Apache 和 *NIX 服务器偏向于使用这种编码格式。

**DER**（Distinguished Encoding Rules）是二进制文件，Java 和 Windows 服务器偏向于使用这种编码格式。

注意：上面两种编码格式的文件扩展名不一定就叫做 `.pem` 或者 `.der`

**【证书文件】**的扩展名有 `.crt` 常见于类 UNIX 系统，`.cer` 常见于 Windows 系统。都是英文单词证书 *certificate* 的缩写。

**【证书请求文件】**的扩展名是 `.csr`（Certificate Signing Request）和 pem 文件类似，也是文本文件，但是它不是证书，它是申请证书的单位向 CA 请求颁发证书前的一个请求文件，里面包含了申请单位的相关信息。

**【证书库文件】** PKCS #12 的扩展名是 `.pfx` 或者 `.p12`，在 Windows 下面比较常见，这样一个文件中包含多个证书。在 Java 环境下常用扩展名 `.jks`（Java KeyStore），利用 Java 自带的工具 *keytool*，可以将 pfx 文件转换为 jks，当然 *keytool* 也能直接生成 jks 文件。证书库文件一般都可以设置一个提取密码。

**PKI**（Public Key Infrastructure）就是围绕公钥建设的一整套安全信任体制，其中最核心的就是 **CA**（Certificate Authority），它们是 PKI 信任链体系中的源头。可以参考[火狐浏览器根证书](https://mozillacaprogram.secure.force.com/CA/IncludedCACertificateReport)，[Mac OS X EI Capitan 的根证书](https://support.apple.com/en-us/HT205204)

Java 中 **keystore** 是一种标准，实现有 Sun/Oracle 的 jks，BouncyCastle 的 bks，**truststore** 指的是证书文件（cer）。`jre/lib/security/cacerts` 能看到 JRE 自带的 CA 根证书。JVM 启动的时候可以添加这些 SSL 配置选项

```
-Djavax.net.ssl.keyStore
-Djavax.net.ssl.keyStorePassword
-Djavax.net.ssl.trustStore
-Djavax.net.ssl.trustStorePassword
```


## OpenSSL 使用简介

OpenSSL 是一个 TLS/SSL 协议的具体实现，[项目主页](https://www.openssl.org/)

```sh
# 查看版本
openssl version
# 查看 PEM 格式证书的信息
openssl x509 -in certificate.pem -text -noout
# 查看 DER 格式证书的信息，加 -inform der 
openssl x509 -in certificate.der -text -noout -inform der
# 查看 PEM 格式的密钥
openssl rsa -in mykey.key -text -noout
# 查看 DER 格式的密钥，加 -inform der 
openssl rsa -in mykey.key -text -noout -inform der
# 查看 PEM 格式 的 CSR 文件
openssl req -noout -text -in my.csr
# 生成 CSR 文件
openssl req -newkey rsa:2048 -new -nodes -keyout my.key -out my.csr
# 生成自签证书
openssl req -newkey rsa:2048 -new -nodes -x509 -days 3650 -keyout key.pem -out cert.pem
# 在生成证书的过程中会要你填一堆的东西，其实真正要填的只有 Common Name
# 通常填写你服务器的域名或者你服务器的IP地址，其它都可以留空

# 证书编码转换：PEM -> DER 
openssl x509 -in cert.crt -outform der -out cert.der
# 证书编码转换：DER -> PEM 
openssl x509 -in cert.crt -inform der -outform pem -out cert.pem
# 密钥（rsa），请求（req）都是可以转换编码的

# 将 DER 格式 的 pfx 文件转换成 PEM 格式的文本文件
# 可能会提示你输入 pfx 文件提取密码
openssl pkcs12 -in for-iis.pfx -out for-iis.pem -nodes
# 生成 pfx 文件，其中CACert.crt是CA(权威证书颁发机构)的根证书
openssl pkcs12 -export -out certificate.pfx -inkey privateKey.key -in certificate.crt -certfile CACert.crt

# 计算信息摘要 sha1
openssl dgst -sha1 data.txt

# 加密解密
openssl enc -des -in data.txt -e -out data.txt.des
openssl enc -des -in data.txt.des -d -out data.txt

# 生成 8 个字节长的随机数据并用十六进制显示
openssl rand -hex 8
```

## Java Keytool 使用简介

```sh
# 生成 foo.jks
keytool -keystore foo.jks -genkeypair -alias foo -dname 'CN=foo.example.com,L=Melbourne,ST=Victoria,C=AU'
# 将 foo.jks 转换成 foo.p12
keytool -importkeystore -srckeystore foo.jks -destkeystore foo.p12 -srcstoretype jks -deststoretype pkcs12
# 将 foo.p12 转换成 foo.pem
# 所以 pem 格式的文本文件也可以存储多个证书
openssl pkcs12 -in foo.p12 -out foo.pem
```

## 参考

1. [证书相关的术语](http://blog.phpdr.net/%E8%AF%81%E4%B9%A6%E7%9B%B8%E5%85%B3%E7%9A%84%E6%9C%AF%E8%AF%AD.html)
2. [Converting a Java Keystore into PEM Format](http://stackoverflow.com/questions/652916/converting-a-java-keystore-into-pem-format)
