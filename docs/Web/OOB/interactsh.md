An OOB interaction gathering server and client library.

## 1. Install

安装

```
┌──(root㉿kali)-[~]
└─$ go install -v github.com/projectdiscovery/interactsh/cmd/interactsh-client@latest
```

配置 PDCP_API_KEY

> Get api key at [https://cloud.projectdiscovery.io](https://cloud.projectdiscovery.io/)

```
┌──(root㉿kali)-[~]
└─$ interactsh-client -auth
```

## 2. Usage

经典运行

```
┌──(root㉿kali)-[~]
└─$ interactsh-client
```

后台运行

```
┌──(root㉿kali)-[~]
└─$ nohup interactsh-client > ~/nohup.log 2>&1 &
echo $! > ~/pid.log
```

---

**References**

- [interactsh](https://github.com/projectdiscovery/interactsh)