## 1. Deploy

克隆项目

```
┌──(root@debian)-[~]
└─# git clone https://github.com/lijiejie/BBScan.git /root/tools/apps/bbscan && cd /root/tools/apps/bbscan
```

安装 Python 3.7

```
┌──(root@debian)-[~]
└─# pyenv install 3.7 && pyenv rehash
```

设置当前目录的 Python 版本

```
┌──(root@debian)-[~]
└─# pyenv local 3.7 && pyenv exec python3.7 --version
```

激活虚拟环境

```
┌──(root@debian)-[~/tools/apps/bbscan]
└─# pyenv exec python3.7 -m venv ./venv && source ./venv/bin/activate
```

安装依赖

```
┌──(venv)─(root@debian)-[~/tools/apps/bbscan]
└─# pyenv exec python3.7 -m pip install -i https://mirrors.ustc.edu.cn/pypi/simple pip -r requirements.txt
```

查看帮助

```
┌──(venv)─(root@debian)-[~/tools/apps/bbscan]
└─# pyenv exec python3.7 ./BBScan.py -h
```

退出虚拟环境

```
┌──(venv)─(root@debian)-[~/tools/apps/bbscan]
└─# deactivate
```

## 2. 初始化

编写运行脚本

```
┌──(root@debian)-[~/tools/apps/bbscan]
└─# vim ./bbscan.sh
```

```shell
#!/bin/bash

# 错误检测
set -e

# 激活虚拟环境
source /root/tools/apps/bbscan/venv/bin/activate

# 获取链接的实际路径并切换到该目录
cd "$(dirname "$(readlink -f "$0")")"

# 运行 bbscan
pyenv exec python3.7 ./BBScan.py "$@"
```

创建链接

```
┌──(root@debian)-[~/tools/apps/bbscan]
└─# chmod +x ./bbscan.sh && ln -sf /root/tools/apps/bbscan/bbscan.sh /usr/local/bin/bbscan && cd
```

查看帮助

```
┌──(root@debian)-[~]
└─# bbscan -h
```

## 3. 使用

从文件中扫描 URL 目标

```
┌──(root@debian)-[~]
└─# bbscan -f /root/fileName.txt --api --skip --no-browser
```

查看扫描报告

```
┌──(root@debian)-[~]
└─# ls /root/tools/apps/bbscan/report/
```

---

References

- [BBScan](https://github.com/lijiejie/BBScan)
