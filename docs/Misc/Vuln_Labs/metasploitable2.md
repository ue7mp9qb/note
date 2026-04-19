A test environment provides a secure place to perform penetration testing and security research.

## 1. Step

- [VMware Workstation Pro](https://support.broadcom.com/group/ecx/productdownloads?subfamily=VMware%20Workstation%20Pro&freeDownloads=true)
- [VirtualBox](https://www.virtualbox.org/)
- [metasploitable-linux-2.0.0.zip](https://sourceforge.net/projects/metasploitable/)

## 2. Init

使用 VMware Workstation Pro 打开虚拟机

![](./../../../images/metasploitable2/%E4%BD%BF%E7%94%A8%20VMware%20Workstation%20Pro%20%E6%89%93%E5%BC%80%E8%99%9A%E6%8B%9F%E6%9C%BA.png)

导出为 OVA

```
ubuntu_0804_temp.ova
```

![](./../../../images/metasploitable2/%E5%AF%BC%E5%87%BA%E4%B8%BA%20OVA.png)

Import

```
ubuntu_0804_temp
```

![](./../../../images/metasploitable2/Import.png)

Host-only

![](./../../../images/metasploitable2/Host-only.png)

Start

![](./../../../images/metasploitable2/Start.png)

Login

```
msfadmin:msfadmin
```

Power off

```
sudo poweroff
```

Export

```
ubuntu_0804.ova
```

![](./../../../images/metasploitable2/Export.png)

Remove

> Delete the virtual machine files and virtual hard disks

![](./../../../images/metasploitable2/Remove.png)

## 3. Deploy

Import, Take Snapshot: `deploy` 

```
ubuntu_0804.ova
```

## 4. Usage

Login `msfadmin:msfadmin` 

---

**References**

- [metasploitable2](https://docs.rapid7.com/metasploit/metasploitable-2/)