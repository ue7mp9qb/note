Identify and fingerprint Web Application Firewall products.

## 1. Usage

识别单个目标使用的 WAF

```
┌──(root㉿kali)-[~]
└─$ wafw00f -a https://example.com -o ~/wafw00f_example.com.txt
```

识别多个目标使用的 WAF

```
┌──(root㉿kali)-[~]
└─$ wafw00f -a -i ~/url.txt -o ~/wafw00f_url.txt
```

---

References

- [wafw00f](https://www.kali.org/tools/wafw00f/)

