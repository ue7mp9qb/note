Debian is a complete Free Operating System!

## 1. Init

SSH to `root` 

```
PS C:\Users\null> ssh root@127.0.0.1
```

==debian 12== 需要更新仓库格式

```
root@debian:~# sed -i 's/non-free$/non-free non-free-firmware/g' /etc/apt/sources.list
```

Update the system

```
root@debian:~# apt update && apt full-upgrade \
&& apt clean && apt autoremove --purge
```

Change the password of root and Create a new user

```
root@debian:~# passwd && adduser null
```

Install common tools

```
root@debian:~# apt install -y nano passwd sudo curl vim unzip tree \
&& usermod -aG sudo null
```

Keep SSH session alive

```
root@debian:~# sed -i 's/#ClientAliveInterval 0/ClientAliveInterval 60/' /etc/ssh/sshd_config \
&& sed -i 's/#ClientAliveCountMax 3/ClientAliveCountMax 60/' /etc/ssh/sshd_config \
&& systemctl reload ssh.service
```

Exit

```
root@debian:~# exit
```

## 2. Deploy

SSH to `null` 

```
PS C:\Users\null> ssh null@127.0.0.1
```

|                            Server                            |
| :----------------------------------------------------------: |
| [配置 SSH 密钥对连接](https://keithpeck177271.gitbook.io/notes/docs/misc/lab_env/pei-zhi-ssh-mi-yao-dui-lian-jie) |
| [ufw](https://keithpeck177271.gitbook.io/notes/misc/security-response/firewall/ufw) |
| [fail2ban](https://keithpeck177271.gitbook.io/notes/misc/security-response/ban/fail2ban) |
| [xray](https://github.com/jadensalas469466/notes/blob/main/docs/Misc/Lab_Env/Proxy/Tools/Remote/xray.md) |

## 3. Usage

### 3.1. 安装桌面环境

安装依赖

```
root@debian:~# apt install -y gnome-shell gdm3 gnome-terminal nautilus
```

Restart

```
root@debian:~# reboot
```

Wait for the VM to start up, then insert Guest Additions CD image

挂载增强工具

```
┌──(null@debian)-[~]
└─$ sudo mount /dev/cdrom /mnt
```

安装增强工具

```
┌──(null@debian)-[~]
└─$ sudo bash /mnt/VBoxLinuxAdditions.run
```

Custom Shortcuts

```
    Name: terminal
 Command: gnome-terminal
Shortcut: Ctrl + Alt +T
```

### 3.2. 配置无线网卡驱动

下载驱动

```
┌──(null@debian)-[~]
└─$ git clone https://github.com/aircrack-ng/rtl8812au.git
```

安装驱动

```
┌──(null@debian)-[~]
└─$ cd ./rtl8812au && sudo make dkms_install
```

卸载驱动

```
┌──(null@debian)-[~]
└─$ cd ./rtl8812au && sudo make dkms_remove
```

### 3.3. 挂载共享文件

安装增强工具后设置共享文件夹 `Share Folder` 并创建挂载目录

```
┌──(null@debian)-[~]
└─$ mkdir -p ~/share
```

挂载共享文件夹

```
┌──(null@debian)-[~]
└─$ sudo mount -t vboxsf <Share Folder> ~/share
```

取消挂载

```
┌──(null@debian)-[~]
└─$ sudo umount ~/share
```

### 3.4. 历史命令

擦除历史命令

```
┌──(null@debian)-[~]
└─# shred -z ~/.bash_history ~/.zsh_history
```

### 3.5. 后台运行

将命令行程序放在后台运行, 即使 SSH 断开连接也不会终止运行

```
┌──(null@debian)-[~]
└─$ nohup <command> > ~/<command>-nohup.log 2>&1 &
echo $! > ~/<command>-pid.log
```

实时查看日志

```
┌──(null@debian)-[~]
└─$ tail -f ~/<command>-nohup.log
```

查看 PID

```
┌──(null@debian)-[~]
└─$ cat ~/<command>-pid.log
```

终止进程

```
┌──(null@debian)-[~]
└─$ kill -15 <PID> # 请求终止

┌──(null@debian)-[~]
└─$ kill -9 <PID>  # 强制终止
```

### 3.6. SSH 配置

SSH 配置文件

```
root@debian:~# sudo nano -l /etc/ssh/sshd_config
```

root 用户登录 (默认允许以密钥对登录)

```
33 #PermitRootLogin prohibit-password # 允许 root 用户以密钥对登录
33 PermitRootLogin prohibit-password  # 允许 root 用户以密钥对登录
33 PermitRootLogin yes                # 允许 root 用户以密钥对或密码登录
33 PermitRootLogin no                 # 禁止 root 用户登录
```

普通用户密钥对登录 (默认允许以密钥对登录)

```
38 #PubkeyAuthentication yes # 允许普通用户以密钥对登录
38 PubkeyAuthentication yes  # 允许普通用户以密钥对登录
```

普通用户密码登录 (默认允许以密码登录)

```
57 #PasswordAuthentication yes # 允许普通用户以密码登录
57 PasswordAuthentication yes  # 允许普通用户以密码登录
57 PasswordAuthentication no   # 禁止普通用户以密码登录
```

重启 SSH 服务

```
root@debian:~# systemctl restart ssh.service
```

### 3.7. 配置网络

**CLI**

查看网络接口

```
root@debian:~# ip link
```

安装依赖

```
root@debian:~# apt install -y systemd-resolved
```

配置服务

```
root@debian:~# systemctl disable NetworkManager.service || true \
&& systemctl disable networking.service \
&& systemctl enable systemd-networkd.service \
&& systemctl enable systemd-resolved.service
```

动态网络

```
root@debian:~# cat << 'EOF' > /etc/systemd/network/custom.network
[Match]
Name=en*

[Network]
DHCP=yes
IPv6AcceptRA=no
DNS=8.8.8.8
DNS=8.8.4.4
EOF
```

静态网络

```
root@debian:~# cat << 'EOF' > /etc/systemd/network/custom.network
[Match]
Name=en*

[Network]
Address=192.168.1.206/24
Gateway=192.168.1.1
IPv6AcceptRA=no
DNS=8.8.8.8
DNS=8.8.4.4
EOF
```

Restart

```
root@debian:~# reboot
```

### 3.8. 创建链接

创建软链接以全局运行

```
┌──(null@debian)-[~]
└─$ ln -sf ~/.local/bin/app /usr/local/bin/app
```

### 3.9. Download directory

Create download directory

```
null@debian:~$ sudo mkdir -p /var/www/html/exploit \
&& sudo chown -R www-data:www-data /var/www/html/exploit \
&& sudo chmod 2755 /var/www/html/exploit
```

> Grant download permissions every time a file is added to this directory
>
> ```
> ┌──(null@debian)-[~]
> └─# sudo chmod 644 /var/www/html/exploit/*
> ```

### 3.10. 创建新用户

创建用户

```
sudo adduser null
```

---

References

- [Debian](https://www.debian.org/)
- [Debian Docs](https://www.debian.org/doc/)

