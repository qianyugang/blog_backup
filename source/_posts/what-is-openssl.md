---
title: openSSL 的加密方式与证书制作
toc: true
categories: 与代码
tags: 
	- openSSL
date: 2019-04-01
---

## openSSL是什么

OpenSSL项目是安全套接字层（secure sockets layer，SSL）和传输层安全（transport layer security，TLS）协议的一个实现，是大家共同努力开发出的代码可靠、功能齐全、商业级别的开源工具集。除了OpenSSL之外的其他TLS实现方式，如GnuTLS、Mozilla的网络安全服务（NSS）和Windows平台的TLS。

项目由遍布世界的志愿者所组成的社区进行管理，他们通过互联网进行沟通、计划和开发OpenSSL工具集以及相关的文档。现在几乎所有的服务器软件和很多客户端软件都在使用OpenSSL，其中基于命令行的工具是进行密钥、证书管理以及测试最常用到的软件了。

## 公钥算法与私钥算法

### 私钥算法

私钥加密算法，又称 对称加密算法，因为这种算法解密密钥和加密密钥是相同的。也正因为同一密钥既用于加密又用于解密，所以这个密钥是不能公开的。常见的有《DES加密算法》、《AES加密算法》。

### 公钥算法

公钥加密算法，也就是 非对称加密算法，这种算法加密和解密的密码不一样，一个是公钥，另一个是私钥：

- 公钥和私钥成对出现
- 公开的密钥叫公钥，只有自己知道的叫私钥
- 用公钥加密的数据只有对应的私钥可以解密
- 用私钥加密的数据只有对应的公钥可以解密
- 如果可以用公钥解密，则必然是对应的私钥加的密
- 如果可以用私钥解密，则必然是对应的公钥加的密
- 公钥和私钥是相对的，两者本身并没有规定哪一个必须是公钥或私钥。


2. 当该用户发送文件时，用私钥签名，别人用他给的公钥解密，可以保证该信息是由他发送的。即数字签名。

3. 当该用户接受文件时，别人用他的公钥加密，他用私钥解密，可以保证该信息只能由他看到。即安全传输。  

- 加密: 公钥用于对数据进行加密，私钥用于对数据进行解密  
- 签名: 私钥用于对数据进行签名，公钥用于对签名进行验证

由于在非对称算法中，公钥加密的数据必须用对应的私钥才能解密，而私钥又只有接收方自己知道，这样就保证了数据传输的安全性。

![](https://img-blog.csdn.net/20150502122610368)

### openssl生成公私钥对文件进行加解密

**生成一个私钥**   

```
openssl genrsa -out rsa-private.key 1024
- genrsa：生成RSA参数
- -out：输出文件
```

**由私钥生成一个公钥**     

```
openssl rsa -in rsa-private.key -pubout -out rsa-public.key
```

- -pubout: 输出公钥文件

**随便新建一个文件**     

```
echo "this is a test message balbalabalabalbalabala" > test.log
```

**使用公钥加密这个文件**     

```
openssl rsautl -encrypt -inkey rsa-public.key -pubin -in test.log -out encrypt-test.log
- rsautl：用于完成RSA签名、验证、加密和解密功能
- -pubin：检查待处理文件是否为公钥文件
```

**使用私钥解密这个文件**     

```
openssl rsautl -decrypt -inkey rsa-private.key -in encode-test.log -out decrypt-test.log
```

## 使用openssl获取证书

显示所有证书
```
openssl s_client -connect www.baidu.com:443 -showcerts
```

- `Certificate chain` 证书链
- `s:`证书本身主体
- `i:`证书颁发机构

服务器查看并解析证书 

```
openssl x509 -in baidu.pem -text -noout
```

- [X.509](https://zh.wikipedia.org/wiki/X.509) 是密码学里公钥证书的格式标准。

## 使用openssl制作证书

### 第一步：创建非对称密钥对

可以选择 RSA DSA ECDSA 的都可以，其中RSA 在浏览器的兼容是最好的。[三者区别](https://blog.csdn.net/sszgg2006/article/details/25478269)

- **rsa key**  

	和 openssl生成公私钥对文件进行加解密 中一样

- **dsa key**  

    ```
    openssl dsaparam -genkey 2048 | openssl dsa -out dsa.key
    - dsaparam：本指令用来生成和操作dsa参数。
    -  -genkey：生成dsa密钥。
    ```

- **ec key**  

    ```
    openssl ecparam -genkey -name secp256r1 | openssl ec -out ec.key
    ```

###  第二步：创建证书签名请求 CSR 文件

> 注：Certificate Signing Request,即证书签名请求，这个并不是证书，而是向权威证书颁发机构获得签名证书的申请，其核心内容是一个公钥(当然还附带了一些别的信息)，在生成这个申请的时候，同时也会生成一个私钥,私钥要自己保管好。做过iOS APP的朋友都应该知道是怎么向苹果申请开发者证书的吧。

新建csr  
```
openssl req -new -key rsa-private.key -out rsa.csr
```

查看csr  
```
openssl req -text -in rsa.csr -noout
```

### 第三步： 自签名证书 （提交）

 生成 csr 文件之后，需要提交到权威的 CA 数字认证中心 VeriSign 后者其他，这里是讲自己认证。

```
openssl x509 -req -days 365 -in rsa.csr -signkey rsa-private.key -out rsa_yugang.cer
```

查看证书

```
openssl x509 -noout -text -in rsa_yugang.cer
```

## 证书格式

- .pem – Privacy Enhanced Mail,打开看文本格式,以"-----BEGIN..."开头, "-----END..."结尾,内容是BASE64编码.
- .cer, .crt, .der – 通常是DER二进制格式的，但Base64编码后也很常见。

## 相关资料

- [OpenSSL 的使用详解](http://www.178linux.com/48764)
- [OpenSSL开发学习总结](https://juejin.im/entry/59f6c30ef265da43085d4e82)
- [openssl 官网](https://www.openssl.org)
- [OpenSSL 与 SSL 数字证书概念贴](http://seanlook.com/2015/01/15/openssl-certificate-encryption/)
- [OPENSSL编程入门学习](http://www.cnblogs.com/LittleHann/p/3741907.html)
- [OpenSSL使用指南](https://jin-yang.github.io/reference/linux/OpenSSL.pdf)
- [OpenSSL攻略](http://www.ituring.com.cn/book/download/338e1e55-fd94-4ac3-9e21-e0bf04984b3f)
