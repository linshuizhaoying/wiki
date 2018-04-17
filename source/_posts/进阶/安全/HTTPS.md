---
title: HTTPS
toc: true
date: 2018-03-27  08:30:46
tags: [前端,HTTPS,基础]
---

# 资料

[HTTPS工作原理](https://cattail.me/tech/2015/11/30/how-https-works.html)


## HTTPS 服务端和客户端连接的原理？（SSL 验证原理）

1. [明文] 客户端发送随机数client_random和支持的加密方式列表
2. [明文] 服务器返回随机数server_random ，选择的加密方式和服务器证书链
3. [RSA] 客户端验证服务器证书，使用证书中的公钥加密premaster secret 发送给服务端
4. 服务端使用私钥解密premaster secret
5. 两端分别通过client_random，server_random 和premaster secret 生成master secret，用于对称加密后续通信内容

HTTPS采用共享密钥加密和公开密钥加密混合的加密方式，在交换密钥对环节使用公开密钥加密方式（防止被监听泄漏密钥）加密共享的密钥，在随后的通信过程中使用共享密钥的方式使用共享的密钥进行加解密。

