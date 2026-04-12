Python is a programming language that lets you work quicklyand integrate systems more effectively.

## 1. Init

配置镜像加速

```
┌──(root@kali)-[~]
└─# pip config set global.index-url "https://mirrors.tuna.tsinghua.edu.cn/pypi/web/simple"
```

## 2. Usage

### 2.1. venv

创建虚拟环境

```
┌──(root㉿kali)-[~]
└─# python -m venv ./venv
```

激活虚拟环境

```
┌──(root㉿kali)-[~]
└─# source ./venv/bin/activate
```

安装依赖

```
┌──(root㉿kali)-[~]
└─# pip install -r ./requirements.txt
```

退出虚拟环境

```
┌──(venv)─(root@kali)-[~]
└─# deactivate
```

### 2.2. pipx

在隔离环境中安装

```
┌──(root㉿kali)-[~]
└─# pipx install <app>
```

---

References

- [python](https://www.python.org/)

