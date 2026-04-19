Active/passive network address scanner using ARP requests.

## 1. Usage

直接探测

```
┌──(root㉿kali)-[~]
└─$ netdiscover
```

被动探测

```
┌──(root㉿kali)-[~]
└─$ netdiscover -p
```

指定网卡

```
┌──(root㉿kali)-[~]
└─$ netdiscover -i vboxnet0
```

指定网段

```
┌──(root㉿kali)-[~]
└─$ netdiscover -r 192.168.1.0/24
```

---

**References**

- [netdiscover](https://www.kali.org/tools/netdiscover/)
