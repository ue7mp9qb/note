SMB 是一种用于局域网中文件共享和设备通信的网络协议.

使用网线互相连接的电脑可开启公用文件共享, 但网段, 网关, DNS 配置要相同.

## 1. 准备

在 “控制面板” 禁用 “SMB 1.0/CIFS 文件共享支持”

![](./../../../../../images/%E9%85%8D%E7%BD%AE_SMB_%E6%96%87%E4%BB%B6%E5%85%B1%E4%BA%AB/%E5%9C%A8%20%E2%80%9C%E6%8E%A7%E5%88%B6%E9%9D%A2%E6%9D%BF%E2%80%9D%20%E7%A6%81%E7%94%A8%20%E2%80%9CSMB%201.0%252FCIFS%20%E6%96%87%E4%BB%B6%E5%85%B1%E4%BA%AB%E6%94%AF%E6%8C%81%E2%80%9D.png)

> 这个版本的 SMB 已不再安全
>
> Windows 10/11 默认禁用

## 2. 步骤

1. 新建一个用户专门用于共享文件的授权, 并设置此用户的权限; 
2. 设置网络共享和系统安全的相关设置; 
3. 开启共享, 授权指定用户; 
4. 手动添加证书, 采用“映射网络驱动器”的方式访问共享.

### 2.1. 新建用户

使用服务端

按 `Win + X` 键在弹出的菜单里选择 “计算机管理”

![](./../../../../../images/%E9%85%8D%E7%BD%AE_SMB_%E6%96%87%E4%BB%B6%E5%85%B1%E4%BA%AB/%E6%8C%89%20%60Win%20+%20X%60%20%E9%94%AE%E5%9C%A8%E5%BC%B9%E5%87%BA%E7%9A%84%E8%8F%9C%E5%8D%95%E9%87%8C%E9%80%89%E6%8B%A9%20%E2%80%9C%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%AE%A1%E7%90%86%E2%80%9D.png)

在 “本地用户和组” 的用户项目中右键添加新用户

![](./../../../../../images/%E9%85%8D%E7%BD%AE_SMB_%E6%96%87%E4%BB%B6%E5%85%B1%E4%BA%AB/%E5%9C%A8%20%E2%80%9C%E6%9C%AC%E5%9C%B0%E7%94%A8%E6%88%B7%E5%92%8C%E7%BB%84%E2%80%9D%20%E7%9A%84%E7%94%A8%E6%88%B7%E9%A1%B9%E7%9B%AE%E4%B8%AD%E5%8F%B3%E9%94%AE%E6%B7%BB%E5%8A%A0%E6%96%B0%E7%94%A8%E6%88%B7.png)

创建一个专门用于文件共享的用户

![](./../../../../../images/%E9%85%8D%E7%BD%AE_SMB_%E6%96%87%E4%BB%B6%E5%85%B1%E4%BA%AB/%E5%88%9B%E5%BB%BA%E4%B8%80%E4%B8%AA%E4%B8%93%E9%97%A8%E7%94%A8%E4%BA%8E%E6%96%87%E4%BB%B6%E5%85%B1%E4%BA%AB%E7%9A%84%E7%94%A8%E6%88%B7.png)

### 2.2. 安全设置

按 `Win + R` 在“运行”窗口中输入 `gpedit.msc` 打开“本地组策略编辑器”

![](./../../../../../images/%E9%85%8D%E7%BD%AE_SMB_%E6%96%87%E4%BB%B6%E5%85%B1%E4%BA%AB/%E6%8C%89%20%60Win%20+%20R%60%20%E5%9C%A8%E2%80%9C%E8%BF%90%E8%A1%8C%E2%80%9D%E7%AA%97%E5%8F%A3%E4%B8%AD%E8%BE%93%E5%85%A5%20%60gpedit.msc%60%20%E6%89%93%E5%BC%80%E2%80%9C%E6%9C%AC%E5%9C%B0%E7%BB%84%E7%AD%96%E7%95%A5%E7%BC%96%E8%BE%91%E5%99%A8%E2%80%9D.png)

禁用不安全的来宾登录

