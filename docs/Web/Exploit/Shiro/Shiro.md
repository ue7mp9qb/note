Apache Shiro™ is a powerful and easy-to-use Java security framework that performs authentication, authorization, cryptography, and session management. With Shiro’s easy-to-understand API, you can quickly and easily secure any application – from the smallest mobile applications to the largest web and enterprise applications.

## 1. Fingerprint

HaE

```
(=deleteMe|rememberMe=)
```

在 login/logout 时, 响应包会返回

```
Set-Cookie: rememberMe=deleteMe
```

Login Page 1

![Login Page 1](./../../../../images/Shiro/Login%20Page%201.png)

Login Page 2

![Login Page 2](./../../../../images/Shiro/Login%20Page%202.png)

Login Page 3

![Login Page 3](./../../../../images/Shiro/Login%20Page%203.png)

## 2. Nday

## 2.1. 反序列化 RCE

靶场

- [CVE-2016-4437](https://hackerone.com/hacktivity/cve_discovery?id=CVE-2016-4437) (Shiro < 1.2.5, ==未修复: 建议自行配置随机密钥==)

  ```
  vulfocus/shiro-cve_2016_4437:latest
  ```

- [CVE-2019-12422](https://hackerone.com/hacktivity/cve_discovery?id=CVE-2019-12422) (Shiro < 1.4.2, ==已修复: AES-CBC -> AES-GCM==)

  ```
  vulfocus/shiro-721:latest
  ```

工具

- [ShiroAttack2](https://github.com/SummerSec/ShiroAttack2)
- [ShiroAttack2_Pro](https://github.com/Chave0v0/ShiroAttack2_Pro)

---

References

- [Shiro](https://shiro.apache.org/)
- [shiro框架漏洞学习](https://www.bilibili.com/video/BV1pRurzFE5W/?spm_id_from=333.1387.favlist.content.click&vd_source=2dcc7806c9580af60063ca1edb63852d)

