APIs (Application Programming Interfaces) enable software systems and applications to communicate and share data. API testing is important as vulnerabilities in APIs may undermine core aspects of a website's confidentiality, integrity, and availability.

All dynamic websites are composed of APIs, so classic web vulnerabilities like SQL injection could be classed as API testing. In this topic, we'll teach you how to test APIs that aren't fully used by the website front-end, with a focus on RESTful and JSON APIs. We'll also teach you how to test for server-side parameter pollution vulnerabilities that may impact internal APIs.

To illustrate the overlap between API testing and general web testing, we've created a mapping between our existing topics and the [OWASP API Security Top 10 2023](https://portswigger.net/web-security/api-testing/top-10-api-vulnerabilities).

## 1. Principle

目标接口限制可绕过

### 1.1. Location

任何调用 API 的位置, 关注 JS 文件

## 2. Test

### 2.1. API 文档泄露

爆破 API 文档

> [H2-9000.txt](https://github.com/SexyBeast233/SecDictionary/blob/master/filelak/H2-9000.txt)
>
> [api-endpoints.txt ](https://github.com/danielmiessler/SecLists/blob/9da1d4931fc83e6999f09c5dae6802b2ad2ec7d0/Discovery/Web-Content/api/api-endpoints.txt)

### 2.2. Bypass

爆破请求方式

隐藏参数绕过

```
username=foo%26x=y # 使用 & 拼接
username=foo%23    # 使用 # 截断
username=foo%3f    # 使用 ? 查询
```

目录穿越绕过

```
username=./foo # 可能存在目录穿越
username=../../../../api-docs%23 # 获取 API 文档
```

---

**Refrences**

- [API testing](https://portswigger.net/web-security/api-testing)
- [CWE-475](https://hackerone.com/hacktivity/cwe_discovery?id=cwe-475)

