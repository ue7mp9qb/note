ss - another utility to investigate sockets

## 1. Usage

筛查可疑连接

```
┌──(nemo@debian)-[~]
└─$ sudo ss -anp | grep ESTAB
```

定位可疑进程

```
┌──(nemo@debian)-[~]
└─$ ps -p <PID> -o user,pid,ppid,cmd
```

---

References

- [ss](https://manpages.debian.org/stretch/iproute2/ss.8.en.html)

