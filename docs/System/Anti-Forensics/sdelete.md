SDelete implements the Department of Defense clearing and sanitizing standard DOD 5220.22-M, to give you confidence that once deleted with *SDelete*, your file data is gone forever.

## 1. Usage

默认删除

```
sdelete file
```

多次删除

```
sdelete -p 3 file
```

覆盖磁盘空闲空间

```
sdelete -z C:
```

---

References

- [SDelete](https://learn.microsoft.com/en-us/sysinternals/downloads/sdelete)