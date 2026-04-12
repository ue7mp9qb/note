OpenSSH is the premier connectivity tool for remote login with the SSH protocol. It encrypts all traffic to eliminate eavesdropping, connection hijacking, and other attacks.

## 1. Install

### 1.1. Windows

使用管理员权限运行 Powershell，检查是否安装 OpenSSH

```
PS C:\Windows\system32> Get-WindowsCapability -Online | Where-Object Name -like 'OpenSSH*'
```

```
Name  : OpenSSH.Client~~~~0.0.1.0
State : Installed

Name  : OpenSSH.Server~~~~0.0.1.0
State : NotPresent
```

安装 OpenSSH 客户端

```
PS C:\Windows\system32> Add-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0
```

安装 OpenSSH 服务端

```
PS C:\Windows\system32> Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
```

卸载 OpenSSH 客户端

```
PS C:\Windows\system32> Remove-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0
```

卸载 OpenSSH 服务端

```
PS C:\Windows\system32> Remove-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
```

## 2. Init

### 2.1. Windows

运行 Powershell，运行 SSH 服务

```
PS C:\Windows\system32> Start-Service -Name sshd
```

配置 SSH 服务开机自启动

```
PS C:\Windows\system32> Set-Service -Name sshd -StartupType 'Automatic'
```

查看 SSH 服务状态

```
PS C:\Windows\system32> Get-Service sshd
```

查看防火墙是否配置

```
PS C:\Windows\system32> if (!(Get-NetFirewallRule -Name "OpenSSH-Server-In-TCP" -ErrorAction SilentlyContinue | Select-Object Name, Enabled)) {
    Write-Output "Firewall Rule 'OpenSSH-Server-In-TCP' does not exist, creating it..."
    New-NetFirewallRule -Name 'OpenSSH-Server-In-TCP' -DisplayName 'OpenSSH Server (sshd)' -Enabled True -Direction Inbound -Protocol TCP -Action Allow -LocalPort 22
} else {
    Write-Output "Firewall rule 'OpenSSH-Server-In-TCP' has been created and exists."
}
```

## 3. Usage

### 3.1. 连接

连接指定用户

```
PS C:\Windows\system32> ssh null@127.0.0.1
```

删除指定公钥记录

```
PS C:\Windows\system32> ssh-keygen -R 127.0.0.1
```

删除所有公钥记录

```
PS C:\Windows\system32> Remove-Item C:\Users\null\.ssh\known_hosts
```

### 3.2. 配置密钥

生成私钥

```
PS C:\Windows\system32> ssh-keygen -t ed25519 -f C:\Users\null\.ssh\ssh_test
```

```
Generating public/private ed25519 key pair.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in C:\Users\null\.ssh\ssh_test
Your public key has been saved in C:\Users\null\.ssh\ssh_test.pub
The key fingerprint is:
SHA256:UBde/s2O/viAqb0hZxf5W1qO0kXpu9QayAK9NtwEJZc null@127.0.0.1
The key's randomart image is:
+--[ED25519 256]--+
|        . +.+.   |
|       . o *E    |
|      .   o .   .|
|       . . . . =.|
|        S . . =.o|
|         o = + *o|
|          B X.=oB|
|         . O.+o@o|
|          . oo*==|
+----[SHA256]-----+
```

查看私钥

```
PS C:\Windows\system32> ls C:\Users\null\.ssh\
```

```
Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        2024/10/24     17:17            444 ssh_test
-a----        2024/10/24     17:17             94 ssh_test.pub
```

### 3.3. 保存私钥

配置 ssh-agent 服务开机自启

```
PS C:\Windows\system32> Get-Service ssh-agent | Set-Service -StartupType Automatic
```

运行 ssh-agent 服务

```
PS C:\Windows\system32> Start-Service ssh-agent
```

查看 ssh-agent 服务状态

```
PS C:\Windows\system32> Get-Service ssh-agent
```

```
Status   Name               DisplayName
------   ----               -----------
Running  ssh-agent          OpenSSH Authentication Agent
```

将私钥载入 ssh-agent

```
PS C:\Windows\system32> ssh-add $env:USERPROFILE\.ssh\ssh_test
```

```
Enter passphrase for C:\Users\null\.ssh\ssh_test:123456
Identity added: C:\Users\null\.ssh\ssh_test (null@127.0.0.1)
```

> 添加到 ssh-agent 后，可将私钥文件备份到一个安全位置，然后将其从本地系统中删除

列出保存的私钥

```
PS C:\Windows\system32> ssh-add -l
```

从 ssh-agent 中移除指定私钥

```
PS C:\Windows\system32> ssh-add -d $env:USERPROFILE\.ssh\ssh_test
```

移除所有私钥

```
PS C:\Windows\system32> ssh-add -D
```

### 3.4. 部署公钥

复制公钥内容到剪切板

```
PS C:\Windows\system32> Get-Content $env:USERPROFILE\.ssh\ssh_test.pub | Set-Clipboard
```

设置 $authorizedKey 变量复制公钥内容到剪切板

```
PS C:\Windows\system32> $authorizedKey = Get-Content -Path $env:USERPROFILE\.ssh\ssh_test.pub
```

设置 $remotePowershell 变量将公钥内容复制到服务器上的 authorized_keys 文件中

```
PS C:\Windows\system32> $remotePowershell = "powershell New-Item -Force -ItemType Directory -Path $env:USERPROFILE\.ssh; Add-Content -Force -Path $env:USERPROFILE\.ssh\authorized_keys -Value '$authorizedKey'"
```

连接服务器并运行 $remotePowerShell 变量

```
PS C:\Windows\system32> ssh null@127.0.0.1 $remotePowershell
```

```
null@127.0.0.1's password:


    Ŀ¼: C:\Users\null


Mode                 LastWriteTime         Length Name

----                 -------------         ------ ----

d-----          2024/6/7     22:00                .ssh
```

> 客户端公钥 `C:\Users\null\.ssh\ssh_test.pub` 的内容需放置在服务器上的 `~/.ssh/authorized_keys` 文件中

### 3.5. 启用密钥登录

服务端启用密钥登录并禁止密码登录

```
C:\ProgramData\ssh\sshd_config
```

```
34 PubkeyAuthentication yes
51 PasswordAuthentication no
```

指定 `authorized_keys` 文件路径

```
87 Match Group administrators
88        AuthorizedKeysFile .ssh/authorized_keys
```

> linux 服务端启用密钥登录并禁止密码登录
>
> ```
> root@server:~# vim /etc/ssh/sshd_config
> ```
> 
>```
> 38 PubkeyAuthentication yes
> 57 PasswordAuthentication no
> ```

运行 Powershell 重启 SSH 服务

```
PS C:\Windows\system32> Restart-Service sshd
```

> Kali 重启 SSH 服务
>
> ```
> ┌──(root㉿kali)-[~]
> └─# systemctl restart sshd.service
> ```

---

References

- [OpenSSH](https://www.openssh.com/)

