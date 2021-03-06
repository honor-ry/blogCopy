---

title: HTTP和HTTPS
date: 2019-06-15 13:44:14
tags: 计算机网络
mathjax: true

---

HTTP和HTTPS的原理介绍，以及两者区别！<!--more-->

## HTTP

## HTTPS

HTTP的缺点：

- 通信使用明文，内容可能被窃取；
- 不验证通信方的身份，可能遭遇伪装；
- 无法验证报文的完整性，可能遭遇篡改攻击。

### 什么是HTTPS

HTTPS并非是应用层一种新的协议，只是HTTP通信接口部分用SSL(安全套接字security scoket layer)和TSL(传输层安全 transport layer security)协议而已。

HTTP协议直接跟TCP通信。当使用SSL时，则演变成HTTP向和SSL协议通信，再由SSL和TCP通信。简而言之，HTTPS就是被SSL封装了一层的HTTP。


### HTTPS的作用

- 内容加密
- 身份认证
- 数据完整性验证

现在的HTTPS基本都是用TLS了，因为更加安全。

### HTTPS加密

- 对称加密： 内容的加密使用对称加密算法，对称加密算法中，公钥和私钥相同或者公私钥可以相互推导出来。客户端和服务端必须提前知道密钥，而在发送密钥的过程中可能被第三方监听。

- 非对称加密： 为了保证对称加密中密钥的安全，使用非对称加密算法。非对称加密算法中，由一个公私钥对，私钥用户保存，公钥发给其他用户。任何人无法从公钥推导出私钥来。

为什么不直接使用非对称加密算法加密？

因为非对称加密算法更为复杂，通信中使用非对称加密算法，效率比较低。

- 确保客户端收到的服务端的公钥是正确的（公钥在传输过程中，可能被攻击者替换），需要对证书中的公钥进行加密（HTTPS的服务端都必须去专门的证书机构注册一个证书），使用证书机构（CA）私钥进行加密，让步客户端利用CA的公钥进行解密。CA的公钥是内嵌在操作系统中，所以是安全的，CA的机构数据不多，所以维护起来方便。

总结：

HTTPS通过非对称加密（通常是RSA算法）加密对称加密的密钥，然后使用证书机构的私钥加密非对称加密的公钥，而证书机构的公钥会内置在操作系统中，从而保证即使第三方监听，也可以保证安全。

### SSL和TLS

SSL可确保数据在传输过程中不会被截取。

TSL用于两个应用程序之间提供保密性和数据完整性。

SSL/TSL协议的作用：

- 认证用户和服务器：确保数据发送到正确的客户端和服务器；
- 加密数据防止数据中途被窃取；
- 维护数据的完整性：确保数据在传输过程中不被篡改。


TSL比SSL的优势：

- 对于消息认证使用密钥散列法：TSL十三亿“消息认证代码的密钥散列法（HMAC）”，确保数据的完整性，HMAC比SSL使用的MAC更安全。
- 增强伪随机功能： PRF生成密钥数据。
- 改进的已完成消息验证
- 一致证书处理
- 特定报警消息


### SSL和TSL的握手过程

SSL与TSL握手整个过程如下图所示：

![](https://images.morethink.cn/ea844ad80d80956a30095d5e4f39fd7b.png)

客户端首次发出请求：

请求包含的内容：

- 支持的协议版本
- 支持的加密算法和压缩算法
- 一个随机数(N1，后面用于生成会话密钥)
- hello

服务端首次回应：

回应内容：

- 确认使用的加密通信协议版本，比如TSL1.0版本
- 确认使用的加密方法，比如RSA加密
- 一个随机数(N2)
- 服务端自己的证书

客户端再次回应：

客户端首先验证服务端证书，如果验证通过，则继续下面的操作，

- 客户端再次生成一个随机数（N3），然后使用服务器证书中的公钥加密
- 一个编码改变的消息（即随后发送的消息都将用双发协商的加密算法和密钥进行加密）
- 客户端握手结束通知，表示客户端的握手阶段已经结束，这一项通知也是前面发送的所有内容的hash值，用来提供服务器校验。

在上面整个握手过程中，客户端和服务器同时有了三个随机数，接着双方就用实现商定的加密算法，各自生成本次会话所用的同一把“会话密钥”。


趋势面试：

- 项目
- 用两三种方法实现斐不拉契数列，并分析时间和空间复杂度（当时就直接给的一个数列，没有是斐不拉契数列，需要自己看出来）
- 线程和进程的区别
- 用C++怎么实现多线程
- socket编程怎么实现TCP和UDP,scoket怎么体现的TCP和UDP
- 如果ping一个域名不通，怎么判断是哪里出错了，比如是南京移动中心过去不，还是北京中心过不去，找出在哪里失败了
- 讲一个你写过的多态例子，分析其中的原理
- SQL实现计算一个学生成绩表中每门成绩的平均成绩
- HTTPS中怎么实现数据完整性的


参考：

[https://www.cnblogs.com/morethink/p/8127276.html](https://www.cnblogs.com/morethink/p/8127276.html)