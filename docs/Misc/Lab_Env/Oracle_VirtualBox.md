VirtualBox is a general-purpose full virtualization software for x86_64 hardware (with version 7.1 additionally for macOS/Arm), targeted at laptop, desktop, server and embedded use.

## 1. Init

创建目录文件

```
%UserProfile%\VirtualBox VMs\ISOs
```

设置虚拟机默认目录

```
/File/Preferences.../Basic/General/Default Machine Folder: C:\Users\null\VirtualBox VMs
```

手动代理配置

```
/File/Preferences.../Expert/Proxy/Manual Proxy Configuration: http://127.0.0.1:10808
```

### 1.1. 配置 Host-Only Networks

> Host-Only 用于虚拟机之间互相访问

创建一个 Host-only Networks

```
/File/Tools/Network/Create
```

启用 DHCP 服务

```
/File/Tools/Network/Properties/DHCP Server/[√] Enable Server
```

## 2. Usage

端口转发

```
/Machines/VM/Settings/Expert/Network/Adapter 1/Port Forwarding
```

> 推荐设定范围为: 49152-65535

安装增强工具

```
/Machines/VM/Start/Devices/Insert Guest Additions CD images...
```

Network 配置为 Host-only 可被其它虚拟机访问

```
/Machines/VM/Settings/Expert/Network/Adapter 2:
[√] Enable Network Adapter
Attached to: Hoat-only Adapter
```

---

- [Oracle VirtualBox](https://www.virtualbox.org/)
- [Download VirtualBox for Linux Hosts](https://www.virtualbox.org/wiki/Linux_Downloads)
