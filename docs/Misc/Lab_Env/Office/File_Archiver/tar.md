GNU Tar provides the ability to create tar archives, as well as various other kinds of manipulation.

## 1. Usage

### 1.1 解压

解压 `.tar` 压缩包

```
tar –xvf [file_name].tar
```

解压 `.tar.gz` 压缩包

```
tar -xzvf [file_name].tar.gz
```

 解压 `.tar.bz2` 压缩包

```
tar -xjvf [file_name].tar.bz2 
```

解压 `.tar.Z` 压缩包

```
tar –xZvf [file_name].tar.Z
```

解压文件到指定路径下

```
tar -xzvf [file_name].tar.gz -C [path]
```

### 1.2. 压缩

将当前目录中所有 `.jpg` 文件打包成 `[file_name].tar` 

```
tar –cvf [file_name].tar *.jpg
```

将目录里所有 `.jpg` 文件打包成 `[file_name].tar` 后，再将其用 gzip 压缩为 `[file_name].tar.gz` 

```
tar –czf [file_name].tar.gz *.jpg
```

将目录里所有 `.jpg` 文件打包成 `[file_name].tar` 后，再将其用 bzip2 压缩为 `[file_name].tar.bz2` 

```
tar –cjf [file_name].tar.bz2 *.jpg
```

将目录里所有 `.jpg` 文件打包成 `[file_name].tar` 后，再将其用 compress压缩为 `[file_name].tar.Z`

```
tar –cZf [file_name].tar.Z *.jpg
```

---

References

- [Tar](https://www.gnu.org/software/tar/)
- [Documentation for Tar](https://www.gnu.org/software/tar/manual/)
