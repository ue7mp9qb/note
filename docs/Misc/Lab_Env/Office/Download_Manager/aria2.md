aria2 is a lightweight multi-protocol & multi-source command-line download utility.

## 1. Init

添加到环境变量

```
%UserProfile%\AppData\Local\Programs\aria2
```

## 3. Usage

经典下载

```
PS C:\Users\null> aria2c -x 16 --all-proxy="http://127.0.0.1:10808" "https://example.com/file_name.zip"
```

配置 16 线程下载

```
PS C:\Users\null> aria2c -x 16 "http://example.com/file.zip"
```

配置代理下载

```
PS C:\Users\null> aria2c --all-proxy="http://127.0.0.1:10808" "https://example.com/file_name.zip"
```

---

References

- [aria2](https://aria2.github.io/)