![](./../../../../../images/%E9%85%8D%E7%BD%AE_SMB_%E6%96%87%E4%BB%B6%E5%85%B1%E4%BA%AB/%E7%A6%81%E7%94%A8%E4%B8%8D%E5%AE%89%E5%85%A8%E7%9A%84%E6%9D%A5%E5%AE%BE%E7%99%BB%E5%BD%95.png)

在安全选项中配置如下策略

```
Microsoft 网络服务器：对通信进行数字签名(始终)：已禁用
Microsoft 网络客户端：对通信进行数字签名(如果服务器允许)：已启用
Microsoft 网络客户端：对通信进行数字签名(始终)：已禁用
设备：防止用户安装打印机驱动程序：已禁用
网络访问：本地账户的共享和安全模型：经典-对本地用户进行身份验证, 不改变其本来身份
```

![](./../../../../../images/%E9%85%8D%E7%BD%AE_SMB_%E6%96%87%E4%BB%B6%E5%85%B1%E4%BA%AB/%E5%9C%A8%E5%AE%89%E5%85%A8%E9%80%89%E9%A1%B9%E4%B8%AD%E9%85%8D%E7%BD%AE%E5%A6%82%E4%B8%8B%E7%AD%96%E7%95%A5.png)

按 `Win + R` 在 “运行” 窗口中输入 `secpol.msc` 打开 “本地安全策略”

![](./../../../../../images/%E9%85%8D%E7%BD%AE_SMB_%E6%96%87%E4%BB%B6%E5%85%B1%E4%BA%AB/%E6%8C%89%20%60Win%20+%20R%60%20%E5%9C%A8%20%E2%80%9C%E8%BF%90%E8%A1%8C%E2%80%9D%20%E7%AA%97%E5%8F%A3%E4%B8%AD%E8%BE%93%E5%85%A5%20%60secpol.msc%60%20%E6%89%93%E5%BC%80%20%E2%80%9C%E6%9C%AC%E5%9C%B0%E5%AE%89%E5%85%A8%E7%AD%96%E7%95%A5%E2%80%9D.png)

授予新用户 “从网络访问此计算机” 的权限

![](./../../../../../images/%E9%85%8D%E7%BD%AE_SMB_%E6%96%87%E4%BB%B6%E5%85%B1%E4%BA%AB/%E6%8E%88%E4%BA%88%E6%96%B0%E7%94%A8%E6%88%B7%20%E2%80%9C%E4%BB%8E%E7%BD%91%E7%BB%9C%E8%AE%BF%E9%97%AE%E6%AD%A4%E8%AE%A1%E7%AE%97%E6%9C%BA%E2%80%9D%20%E7%9A%84%E6%9D%83%E9%99%90.png)

拒绝新用户本地登录和通过远程桌面服务登录

![](./../../../../../images/%E9%85%8D%E7%BD%AE_SMB_%E6%96%87%E4%BB%B6%E5%85%B1%E4%BA%AB/%E6%8B%92%E7%BB%9D%E6%96%B0%E7%94%A8%E6%88%B7%E6%9C%AC%E5%9C%B0%E7%99%BB%E5%BD%95%E5%92%8C%E9%80%9A%E8%BF%87%E8%BF%9C%E7%A8%8B%E6%A1%8C%E9%9D%A2%E6%9C%8D%E5%8A%A1%E7%99%BB%E5%BD%95.png)

隐藏 “快速用户切换” 的入口点

![](./../../../../../images/%E9%85%8D%E7%BD%AE_SMB_%E6%96%87%E4%BB%B6%E5%85%B1%E4%BA%AB/%E9%9A%90%E8%97%8F%E2%80%9C%E5%BF%AB%E9%80%9F%E7%94%A8%E6%88%B7%E5%88%87%E6%8D%A2%E2%80%9D%E7%9A%84%E5%85%A5%E5%8F%A3%E7%82%B9.png)

在 “网络和共享中心” 中打开高级共享设置

