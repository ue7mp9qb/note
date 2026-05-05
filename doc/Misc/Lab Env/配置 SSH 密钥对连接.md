## 1. 生成密钥对

生成 SSH 密钥对, 并为私钥设置密码

```
PS C:\Users\null> ssh-keygen -t ed25519 -f C:\Users\null\.ssh\ssh_test
```

```
Generating public/private ed25519 key pair.
Enter passphrase (empty for no passphrase):123456
Enter same passphrase again:123456
```

查看 SSH 密钥对

```
PS C:\Users\null> ls C:\Users\null\.ssh\
```

```
Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        2024/10/24     21:44            444 ssh_test
-a----        2024/10/24     21:44             94 ssh_test.pub
```

## 2. 使 KeePassXC 连接 SSH Agent

### 2.1. Debian

添加到环境变量

```
┌──(nemo@debian)-[~]
└─$ echo 'export SSH_AUTH_SOCK=/run/user/1000/keyring/ssh' >> ~/.zshrc \
&& source ~/.zshrc
```

列出 ssh-agent 中保存的私钥

```
┌──(nemo@debian)-[~]
└─$ ssh-add -l
```

在 Debian 中启用 SSH Agent 集成

![在 Debian 中启用 SSH Agent 集成](./../../../image/%E9%85%8D%E7%BD%AE%20SSH%20%E5%AF%86%E9%92%A5%E5%AF%B9%E8%BF%9E%E6%8E%A5/%E5%9C%A8%20Debian%20%E4%B8%AD%E5%90%AF%E7%94%A8%20SSH%20Agent%20%E9%9B%86%E6%88%90.png)

### 2.2. Windows

运行 SSH Agent 服务

```
PS C:\Users\null> Start-Service ssh-agent
```

配置 SSH Agent 服务开机自启

```
PS C:\Users\null> Get-Service ssh-agent | Set-Service -StartupType Automatic
```

查看 SSH Agent 服务状态

```
PS C:\Users\null> Get-Service ssh-agent
```

```
Status   Name               DisplayName
------   ----               -----------
Running  ssh-agent          Openssh Authentication Agent
```

列出 SSH Agent 中保存的私钥

```
PS C:\Users\null> ssh-add -l
```

在 Windows 中启用 SSH Agent 集成

![在 Windows 中启用 SSH Agent 集成](./../../../image/%E9%85%8D%E7%BD%AE%20SSH%20%E5%AF%86%E9%92%A5%E5%AF%B9%E8%BF%9E%E6%8E%A5/%E5%9C%A8%20Windows%20%E4%B8%AD%E5%90%AF%E7%94%A8%20SSH%20Agent%20%E9%9B%86%E6%88%90.png)

## 3. 保存私钥到 KeePassXC

新建条目, 设置条目密码为私钥密码

![新建条目, 设置条目密码为私钥密码](./../../../image/%E9%85%8D%E7%BD%AE%20SSH%20%E5%AF%86%E9%92%A5%E5%AF%B9%E8%BF%9E%E6%8E%A5/%E6%96%B0%E5%BB%BA%E6%9D%A1%E7%9B%AE,%20%E8%AE%BE%E7%BD%AE%E6%9D%A1%E7%9B%AE%E5%AF%86%E7%A0%81%E4%B8%BA%E7%A7%81%E9%92%A5%E5%AF%86%E7%A0%81.png)

添加私钥到附件

![添加私钥到附件](./../../../image/%E9%85%8D%E7%BD%AE%20SSH%20%E5%AF%86%E9%92%A5%E5%AF%B9%E8%BF%9E%E6%8E%A5/%E6%B7%BB%E5%8A%A0%E7%A7%81%E9%92%A5%E5%88%B0%E9%99%84%E4%BB%B6.png)

从附件添加私钥到 SSH Agent

![从附件添加私钥到 SSH Agent](./../../../image/%E9%85%8D%E7%BD%AE%20SSH%20%E5%AF%86%E9%92%A5%E5%AF%B9%E8%BF%9E%E6%8E%A5/%E4%BB%8E%E9%99%84%E4%BB%B6%E6%B7%BB%E5%8A%A0%E7%A7%81%E9%92%A5%E5%88%B0%20SSH%20Agent.png)

## 4. 部署公钥到服务器

将公钥写入文件

```
null@debain:~$ mkdir -p -m 700 ~/.ssh \
&& touch ~/.ssh/authorized_keys \
&& chmod 600 ~/.ssh/authorized_keys \
&& nano ~/.ssh/authorized_keys
```

```
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIAhONuGnP2jmIemHuiaiNeMnSzn+RHzG9mNitLZcqUEc test@DESKTOP-0EN5L6
```

![将公钥写入文件](./../../../image/%E9%85%8D%E7%BD%AE%20SSH%20%E5%AF%86%E9%92%A5%E5%AF%B9%E8%BF%9E%E6%8E%A5/%E5%B0%86%E5%85%AC%E9%92%A5%E5%86%99%E5%85%A5%E6%96%87%E4%BB%B6.png)

## 5. 允许普通用户以密钥对登录

修改配置文件

```
null@debain:~$ sudo nano -l /etc/ssh/sshd_config
```

```
33 PermitRootLogin no        # 禁止 root 用户登录
38 PubkeyAuthentication yes  # 允许普通用户以密钥对登录
57 PasswordAuthentication no # 禁止普通用户以密码登录
```

重启 SSH 服务

```
null@debain:~$ sudo systemctl restart sshd.service
```

完成后可删除密钥对

---

**References**

- [适用于 Windows 的 OpenSSH 入门](https://learn.microsoft.com/zh-cn/windows-server/administration/openssh/openssh_install_firstuse?tabs=gui&pivots=windows-server-2025)
- [OpenSSH for Windows 中基于密钥的身份验证](https://learn.microsoft.com/zh-cn/windows-server/administration/openssh/openssh_keymanagement)