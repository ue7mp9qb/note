Let KeePassXC safely store your passwords and auto-fill them into your favorite apps, so you can forget all about them.

## 1. Init

配置 ssh-agent 服务开机自启

```powershell
PS C:\Windows\system32> Get-Service ssh-agent | Set-Service -StartupType Automatic
```

运行 ssh-agent 服务

```powershell
PS C:\Windows\system32> Start-Service ssh-agent
```

查看 ssh-agent 服务状态

```powershell
PS C:\Windows\system32> Get-Service ssh-agent
```

```
Status   Name               DisplayName
------   ----               -----------
Running  ssh-agent          OpenSSH Authentication Agent
```

导入应用设置

![导入应用设置](./../../../../../image/KeePassXC/%E5%AF%BC%E5%85%A5%E5%BA%94%E7%94%A8%E8%AE%BE%E7%BD%AE.png)

导入插件设置

![导入插件设置](./../../../../../image/KeePassXC/%E5%AF%BC%E5%85%A5%E6%8F%92%E4%BB%B6%E8%AE%BE%E7%BD%AE.png)

## 2. Usage

### 2.1. TOTP

获取 Secret Key

![获取 Secret Key](./../../../../../image/KeePassXC/%E8%8E%B7%E5%8F%96%20Secret%20Key.png)

设置 TOTP

![设置 TOTP](./../../../../../image/KeePassXC/%E8%AE%BE%E7%BD%AE%20TOTP.png)

使用默认的 RFC 6238 添加 Secret Key 即可

![使用默认的 RFC 6238 添加 Secret Key 即可](./../../../../../image/KeePassXC/%E4%BD%BF%E7%94%A8%E9%BB%98%E8%AE%A4%E7%9A%84%20RFC%206238%20%E6%B7%BB%E5%8A%A0%20Secret%20Key%20%E5%8D%B3%E5%8F%AF.png)

### 2.2. Passkey

确保插件的相关功能开启

![确保插件的相关功能开启](./../../../../../image/KeePassXC/%E7%A1%AE%E4%BF%9D%E6%8F%92%E4%BB%B6%E7%9A%84%E7%9B%B8%E5%85%B3%E5%8A%9F%E8%83%BD%E5%BC%80%E5%90%AF.png)

添加通行密钥

![添加通行密钥](./../../../../../image/KeePassXC/%E6%B7%BB%E5%8A%A0%E9%80%9A%E8%A1%8C%E5%AF%86%E9%92%A5.png)

添加到现有条目

![添加到现有条目](./../../../../../image/KeePassXC/%E6%B7%BB%E5%8A%A0%E5%88%B0%E7%8E%B0%E6%9C%89%E6%9D%A1%E7%9B%AE.png)

保存到指定条目

![保存到指定条目](./../../../../../image/KeePassXC/%E4%BF%9D%E5%AD%98%E5%88%B0%E6%8C%87%E5%AE%9A%E6%9D%A1%E7%9B%AE.png)

在网站为这个 Passkey 命名

![在网站为这个 Passkey 命名](./../../../../../image/KeePassXC/%E5%9C%A8%E7%BD%91%E7%AB%99%E4%B8%BA%E8%BF%99%E4%B8%AA%20Passkey%20%E5%91%BD%E5%90%8D.png)

设置 Passkeys 为多因素验证的首选方法

![设置 Passkeys 为多因素验证的首选方法](./../../../../../image/KeePassXC/%E8%AE%BE%E7%BD%AE%20Passkeys%20%E4%B8%BA%E5%A4%9A%E5%9B%A0%E7%B4%A0%E9%AA%8C%E8%AF%81%E7%9A%84%E9%A6%96%E9%80%89%E6%96%B9%E6%B3%95.png)

---

**References**

- [KeePassXC](https://keepassxc.org/)

