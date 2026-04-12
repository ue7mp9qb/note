Windows PE (WinPE) is a small operating system used to install, deploy, and repair Windows desktop editions, Windows Server, and other Windows operating systems.

## 1. Download

下载对应的 ADK 和 WinPE 插件

- [Windows ADK for Windows 10, version 2004](https://go.microsoft.com/fwlink/?linkid=2120254)

- [Windows PE add-on for the ADK, version 2004](https://go.microsoft.com/fwlink/?linkid=2120253)

## 2. Install

安装 ADK 到默认路径

```
C:\Program Files (x86)\Windows Kits\10\
```

![安装 ADK 到默认路径](./../../../../../images/WinPE/%E5%AE%89%E8%A3%85%20ADK%20%E5%88%B0%E9%BB%98%E8%AE%A4%E8%B7%AF%E5%BE%84.png)

选择默认安装

安装 WinPE 插件到默认路径

![安装 WinPE 插件到默认路径](./../../../../../images/WinPE/%E5%AE%89%E8%A3%85%20WinPE%20%E6%8F%92%E4%BB%B6%E5%88%B0%E9%BB%98%E8%AE%A4%E8%B7%AF%E5%BE%84.png)

选择默认安装

## 3. Init

以管理员身份运行部署和映像工具环境

创建 WinPE 文件的工作副本

```
C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Deployment Tools>copype amd64 "C:\WinPE_amd64"
```

制作 WinPE 镜像

```
C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Deployment Tools>MakeWinPEMedia /iso "C:\WinPE_amd64" "C:\WinPE.iso"
```

## 4. Usage

常用命令

```
X:\windows\system32>wpeutil shutdown # 关机
X:\windows\system32>wpeutil reboot   # 重启
```

```
PS X:\windows\system32> exit             # 退出 PowerShell
PS X:\windows\system32> stop-computer    # 关机
PS X:\windows\system32> restart-computer # 重启
```

---

References

- [Windows PE (WinPE)](https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/winpe-intro?view=windows-11)
- [Download and install the Windows ADK](https://learn.microsoft.com/en-us/windows-hardware/get-started/adk-install#other-adk-downloads)
- [Create bootable Windows PE media](https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/winpe-create-usb-bootable-drive?view=windows-11)
- [WinPE: Mount and Customize](https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/winpe-mount-and-customize?view=windows-11)
