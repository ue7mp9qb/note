Subdomain discovery tool.

## 1. Install

安装

```
┌──(root@kali)-[~]
└─$ apt install -y subfinder
```

## 2. Usage

更新

```
┌──(root@kali)-[~]
└─$ subfinder -up
```

指定域名扫描

```
┌──(root@kali)-[~]
└─$ subfinder -d example.com
```

指定文件扫描

```
┌──(root@kali)-[~]
└─$ subfinder -dL ~/domain.txt
```

保存到文件

```
┌──(root@kali)-[~]
└─$ subfinder -d example.com -o ~/subfinder_example.com.txt
```

仅显示活跃的 HOST

```
┌──(root@kali)-[~]
└─$ subfinder -d example.com -nW
```

仅保存活跃的 HOST

```
┌──(root@kali)-[~]
└─$ subfinder -d example.com -nW -o ~/subfinder_example.com.txt
```

查看 API 配置

```
┌──(root@kali)-[~]
└─$ subfinder -ls
```

配置 API

```
┌──(root@kali)-[~]
└─$ nano ~/.config/subfinder/provider-config.yaml
```

---

References

- [subfinder](https://www.kali.org/tools/subfinder/)

