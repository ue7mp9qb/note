目录扫描。

## 1 安装

下载

```shell
┌──(root㉿kali)-[~]
└─# git clone https://github.com/H4ckForJob/dirmap.git /root/tools/dirmap && pip3 install -r /root/tools/dirmap/requirement.txt
```

编写脚本

```shell
┌──(root㉿kali)-[~]
└─# vim /root/tools/dirmap/dirmap.sh
```

```sh
#!/bin/bash

# 错误检测
set -e

# 获取链接的实际路径并切换到该目录
cd "$(dirname "$(readlink -f "$0")")"

# 运行 dirmap
python3 ./dirmap.py "$@"

```

创建链接

```shell
┌──(root㉿kali)-[~]
└─# chmod +x /root/tools/dirmap/dirmap.sh && ln -sf /root/tools/dirmap/dirmap.sh /usr/local/bin/dirmap
```

## 2 使用

查看帮助

```shell
┌──(root㉿kali)-[~]
└─# dirmap -h
```

扫描 host

```shell
┌──(root㉿kali)-[~]
└─# dirmap -i https://example.com/ -lcf
```

扫描 ip

```shell
┌──(root㉿kali)-[~]
└─# dirmap -i 192.168.1.1 -lcf 
```

扫描子网

```shell
┌──(root㉿kali)-[~]
└─# dirmap -i 192.168.1.0/24 -lcf
```

扫描网络范围

```shell
┌──(root㉿kali)-[~]
└─# dirmap -i 192.168.1.1-192.168.1.100 -lcf
```

从文件中读取

```shell
┌──(root㉿kali)-[~]
└─# dirmap -iF targets.txt -lcf
```

> 结果将自动保存在 `/root/tools/dirmap/output`

---

References

- [Dirmap](https://github.com/H4ckForJob/dirmap)
