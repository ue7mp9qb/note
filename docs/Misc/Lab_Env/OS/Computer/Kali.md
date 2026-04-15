The most advanced Penetration Testing Distribution. Ever.

## 1. Step

- [VirtualBox](https://www.virtualbox.org/)
- [Kali Linux](https://www.kali.org/)

## 2. Config

Move the images file to the directory

```
%UserProfile%\VirtualBox VMs\ISOs
```

Create a new virtual machine

```
/Home/New
```

```
VM Name: kali
VM Folder: C:\Users\null\VirtualBox VMs
ISO lmage: C:\Users\null\VirtualBox VMs\ISOs\kali-linux-installer-amd64.iso
OS Distribution: Debin
OS Version: Debian (64-bit)
Base Memory: 8192 MB
Number of CPUs: 2 CPU
Disk Size: 64.00GB
```

Take Snapshot: `config` 

## 3. Install

Kali Linux installer menu

```
Install
```

Select a language

```
Language:
English - English
```

Select your location

```
Country, territory or area:
United States
```

Configure the keyboard

```
Keymap to use:
American English
```

Configure the network

```
Hostname:
kali
```

```
Domain name:
```

Set up users and passwords

```
Full name for the new user:
null
```

```
Username for your account:
null
```

```
Choose a password for the new user:
123456
```

```
Re-enter password to verify:
123456
```

Configure the clock

```
Select your time zone:
Eastern
```

Partition disks

```
Partitioning method:
Guided - use entire disk
```

```
Select disk to partition:
VBOX HARDDISK
```

```
Partitioning scheme:
All files in one partition
```

```
Finish partitioning and write changes to disk
```

```
Write the changes to disks?
<Yes>
```

Software selection

```
Choose software to install:
[*] Desktop environment
[*] ... Xfce
[*] Collention of tools
[*] ... top 10 -- the 10 most popular tools
[*] ... default -- recommended tools
```

Configuring grub-pc

```
Install the GRUB boot loader to your primary drive?
<Yes>
```

```
Device for boot loader installation:
/dev/sda
```

Finish the installation

```
Please choose <Continue> to reboot.
<Continue>
```

Login

```
username: null
password: 123456
```

Set root password

```
┌──(null㉿kali)-[~]
└─$ sudo passwd root
```

Take Snapshot: `install` 

```
┌──(null㉿kali)-[~]
└─$ sudo poweroff
```

## 4. Init

Login

```
username: root
password: 123456
```

Allow root user to login with key pairs or password

```
┌──(root㉿kali)-[~]
└─# nano -l /etc/ssh/sshd_config
```

```
 33 #PermitRootLogin prohibit-password
 33 PermitRootLogin yes
```

Enable the SSH service

```
┌──(root㉿kali)-[~]
└─# systemctl enable --now ssh.service
```

Port Forwarding

```
/Machines/VM/Settings/Expert/Network/Adapter 1/Port Forwarding
```

| Name | Host Port | Guest Port |
| ---- | --------- | ---------- |
| SSH  | 60022     | 22         |

SSH to `root` 

```
PS C:\Users\null> ssh -p 60022 root@127.0.0.1
```

Configuring Apt Sources

```
┌──(root㉿kali)-[~]
└─# sed -i "s@http://http.kali.org/kali@https://mirrors.ustc.edu.cn/kali@g" /etc/apt/sources.list
```

Keep SSH session alive

```
┌──(root㉿kali)-[~]
└─# sed -i 's/#ClientAliveInterval 0/ClientAliveInterval 60/' /etc/ssh/sshd_config \
&& sed -i 's/#ClientAliveCountMax 3/ClientAliveCountMax 60/' /etc/ssh/sshd_config \
&& systemctl reload ssh.service
```

Update the system

```
┌──(root㉿kali)-[~]
└─# apt update && apt full-upgrade \
&& apt clean && apt autoremove --purge \
&& apt install -y virtualbox-guest-x11
```

Create directory

```
┌──(root㉿kali)-[~]
└─# mkdir -p ~/.local/bin
```

Take Snapshot: `init` 

```
┌──(root㉿kali)-[~]
└─# poweroff
```

## 5. Deploy

|                            Tools                             |
| :----------------------------------------------------------: |
| [proxychains-ng](https://www.kali.org/tools/proxychains-ng/) |
| [proxy](https://raw.githubusercontent.com/ue7mp9qb/scripts/refs/heads/main/proxy_install.sh) |
|                 [git](https://git-scm.com/)                  |
|              [python](https://www.python.org/)               |
|                    [go](https://go.dev/)                     |
|              [docker](https://www.docker.com/)               |
|       [vulfocus](https://fofapro.github.io/vulfocus/)        |
|          [naabu](https://www.kali.org/tools/naabu/)          |
|  [httpx-toolkit](https://www.kali.org/tools/httpx-toolkit/)  |
|     [katana](https://github.com/projectdiscovery/katana)     |
|       [xsstrike](https://www.kali.org/tools/xsstrike/)       |

Take Snapshot: `deploy` 

```
┌──(root㉿kali)-[~]
└─# poweroff
```

---

References

- [Kali](https://www.kali.org/)
- [Kali Docs](https://www.kali.org/docs/)

