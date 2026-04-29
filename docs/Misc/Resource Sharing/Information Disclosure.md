Information disclosure, also known as information leakage, is when a website unintentionally reveals sensitive information to its users. Depending on the context, websites may leak all kinds of information to a potential attacker, including:

- Data about other users, such as usernames or financial information
- Sensitive commercial or business data
- Technical details about the website and its infrastructure

The dangers of leaking sensitive user or business data are fairly obvious, but disclosing technical information can sometimes be just as serious. Although some of this information will be of limited use, it can potentially be a starting point for exposing an additional attack surface, which may contain other interesting vulnerabilities. The knowledge that you are able to gather could even provide the missing piece of the puzzle when trying to construct complex, high-severity attacks.

Occasionally, sensitive information might be carelessly leaked to users who are simply browsing the website in a normal fashion. More commonly, however, an attacker needs to elicit the information disclosure by interacting with the website in unexpected or malicious ways. They will then carefully study the website's responses to try and identify interesting behavior.

## 1. Principle

信息泄露

### 1.1. Location

页面源码, URL, 报错, 数据包, 隐藏路径

## 2. Test

### 2.1. Error Messages

测试 SQLi 后报错, 回显中有框架版本信息

```
Apache Struts 2 2.3.31
```

### 2.2. Debug Page

在页面源码中找到了一个调试页面的路径

```
<!-- <a href=/cgi-bin/phpinfo.php>Debug</a> -->
```

### 2.3. Source Code Disclosure

目录扫描得到源码备份

```
https://web-security-academy.net/backup
```

### 2.4. Information Disclosure

修改请求方式获取隐藏参数

```
X-Custom-Ip-Authorization: 127.0.0.1
```

### 2.5. Version Control History

目录扫描发现 Git 源码泄露

```
wget -r "https://web-security-academy.net/.git"
```

---

**References**

- [Information disclosure](https://portswigger.net/web-security/information-disclosure)
- [CWE-200](https://hackerone.com/hacktivity/cwe_discovery?id=cwe-200)