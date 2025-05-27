---
title: Cookie,Session,Jwt之间的区别和使用
date: 2025-05-19 17:34:31
tags:
---

# 为什么需要Cookie和Session

原因：由于HTTP是无状态的，HTTP的每个请求都是独立的，服务器不会保存客户端之前的请求和会话状态，所以单单依靠HTTP，是无法实现用户登录后，访问其他页面也是使用之前的登录状态，而是会跳转到重新登录页面。

所以需要Cookie和Session来实现用户登录状态的保存！

## Cookie和Session的使用

1.用户登录时将用户名和密码传递给服务端进行校验身份

2.校验身份成功后，服务端会创建一个session对象用来存储用户信息（session保存在内存中），并返回一个session_id给客户端

3.客户端将session_id存入cookie中，后续客户端每次进行访问该服务端进行携带该session_id即可

4.服务端接受到session_id后，根据该id去查询之前保存的session信息

<img src="C:\Users\彭思凯\AppData\Roaming\Typora\typora-user-images\image-20250519175704245.png" alt="image-20250519175704245" style="zoom:80%;" />

## 使用Cookie和Session保存用户状态的缺点

session是保存在服务器的内存中的。当服务端部署的机器是集群的情况，那么这些机器都需要保存相同的session信息，如果有一台机器没有保存到该session信息，那么进行负载均衡访问该机器时，就会跳转到登录页面重新登录



### 解决方案

**1.使用MySQL和Redis来保存用户信息**

由于session保存在内存中，所以服务端是集群的情况时，需要全都同步该session信息。所以可以使用数据库保存session_id，当用户传递一个session_id过来时，服务端可以去数据库查询是否存在该session_id来判断用户的登录状态。缺点：存在单点故障（如果数据库宕机，则保存的信息就访问不到或者丢失，如果部署数据库集群又会过于麻烦）



**2.使用JWT来进行无状态登录**

JWT工作流程：

1.用户登录时将用户名和密码传递给服务端，服务端校验用户身份成功后会生成JWT返回给客户端

2.客户端将该JWT保存在本地，之后每次访问服务端时都在请求头中携带该JWT

3.服务端接受到客户端请求后，从请求头中提取出JWT内容

4.验证JWT签名：

- 服务端使用预存的密钥（如 HMAC 的共享密钥或 RSA 的公钥），重新计算 JWT 的签名。
- 对比计算的签名与 JWT 中的第三段（签名段）是否一致

**客户端保存的JWT格式(Authorization)：Header.Payload.Signature**

```
GET /restful/products/1 HTTP/1.1
Host: icyfenix.cn
Connection: keep-alive
Authorization: Bearer eyJhbGciOiJXVCJ9.eyJ1c2VyXpeCJ9.539WMzbAn8bheflL8
```



<img src="C:\Users\彭思凯\AppData\Roaming\Typora\typora-user-images\image-20250519180502861.png" alt="image-20250519180502861"  />



## Cookie和Session的区别

存储位置：Cookie存放在本地浏览器中，session存储在服务端中

存储大小：Cookie存放在浏览器中，大小是有限制的，不能超过4K；而session存储大小没有限制

安全性：因为Cookie存放在浏览器中，安全性较低；session存放在服务端中，安全性高一点

有效时间：Cookie有效时间比session长
