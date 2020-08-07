---
title: 有状态（SESSION）和无状态（JWT）登录验证
toc: true
categories: 与代码
tags: 
	- JWT
date: 2018-12-05
---

最近公司在做系统设计的时候提到了这么一个讨论，就是有状态的登录验证和无状态的登录验证，至于这两者的利弊有哪些，具体如何做选择，可以把两者的特点来做一些对比。其实这两者共同的目的就是解决http协议的无状态特性，标示客户端会话用户。不论用这两者哪种方式，token 和 session 都可能会被窃取从而发生CSRF攻击，全站 HTTPS 是必不可少。google一下发现关于这两者的讨论都不少，在此总结一番，有其他意见的欢迎留言讨论。

## 有状态（SESSION）
所谓有状态，就是的就是传统的 cookie session ，cookie的身份验证是有状态的。这意味着验证的记录或者会话(session)必须同时保存在服务器端和客户端。服务器端需要跟踪记录session并存至数据库，同时前端需要在cookie中保存一个sessionID，作为session的唯一标识符，可看做是session的“身份证”。前端退出的话就清cookie。后端强制前端重新认证的话就清或者修改session。

### 优点
- 相较于无状态的验证机制，传统的session可以直接从后端主动控制下线，删除session，方便实现互t等功能	
- session保存在服务端，相对比较安全
- 有状态的session可以较为准确统计在线人数

### 缺点
- 有状态的存储session需要服务器空间
- 扩展不方便，需要session同步，借助redis实现共享等

## 无状态（JWT）
无状态，首先就是不需要服务端去存储，只需要将用户名密码传入后端接口后，将用户的信息作为 JWT的payload，将其与头部分别进行Base64编码拼接后签名，形成一个JWT。形成的JWT就是一个形同lll.zzz.xxx的字符串。前端就需要保存这个返回的结果在localStorage或sessionStorage上，退出登录时前端删除即可。

### 优点

- json通用性，方便跨语言
- 不需要在服务端保存会话信息, 易于应用的扩展
- 占用字节很小，方便传输

### 缺点
- JWT的加解密耗费CPU计算资源
- 不能方便的管理会话
- 注销没有即时性，如果token泄露，注销状态下仍可登录操作

## 相关讨论&链接
- [聊一聊JWT与session](https://juejin.im/post/5a437441f265da43294e54c3)
- [cookie-session机制与JWT机制对比](https://www.jianshu.com/p/cbe253281d26)
- [别再用 JWT 做会话管理了](https://www.bonjourcs.com/2017/12/19/%E5%88%AB%E5%86%8D%E7%94%A8-JWT-%E5%81%9A%E4%BC%9A%E8%AF%9D%E7%AE%A1%E7%90%86%E4%BA%86/)
- [Stop using JWT for sessions](http://cryto.net/~joepie91/blog/2016/06/13/stop-using-jwt-for-sessions/)
- [请停止使用 JWT 认证](https://www.zcfy.cc/article/stop-using-jwt-for-sessions-joepie91s-ramblings)
- [从有状态应用（Session）到无状态应用（JWT），以及 SSO 和 OAuth2](https://blog.csdn.net/kikajack/article/details/80293328)
- [不要用JWT替代session管理（上）：全面了解Token,JWT,OAuth,SAML,SSO](https://zhuanlan.zhihu.com/p/38942172)
- [JSON Web Tokens vs. Session Cookies: In Practice](https://ponyfoo.com/articles/json-web-tokens-vs-session-cookies)

