Fast and customizable vulnerability scanner based on simple YAML based DSL.

## 1. Install

安装

```
┌──(root@kali)-[~]
└─$ apt install -y nuclei
```

## 2. Init

更新模板

```
┌──(root@kali)-[~]
└─$ nuclei -ut
```

## 3. Usage

经典扫描

```
┌──(root@kali)-[~]
└─$ nuclei -u "https://www.example.com/"
```

---

**References**

- [nuclei](https://www.kali.org/tools/nuclei/)

