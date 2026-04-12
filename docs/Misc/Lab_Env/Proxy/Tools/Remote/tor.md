Tor protects your privacy on the internet by hiding the connection between your Internet address and the services you use. We believe Tor is reasonably secure, but please ensure you read the instructions and configure it properly.

## 1. Install

安装

```
┌──(sec@debian)-[~]
└─$ sudo apt install -y tor
```

## 2. Init

修改配置文件

```
┌──(sec@debian)-[~]
└─$ sudo tee -a /etc/tor/torrc > /dev/null << 'EOF'
MaxCircuitDirtiness 60
NewCircuitPeriod 60
HiddenServiceDir /var/lib/tor/hidden_service/
HiddenServicePort 80 127.0.0.1:80
EOF
```

配置开机自启并重启

```
┌──(sec@debian)-[~]
└─$ sudo systemctl enable tor.service && sudo systemctl restart tor.service
```

> 默认端口 127.0.0.1:9050

查看分配到的 Onion 域名

```
┌──(sec@debian)-[~]
└─$ sudo cat /var/lib/tor/hidden_service/hostname
```

```
example.onion
```

> Tor 会将来自 `example.onion` 的访问转发到本机的 80 端口

---

References

- [tor](https://gitlab.com/torproject/tor)
