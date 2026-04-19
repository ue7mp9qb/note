Very fast network logon cracker.

## 1. Usage

basic auth

```
┌──(root@kali)-[~]
└─$ hydra <host> -s <port> http-get /[protected_page] -L username.txt -P wordlist.txt -e ns -vV
```

ssh

```
┌──(root@kali)-[~]
└─$ hydra <host> ssh -l root -P wordlist.txt -t 4 -e ns -vV
```

ftp

```
┌──(root@kali)-[~]
└─$ hydra <host> ftp -L username.txt -P wordlist.txt -e ns -vV
```

---

**References**

- [hydra](https://www.kali.org/tools/hydra/)
