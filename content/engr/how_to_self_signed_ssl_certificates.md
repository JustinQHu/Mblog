---
title: "How to Create Self-Signed SSL Certificates"  
date: 2024-01-26T14:09:39-05:00  
draft: false  
categories: ['engineering']  
tags: ['engineering', 'Security', 'Web Development', 'SSL', 'SSL Certificate',  'Self-Signed SSL Certificate', HTTPS']  
---

## SSL Certificate 

Securing your website or application has become increasingly crucial in today's
digital landscape where cyber crimes become ever more pervasive.  

One common way to achieve security is through encryption.  Symmetric and asymmetric encryption have been
heavily studied in the field of [Cryptograph](https://en.wikipedia.org/wiki/Cryptography) and are widely used in the industry to help
secure digital systems. SSL certificates, which build on top of asymmetric encryption, 
are often used to secure websites by ensuring data in transit between clients and web
server are encrypted, thus private and secure. 

Businesses,organizations, and developers usually get SSL certificates from public [CA(Certificate Authority)](https://en.wikipedia.org/wiki/Certificate_authority) who act 
as the **Source of Trust** for the internet. It might be necessary to create self-signed SSL certificates in some cases,such as testing, 
learning, development, or internal purposes. 

This article serves as a simple guide on how to generate self-signed SSL certificates using [OpenSSL](https://www.openssl.org/).

## Step-by-Step Guide

### 1: Install OpenSSL

There are tons of information available on the internet on how to install [OpenSSL](https://www.openssl.org/).

1. For Ubuntu or Debian-based system:
```shell
sudo apt install openssl
```
2. For CentOS, Fedora, or RHEL-based systems:
```shell
sudo yum install openssl
```
3. For MacOS
```shell
brew install openssl
```

### 2: Generate a Private Key
```shell
openssl genrsa -out test.key 2048 
```
Run the command to generate a 2048-bit RSA private key (**replace** test.key with your file name):
```shell
Generating RSA private key, 2048 bit long modulus (2 primes)
..............+++++
.............+++++
e is 65537 (0x010001)
```

### 3: Create a Certificate Signing Request
A **Certificate Signing Request(CSR)** includes all information about your application
and organization.  Then a CSR can be used to generate the SSL certificate.

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

### 4: Request the Self-signed SSL Certificate
```shell
openssl x509 -req -days 365 -in test.csr -signkey test.key -out test.crt
```

Command options and usage can be found at:  https://www.openssl.org/docs/man1.1.1/man1/x509.html

```shell
Signature ok
subject=C = CA, ST = Ontario, L = Toronto, O = Test, OU = Test, CN = test.com, emailAddress = test@test.com
Getting Private key
```


### 5: Configure HTTPs Web Server

A self-signed SSL certificate is generated and can be used by the web server. 

A popular choice is [Nginx](https://nginx.org/en/docs/http/configuring_https_servers.html).  And here is the example configuration of nginx HTTPs: 
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

