Web server security scanner.

## 1. Install

安装

```
┌──(nemo@debian)-[~]
└─$ apt install -y nikto
```

## 2. Usage

对单个目标进行扫描

```
┌──(nemo@debian)-[~]
└─$ nikto -h example.com -Format htm -o ~/nikto_example.com.htm
```

对多个目标进行扫描

```
┌──(nemo@debian)-[~]
└─$ nikto -h ~/host.txt -Format htm -o ~/nikto_host.htm
```

指定 HTTPS 端口扫描

```
┌──(nemo@debian)-[~]
└─$ nikto -h ~/host.txt -p 443 -ssl -Format htm -o ~/nikto_host.htm
```

---

**References**

- [nikto](https://www.kali.org/tools/nikto/)

