Ufw stands for Uncomplicated Firewall, and is program for managing a netfilter firewall. It provides a command line interface and aims to be uncomplicated and easy to use.

## 1. Install

安装

```
null@debian:~$ sudo apt install -y ufw
```

## 2. Init

开放 SSH 端口

```
null@debian:~$ sudo ufw allow 22
```

启用防火墙

```
null@debian:~$ sudo ufw enable
```

## 3. Usage

```
# 管理防火墙
sudo ufw enable  # 启用
sudo ufw disable # 禁用

# 显示当前策略
sudo ufw status verbose

# 指定端口的入站流量
sudo ufw allow <port> # 放行
sudo ufw deny <port>  # 丢弃

# 列出所有规则记录
sudo ufw status numbered

# 删除指定规则记录
sudo ufw delete <NUM>

# 指定 IP 的入站流量
sudo ufw deny from <ip>  # 丢弃
sudo ufw allow from <ip> # 放行

# 指定 IP 的出战流量
sudo ufw deny to <ip>  # 丢弃
sudo ufw allow to <ip> # 放行

# 所有出战流量
sudo ufw default deny outgoing  # 丢弃
sudo ufw default allow outgoing # 放行
```

---

**References**

- [ufw](https://launchpad.net/ufw)

