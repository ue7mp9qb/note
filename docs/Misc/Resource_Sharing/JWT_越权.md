 JWT 是一种凭证, 可以授予对资源的访问权限.

## 1. 分析

以 `eyJ` 开头的一段编码字符串通常是 JWT

```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
```

其中 Header 和 Payload 部分是 Base64 编码

```
Header.Payload.Signature
```

## 2. 越权

可通过修改 Payload 解码后的用户名越权

```
"name":"test"
"name":"admin"
```

## 3. 绕过

### 3.1. 将 Header 中的 `alg` 的值改为 `none` 

```
"alg":"HS256"
"alg":"none"
```

### 3.2. 删除 Signature 部分

```
Header.Payload.
```

---

References

- https://jwt.io/
