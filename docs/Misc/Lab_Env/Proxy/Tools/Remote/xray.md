Xray, Penetrates Everything. Also the best v2ray-core. Where the magic happens. An open platform for various uses.

## 1. Install

安装

```
┌──(root㉿kali)-[~]
└─# bash -c "$(curl -fsSL https://github.com/XTLS/Xray-install/raw/main/install-release.sh)" @ install
```

> 记得使用 UFW 开放端口

## 2. Init

将 [v2ray-agent](https://github.com/mack-a/v2ray-agent) 生成的客户端配置文件写入 `config.json` 

```
┌──(root㉿kali)-[~]
└─# nano /usr/local/etc/xray/config.json && systemctl restart xray.service
```

---

**References**

- [Xray-install](https://github.com/XTLS/Xray-install)

