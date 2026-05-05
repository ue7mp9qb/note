A utility to detect various technology for a given IP address.

## 1. Install

安装

```
┌──(root@kali)-[~]
└─$ go install -v github.com/projectdiscovery/cdncheck/cmd/cdncheck@latest
```

## 2. Usage

确认 IP 是否使用了 CDN

```
┌──(root@kali)-[~]
└─$ cdncheck -i 1.1.1.1 -cdn
```

显示列表中使用了 CDN 的 IP

```
┌──(root@kali)-[~]
└─$ cdncheck -i ~/ip.txt -cdn
```

保存列表中使用了 CDN 的 IP

```
┌──(root@kali)-[~]
└─$ cdncheck -i ~/ip.txt -cdn -o ~/cdncheck_example.com.txt
```

---

**References**

- [cdncheck](https://github.com/projectdiscovery/cdncheck)