![](./../../../../../images/%E9%85%8D%E7%BD%AE_SMB_%E6%96%87%E4%BB%B6%E5%85%B1%E4%BA%AB/%E5%9C%A8%20%E2%80%9C%E7%BD%91%E7%BB%9C%E5%92%8C%E5%85%B1%E4%BA%AB%E4%B8%AD%E5%BF%83%E2%80%9D%20%E4%B8%AD%E6%89%93%E5%BC%80%E9%AB%98%E7%BA%A7%E5%85%B1%E4%BA%AB%E8%AE%BE%E7%BD%AE.png)

关闭网络发现, 启用文件和打印机共享

![](./../../../../../images/%E9%85%8D%E7%BD%AE_SMB_%E6%96%87%E4%BB%B6%E5%85%B1%E4%BA%AB/%E5%85%B3%E9%97%AD%E7%BD%91%E7%BB%9C%E5%8F%91%E7%8E%B0,%20%E5%90%AF%E7%94%A8%E6%96%87%E4%BB%B6%E5%92%8C%E6%89%93%E5%8D%B0%E6%9C%BA%E5%85%B1%E4%BA%AB.png)

对所有网络进行共享配置

![](./../../../../../images/%E9%85%8D%E7%BD%AE_SMB_%E6%96%87%E4%BB%B6%E5%85%B1%E4%BA%AB/%E5%AF%B9%E6%89%80%E6%9C%89%E7%BD%91%E7%BB%9C%E8%BF%9B%E8%A1%8C%E5%85%B1%E4%BA%AB%E9%85%8D%E7%BD%AE.png)

### 2.3. 开启文件共享

右键需要共享的文件查看属性

![](./../../../../../images/%E9%85%8D%E7%BD%AE_SMB_%E6%96%87%E4%BB%B6%E5%85%B1%E4%BA%AB/%E5%8F%B3%E9%94%AE%E9%9C%80%E8%A6%81%E5%85%B1%E4%BA%AB%E7%9A%84%E6%96%87%E4%BB%B6%E6%9F%A5%E7%9C%8B%E5%B1%9E%E6%80%A7.png)

选择高级共享

![](./../../../../../images/%E9%85%8D%E7%BD%AE_SMB_%E6%96%87%E4%BB%B6%E5%85%B1%E4%BA%AB/%E9%80%89%E6%8B%A9%E9%AB%98%E7%BA%A7%E5%85%B1%E4%BA%AB.png)

共享此文件夹并配置权限

![](./../../../../../images/%E9%85%8D%E7%BD%AE_SMB_%E6%96%87%E4%BB%B6%E5%85%B1%E4%BA%AB/%E5%85%B1%E4%BA%AB%E6%AD%A4%E6%96%87%E4%BB%B6%E5%A4%B9%E5%B9%B6%E9%85%8D%E7%BD%AE%E6%9D%83%E9%99%90.png)

删除 Everyone 用户

![](./../../../../../images/%E9%85%8D%E7%BD%AE_SMB_%E6%96%87%E4%BB%B6%E5%85%B1%E4%BA%AB/%E5%88%A0%E9%99%A4%20Everyone%20%E7%94%A8%E6%88%B7.png)

选择添加组或用户

![](./../../../../../images/%E9%85%8D%E7%BD%AE_SMB_%E6%96%87%E4%BB%B6%E5%85%B1%E4%BA%AB/%E9%80%89%E6%8B%A9%E6%B7%BB%E5%8A%A0%E7%BB%84%E6%88%96%E7%94%A8%E6%88%B7.png)

添加新用户

![](./../../../../../images/%E9%85%8D%E7%BD%AE_SMB_%E6%96%87%E4%BB%B6%E5%85%B1%E4%BA%AB/%E6%B7%BB%E5%8A%A0%E6%96%B0%E7%94%A8%E6%88%B7.png)

为新用户配置共享权限

![](./../../../../../images/%E9%85%8D%E7%BD%AE_SMB_%E6%96%87%E4%BB%B6%E5%85%B1%E4%BA%AB/%E4%B8%BA%E6%96%B0%E7%94%A8%E6%88%B7%E9%85%8D%E7%BD%AE%E5%85%B1%E4%BA%AB%E6%9D%83%E9%99%90.png)

记住网络路径

