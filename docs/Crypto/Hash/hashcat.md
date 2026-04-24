World’s fastest and most advanced password recovery utility.

## 1. Usage

使用字典爆破 JWT 密钥

```
┌──(root㉿kali)-[~]
└─# hashcat -a 0 -m 16500 <YOUR-JWT> scraped-JWT-secrets.txt
```

---

**References**

- [hashcat](https://www.kali.org/tools/hashcat/)