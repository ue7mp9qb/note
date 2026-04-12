Daemon to ban hosts that cause multiple authentication errors

## 1. Install

安装

```
null@debian:~$ sudo apt install -y fail2ban
```

## 2. Init

修改配置文件

```
null@debian:~$ sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local \
&& sudo nano -l /etc/fail2ban/jail.local
```

```
279 mode   = aggressive
```

运行 `fail2ban` 服务

```
null@debian:~$ sudo systemctl enable --now fail2ban.service
```

## 3. Usage

查看封禁统计

```
null@debian:~$ sudo fail2ban-client status sshd
```

查看封禁的 IP

```
null@debian:~$ sudo fail2ban-client status sshd | grep -A 5 "IP list"
```

移除封禁的 IP

```
null@debian:~$ sudo fail2ban-client set sshd unbanip [ban_ip]
```

---

References

- [fail2ban](https://github.com/fail2ban/fail2ban)
- [fail2ban配置教程 有效防止服务器被暴力破解](https://www.wanpeng.life/1672.html)
