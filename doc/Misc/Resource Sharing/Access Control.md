Access control is the application of constraints on who or what is authorized to perform actions or access resources. In the context of web applications, access control is dependent on authentication and session management:

- **Authentication** confirms that the user is who they say they are.
- **Session management** identifies which subsequent HTTP requests are being made by that same user.
- **Access control** determines whether the user is allowed to carry out the action that they are attempting to perform.

Broken access controls are common and often present a critical security vulnerability. Design and management of access controls is a complex and dynamic problem that applies business, organizational, and legal constraints to a technical implementation. Access control design decisions have to be made by humans so the potential for errors is high.

## 1. Principle

目标的权限配置不够完善造成越权或未授权

### 1.1. Location

需要隔离权限的访问

## 2. Test

### 2.1. Unprotected

目标存在隐藏的控制面板未作校验即可访问

### 2.2. Request Parameter

通过修改请求参数实现越权

```
Cookie: Admin=true; session=MOp4UbKnwySqBZrJQb3cyHxYKSlY8G2e
```

### 2.3. User Profile

在响应包中发现了隐藏参数 `"roleid":1` , 添加到请求包中并修改为 `"roleid":2` 可实现越权

### 2.4. User ID

通过修改 UID 实现越权

```
https://web-security-academy.net/my-account?id=carlos
```

### 2.5. Unpredictable User IDs

UUID 不可预测, 可尝试通过访问与目标相关的页面查找泄露的 UUID 

```
https://web-security-academy.net/blogs?userId=13eafe6b-685d-4a62-897e-4135a70cee9a
```

### 2.6. Data Leakage in Redirect

依然是修改 UID, 但是若登录信息不匹配会跳转到登录界面, 可使用 `Repeater` 发包在跳转前获取信息

```
https://web-security-academy.net/my-account?id=carlos
```

### 2.7. Password Disclosure

目标用户主页可访问, 密码隐藏在界面中

```
<input required type=password name=password value='95plcdbbjecafn6gfwfq'/>
```

### 2.8. Insecure Direct Object References

可越权下载其它用户的敏感信息

```
https://web-security-academy.net/download-transcript/1.txt
```

### 2.9. 403 Bypass

使用 HTTP Header 伪造绕过 403

### 2.10. Change Request Method

通过修改 Change request method 越权访问

### 2.11. Multi-Step

多步骤的请求越权

### 2.12. Referer-Based

需要跳转的请求越权

---

**References**

- [Access control](https://portswigger.net/web-security/access-control)
- [CWE-284](https://hackerone.com/hacktivity/cwe_discovery?id=cwe-284)