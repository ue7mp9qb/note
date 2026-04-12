dig  is a flexible tool for interrogating DNS name servers.

## 1. Usage

查询 A 记录

```
┌──(nemo@debian)-[~]
└─$ dig A example.com +short
```

保存 A 记录

```
┌──(nemo@debian)-[~]
└─$ dig A example.com +short | tee -a ~/dig_example.com.txt
```

查询 PTR 记录

```
┌──(nemo@debian)-[~]
└─$ dig -x 1.1.1.1 +short
```

---

References

- [dig](https://www.kali.org/tools/bind9/#dig)

