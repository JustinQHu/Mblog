---
title: "如何创建自签名SSL证书"  
date: 2024-01-28T14:09:39-05:00  
draft: false  
categories: ['工程','技术']  
tags: ['工程','技术', '安全', '网站开发', 'SSL', 'SSL证书',  '自签名SSL证书', HTTPS']  
---

## SSL证书

在当今网络犯罪变得越来越普遍的数字环境中，保护您的网站或应用程序变得越来越重要。

实现安全性的一种常见方法是通过加密。 对称和非对称加密在密码学领域已得到深入研究，并广泛应用于业界以帮助保护数字系统。 SSL 证书建立在非对称加密之上，通常用于通过确保客户端和 Web 服务器之间传输的数据经过加密来保护网站安全，从而保证数据的私密性和安全性。

企业、组织和开发人员通常从充当互联网信任源的公共CA（证书颁发机构）获取SSL证书。 在某些情况下，例如测试、学习、开发或内部目的，可能需要创建自签名 SSL 证书。

本文作为如何使用OpenSSL生成自签名SSL证书的简单指南。

## 分步指南

### 1: 安装OpenSSL

互联网上有大量有关如何安装 OpenSSL 的信息。

1. 对于基于 Ubuntu 或 Debian 的系统：
```shell
sudo apt install openssl
```
2. 对于基于 CentOS、Fedora 或 RHEL 的系统：
```shell
sudo yum install openssl
```
3. 对于MacOS
```shell
brew install openssl
```

### 2: 生成私钥
```shell
openssl genrsa -out test.key 2048 
```
运行命令生成 2048 位 RSA 私钥（将 test.key 替换为您的文件名）：
```shell
Generating RSA private key, 2048 bit long modulus (2 primes)
..............+++++
.............+++++
e is 65537 (0x010001)
```

### 3: 创建证书签名请求
证书签名请求 (CSR) 包含有关您的应用程序和组织的所有信息。 然后可以使用 CSR 来生成 SSL 证书。
```shell
openssl req -new -key test.key -out test.csr
```
```shell
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]:CA
State or Province Name (full name) [Some-State]:Ontario 
Locality Name (eg, city) []:Toronto 
Organization Name (eg, company) [Internet Widgits Pty Ltd]:Test 
Organizational Unit Name (eg, section) []:Test 
Common Name (e.g. server FQDN or YOUR name) []:test.com 
Email Address []:test@test.com

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:
An optional company name []:
```

### 4: 请求自签名SSL证书
```shell
openssl x509 -req -days 365 -in test.csr -signkey test.key -out test.crt
```

命令选项和用法可以在以下位置找到:  https://www.openssl.org/docs/man1.1.1/man1/x509.html

```shell
Signature ok
subject=C = CA, ST = Ontario, L = Toronto, O = Test, OU = Test, CN = test.com, emailAddress = test@test.com
Getting Private key
```


### 5: 配置 HTTPs Web 服务器

生成自签名 SSL 证书并可供 Web 服务器使用。

一个流行的选择是 Nginx。 这是 nginx HTTPs 的示例配置：
```shell
server {
    listen              443 ssl;
    server_name         www.example.com;
    ssl_certificate     www.example.com.crt;
    ssl_certificate_key www.example.com.key;
    ssl_protocols       TLSv1 TLSv1.1 TLSv1.2 TLSv1.3;
    ssl_ciphers         HIGH:!aNULL:!MD5;
    ...
}
```

