Perform multiple dns queries.

## 1. Install

安装

```
┌──(root@kali)-[~]
└─$ apt install -y dnsx
```

## 2. Usage

更新

```
┌──(root@kali)-[~]
└─$ dnsx -up
```

查询 A 记录

```
┌──(root@kali)-[~]
└─$ echo example.com | dnsx -a -ro
```

保存 A 记录

```
┌──(root@kali)-[~]
└─$ echo example.com | dnsx -a -ro -o ~/dnsx_example.com.txt
```

查询 PTR 记录

```
┌──(root@kali)-[~]
└─$ echo 1.1.1.1 | dnsx -ptr -ro
```

---

**References**

- [dnsx](https://www.kali.org/tools/dnsx/)

