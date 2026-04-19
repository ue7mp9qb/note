Redirect connections through socks/http proxies.

## 1. Init

修改配置文件

```
┌──(root@kali)-[~]
└─$ nano -l /etc/proxychains4.conf
```

```
161  # socks5        127.0.0.1 9050
162  socks5          10.0.2.2 10808
```

测试代理

```
┌──(root@kali)-[~]
└─$ proxychains4 curl ipinfo.io
```

## 2. Usage

通过代理执行程序

```
┌──(root@kali)-[~]
└─$ proxychains4 <command>
```

---

**References**

- [proxychains-ng](https://www.kali.org/tools/proxychains-ng/)

