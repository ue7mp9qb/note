Searches through git repositories for secrets.

## 1. Install

安装

```
┌──(root㉿kali)-[~]
└─$ apt install -y trufflehog
```

## 2. Usage

扫描凭证

```
┌──(root㉿kali)-[~]
└─$ trufflehog git https://github.com/trufflesecurity/test_keys
```

---

References

- [trufflehog](https://www.kali.org/tools/trufflehog/)