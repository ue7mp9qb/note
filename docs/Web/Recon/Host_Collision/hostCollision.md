一款 Host 碰撞工具.

## 1. 安装

克隆仓库

```
┌──(root@debian)-[~]
└─# git clone https://github.com/alwaystest18/hostCollision.git /root/tools/apps/hostCollision && cd /root/tools/apps/hostCollision
```

```
┌──(root@debian)-[~/tools/apps/hostCollision]
└─# go install && go build hostCollision.go
```

## 2. 初始化

编写运行脚本

```
┌──(root@debian)-[~/tools/apps/hostCollision]
└─# nano ./hostCollision.sh
```

```sh
#!/bin/bash

# 错误检测
set -e

# 获取链接的实际路径并切换到该目录
cd "$(dirname "$(readlink -f "$0")")"

# 运行 hostCollision
./hostCollision "$@"
```

创建链接

```
┌──(root@debian)-[~/tools/apps/hostCollision]
└─# chmod +x ./hostCollision.sh \
&& ln -sf /root/tools/apps/hostCollision/hostCollision.sh /usr/local/bin/hostCollision \
&& cd
```

查看帮助

```
┌──(root@debian)-[~]
└─# hostCollision -h
```

## 3. 使用

测试 Host 碰撞

```
┌──(root@debian)-[~]
└─# hostCollision -df fileName.txt -uf fileName.txt -o fileName.txt
```

---

References

- [hostCollision](https://github.com/alwaystest18/hostCollision)

