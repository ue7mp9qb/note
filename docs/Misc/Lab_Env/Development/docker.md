A safer container ecosystem.

## 1. Install

安装

```
┌──(root@kali)-[~]
└─$ apt install -y docker.io docker-compose
```

## 2. Init

将当前用户追加到 `docker` 用户组

```
┌──(root@kali)-[~]
└─$ usermod -aG docker $USER && newgrp docker
```

配置代理

```
┌──(root@kali)-[~]
└─$ mkdir -p /etc/systemd/system/docker.service.d \
&& tee /etc/systemd/system/docker.service.d/http-proxy.conf > /dev/null << 'EOF'
[Service]
Environment="HTTP_PROXY=sock5://10.0.2.2:10808"
Environment="HTTPS_PROXY=socks5://10.0.2.2:10808"
Environment="NO_PROXY=localhost,10.0.2.2,192.168.0.0/16,::1"
EOF
```

重启 Docker 加载配置

```
┌──(root@kali)-[~]
└─$ systemctl daemon-reload && systemctl restart docker.service
```

## 3. Usage

拉取镜像

```
┌──(root@kali)-[~]
└─$ docker pull <URL>
```

---

References

- [docker](https://www.docker.com/)

