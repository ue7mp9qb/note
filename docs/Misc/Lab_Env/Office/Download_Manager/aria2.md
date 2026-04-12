aria2 is a lightweight multi-protocol & multi-source command-line download utility.

## 1. Install

安装

```
┌──(nemo@debian)-[~]
└─$ sudo apt install -y aria2
```

## 2. Init

添加到环境变量

```
C:\Users\sec\AppData\Local\Programs\aria2
```

## 3. Usage

经典下载

```
┌──(nemo@debian)-[~]
└─$ aria2c -x 16 --all-proxy="http://127.0.0.1:10808" "https://example.com/file_name.zip"
```

配置 16 线程下载

```
┌──(nemo@debian)-[~]
└─$ aria2c -x 16 "http://example.com/file.zip"
```

配置代理下载

```
┌──(nemo@debian)-[~]
└─$ aria2c --all-proxy="http://127.0.0.1:10808" "https://example.com/file_name.zip"
```

---

References

- [aria2](https://aria2.github.io/)
