Vulfocus 是一个漏洞集成平台, 将漏洞环境 docker 镜像, 放入即可使用, 开箱即用.

## 1. Install

Run

```
┌──(root㉿kali)-[~]
└─$ docker run -d --restart always -p 60088:80 -v /var/run/docker.sock:/var/run/docker.sock -e VUL_IP=127.0.0.1 vulfocus/vulfocus
```

## 2. Init

端口转发

```
/Machines/VM/Settings/Expert/Network/Adapter 1/Port Forwarding
```

| Name     | Host Port | Guest Port |
| -------- | --------- | ---------- |
| Vulfocus | 60088     | 60088      |

Visit

> http://127.0.0.1:60088

Login

```
admin:admin
```

修改系统配置

![修改系统配置](./../../../images/vulfocus/%E4%BF%AE%E6%94%B9%E7%B3%BB%E7%BB%9F%E9%85%8D%E7%BD%AE.png)

|         Vulfocus          |
| :-----------------------: |
|   vulfocus/dvwa:latest    |
|  vulfocus/pikachu:latest  |
| vulfocus/xss-labs:latest  |
| vulfocus/sqli-labs:latest |
| glzjin/upload-labs:latest |

## 3. Usage

同步镜像, 下载靶场
![同步镜像, 下载靶场](./../../../images/vulfocus/%E5%90%8C%E6%AD%A5%E9%95%9C%E5%83%8F,%20%E4%B8%8B%E8%BD%BD%E9%9D%B6%E5%9C%BA.png)

启动靶场

![启动靶场](./../../../images/vulfocus/%E5%90%AF%E5%8A%A8%E9%9D%B6%E5%9C%BA.png)

镜像信息

![镜像信息](./../../../images/vulfocus/%E9%95%9C%E5%83%8F%E4%BF%A1%E6%81%AF.png)

Port Forwarding Rules

| Name | Host Port | Guest Port |
| ---- | --------- | ---------- |
| DVWA | 39083     | 39083      |

Visit

> http://127.0.0.1:39083

停止靶场

![停止靶场](./../../../images/vulfocus/%E5%81%9C%E6%AD%A2%E9%9D%B6%E5%9C%BA.png)

---

**References**

- [vulfocus](https://fofapro.github.io/vulfocus/#/)

