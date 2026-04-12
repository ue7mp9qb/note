搭建私人 DNS 服务器，避免国际 DNS 服务器被 [GFW](https://en.greatfire.org/) 污染.

## 1. Deply

安装 Bind9 用于部署 DNS 服务器

```
null@debian:~$ sudo apt install -y bind9
```

配置 Bind9 转发上游 DNS

```
null@debian:~$ sudo nano /etc/bind/named.conf.options
```

```
options {
        directory "/var/cache/bind";

        forwarders {
            8.8.8.8;  // Google Public DNS
            1.1.1.1;  // Cloudflare DNS
        };

        forward only;

        allow-query { any; };

        dnssec-validation auto;

        listen-on-v6 { any; } ;

};

```

使 Bind9 开机自启

```
null@debian:~$ sudo systemctl enable --now named.service
```

在本地计算机中验证 DNS 服务器是否可用

```
null@debian:~$ dig @<dns_ip> mit.edu
```

> ==记得使用 UFW 开放端口==

---

References

- [BIND 9](https://gitlab.isc.org/isc-projects/bind9)
- [debian配置BIND DNS服务器](https://blog.csdn.net/qq_51470638/article/details/138235472)

