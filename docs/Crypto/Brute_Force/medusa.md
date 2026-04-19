Fast, parallel, modular, login brute-forcer for network services.

## 1. Usage

列出已知模块

```
┌──(root@kali)-[~]
└─$ medusa -d
```

爆破 SSH

```
┌──(root@kali)-[~]
└─$ medusa -M ssh -h example.com -u username -p password -e ns

┌──(root@kali)-[~]
└─$ medusa -M ssh -H host.txt -U username.txt -P password.txt -e ns 
```

---

**References**

- [medusa](https://www.kali.org/tools/medusa/)

