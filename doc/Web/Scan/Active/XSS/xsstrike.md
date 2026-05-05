Most advanced XSS scanner.

## 1. Install

安装

```
┌──(root@kali)-[~]
└─$ apt install -y xsstrike
```

## 2. Usage

测试 GET 传参的网页

```
┌──(root㉿kali-23)-[~]
└─# xsstrike -u "http://example.com/search.php?q=query"
```

测试 POST 传参的网页

```
┌──(root㉿kali-23)-[~]
└─# xsstrike -u "http://example.com/search.php" --data "q=query"
```

---

**References**

- [xsstrike](https://www.kali.org/tools/xsstrike/)