![](./../../../../../images/%E9%85%8D%E7%BD%AE_SMB_%E6%96%87%E4%BB%B6%E5%85%B1%E4%BA%AB/%E8%AE%B0%E4%BD%8F%E7%BD%91%E7%BB%9C%E8%B7%AF%E5%BE%84.png)

### 2.4. 访问共享文件

### 2.4.1. Debian

创建挂载目录并安装工具

```
┌──(nemo@debian)-[~]
└─$ mkdir -p ~/smb && sudo apt install -y cifs-utils
```

将共享文件夹挂载到本地

```
┌──(nemo@debian)-[~]
└─$ sudo mount -t cifs //192.168.0.57/D /home/nemo/Documents/smb -o username=share
```

取消挂载

```
┌──(nemo@debian)-[~]
└─$ sudo umount /home/nemo/Documents/smb
```

### 2.4.2. Windows

打开凭据管理器

```
Control Panel\User Accounts\Credential Manager
```

添加 Windows 凭据

![](./../../../../../images/%E9%85%8D%E7%BD%AE_SMB_%E6%96%87%E4%BB%B6%E5%85%B1%E4%BA%AB/%E6%B7%BB%E5%8A%A0%20Windows%20%E5%87%AD%E6%8D%AE.png)

键入网站地址（或网络位置）和凭据

> 要确保主机在同一局域网下, 若其中一台使用路由器则要关闭 DHCP

![](./../../../../../images/%E9%85%8D%E7%BD%AE_SMB_%E6%96%87%E4%BB%B6%E5%85%B1%E4%BA%AB/%E9%94%AE%E5%85%A5%E7%BD%91%E7%AB%99%E5%9C%B0%E5%9D%80%EF%BC%88%E6%88%96%E7%BD%91%E7%BB%9C%E4%BD%8D%E7%BD%AE%EF%BC%89%E5%92%8C%E5%87%AD%E6%8D%AE.png)

在此电脑右键映射网络驱动器

![](./../../../../../images/%E9%85%8D%E7%BD%AE_SMB_%E6%96%87%E4%BB%B6%E5%85%B1%E4%BA%AB/%E5%9C%A8%E6%AD%A4%E7%94%B5%E8%84%91%E5%8F%B3%E9%94%AE%E6%98%A0%E5%B0%84%E7%BD%91%E7%BB%9C%E9%A9%B1%E5%8A%A8%E5%99%A8.png)

映射网络路径

![](./../../../../../images/%E9%85%8D%E7%BD%AE_SMB_%E6%96%87%E4%BB%B6%E5%85%B1%E4%BA%AB/%E6%98%A0%E5%B0%84%E7%BD%91%E7%BB%9C%E8%B7%AF%E5%BE%84.png)

> 网络文件夹格式为
>
> ```
> \\ip\path
> ```

> 若配置完成后无法连接可尝试重启计算机

### 2.5. 取消共享文件

按 `Win + X` 键在弹出的菜单里选择 “计算机管理”

![](./../../../../../images/%E9%85%8D%E7%BD%AE_SMB_%E6%96%87%E4%BB%B6%E5%85%B1%E4%BA%AB/%E6%8C%89%20%60Win%20+%20X%60%20%E9%94%AE%E5%9C%A8%E5%BC%B9%E5%87%BA%E7%9A%84%E8%8F%9C%E5%8D%95%E9%87%8C%E9%80%89%E6%8B%A9%20%E2%80%9C%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%AE%A1%E7%90%86%E2%80%9D.png)

找到共享的文件夹, 停止共享即可

![](./../../../../../images/%E9%85%8D%E7%BD%AE_SMB_%E6%96%87%E4%BB%B6%E5%85%B1%E4%BA%AB/%E6%89%BE%E5%88%B0%E5%85%B1%E4%BA%AB%E7%9A%84%E6%96%87%E4%BB%B6%E5%A4%B9,%20%E5%81%9C%E6%AD%A2%E5%85%B1%E4%BA%AB%E5%8D%B3%E5%8F%AF.png)

---

References

- [Windows 10/ 11 下安全并正确地使用 SMB 共享](https://post.smzdm.com/p/akxwkxqk/)

