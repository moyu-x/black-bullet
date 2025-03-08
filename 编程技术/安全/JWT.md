## 概念

JSON Web Token（JWT）是一种轻量级、自包含的安全标准，用于在各方之间安全地传输信息。它以 JSON 对象的形式存在，包含三个部分：Header（头部）、Payload（载荷）和Signature（签名）。

## 使用场景

- 授权
- 信息交换

## 结构

- Header
	- 令牌类型
	- 使用的签名算法
- Payload
	- Registered claims
	- Public claims
	- Private claims
- Signature

## 使用

通常在 Authorization 头中使用 Bearer 方案。头部的内容应该如下所示：

```txt
Authorization: Bearer <token>
```

如果通过 HTTP 头部发送 JWT 令牌，应该尽量避免令牌过大。一些服务器不接受超过 8 KB 的头部。如果您试图在 JWT 令牌中嵌入过多信息，比如包含所有用户的权限，则可能需要替代解决方案

## JWT 的验证

验证确保令牌格式正确并包含可执行的声明，通常指的是检查 JWT 的结构、格式和内容

## JWT的校验

确认确保令牌是真实的且未被修改。涉及确认令牌的真实性和完整性

## ref：

- [JSON Web Token Introduction - jwt.io](https://jwt.io/introduction)