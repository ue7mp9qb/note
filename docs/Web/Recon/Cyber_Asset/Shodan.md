## 1. Install

安装

```
┌──(root@debian)-[~]
└─# pipx install shodan
```

配置环境变量

```
┌──(root@debian)-[~]
└─# pipx ensurepath
```

加载配置文件

```
┌──(root@debian)-[~]
└─# source ~/.zshrc
```

## 2. Init

添加API

```
┌──(root@debian)-[~]
└─# shodan init <api_key>
```

## 3. Usage

```
shodan search hostname:10086.cn http.status:200
shodan search hostname:10086.cn,http.status:200
shodan search hostname:10086.cn http.status:200 -country:CN
shodan search hostname:10086.cn,(http.status:200 -country:CN)
```

---

References

- [Shodan](https://www.shodan.io/)
- [Install the CLI](https://help.shodan.io/command-line-interface/0-installation)