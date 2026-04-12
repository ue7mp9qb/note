OneForAll 是一款功能强大的子域收集工具.

## 1 安装

克隆项目

```
root@debian:~# git clone https://github.com/shmilylty/OneForAll.git ~/.local/oneforall && cd ~/.local/oneforall
```

激活虚拟环境

```
root@debian:~/tools/apps/oneforall# python3 -m venv ./venv && source ./venv/bin/activate
```

安装依赖

```
(venv) root@debian:~/tools/apps/oneforall# python3 -m pip install -r ./requirements.txt
```

查看帮助

```
(venv) root@debian:~/tools/apps/oneforall# python3 ./oneforall.py --help
```

退出虚拟环境

```
(venv) root@debian:~/tools/apps/oneforall# deactivate
```

## 2 初始化

编写运行脚本

```
root@debian:~/tools/apps/oneforall# vim ./oneforall.sh
```

```sh
#!/bin/bash

# 错误检测
set -e

# 激活虚拟环境
source /root/tools/apps/oneforall/venv/bin/activate

# 获取链接的实际路径并切换到该目录
cd "$(dirname "$(readlink -f "$0")")"

# 运行 oneforall
python3 ./oneforall.py "$@"
```

创建链接

```
root@debian:~/tools/apps/oneforall# chmod +x ./oneforall.sh && ln -sf /root/tools/apps/oneforall/oneforall.sh /usr/local/bin/oneforall && cd
```

查看帮助

```
root@debian:~# oneforall --help
```

## 3 使用

枚举单个目标的子域名

```
root@debian:~# oneforall --target example.com run
```

枚举文件中所有目标的子域名

```
root@debian:~# oneforall --target FileName.txt run
```

查看结果

```
root@debian:~# ls /root/tools/apps/oneforall/results
```

---

Resferences

- [OneForAll](https://github.com/shmilylty/OneForAll)
