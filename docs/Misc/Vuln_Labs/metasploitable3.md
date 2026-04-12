Metasploitable3 is a VM that is built from the ground up with a large amount of security vulnerabilities.

## 1. Step

- [VirtualBox](https://www.virtualbox.org/)
- [Metasploitable3-ub1404.ova](http://cs.smumn.edu/courses/ISO/Metasploitable3-ub1404.ova)
- [metasploitable3-workspace_win2k8_1708656020049_82427.ova](http://cs.smumn.edu/courses/ISO/metasploitable3-workspace_win2k8_1708656020049_82427.ova)

## 2. Init

### 2.1. ubuntu_1404

Import

```
ubuntu_1404_temp
```

![Import](./../../../images/metasploitable3/ubuntu_1404/Import.png)

Host-only

![Host-only](./../../../images/metasploitable3/ubuntu_1404/Host-only.png)

Start

![Start](./../../../images/metasploitable3/ubuntu_1404/Start.png)

Login

```
vagrant:vagrant
```

Power off

```
sudo poweroff
```

Export

```
ubuntu_1404.ova
```

![Export](./../../../images/metasploitable3/ubuntu_1404/Export.png)

Remove

> Delete the virtual machine files and virtual hard disks

![Remove](./../../../images/metasploitable3/ubuntu_1404/Remove.png)

### 2.2. windows_2008_r2

Import

```
windows_2008_r2_temp
```

![Import](./../../../images/metasploitable3/windows_2008_r2/Import.png)

Host-only

![Host-only](./../../../images/metasploitable3/windows_2008_r2/Host-only.png)

Start

![Start](./../../../images/metasploitable3/windows_2008_r2/Start.png)

Insert Ctrl-Alt-Del

![Insert Ctrl-Alt-Del](./../../../images/metasploitable3/windows_2008_r2/Insert%20Ctrl-Alt-Del.png)

Login

```
vagrant:vagrant
```

Ask me later

![Ask me later](./../../../images/metasploitable3/windows_2008_r2/Ask%20me%20later.png)

Restart Now

![Restart Now](./../../../images/metasploitable3/windows_2008_r2/Restart%20Now.png)

Shut down

![Shut down](./../../../images/metasploitable3/windows_2008_r2/Shut%20down.png)

Export

```
windows_2008_r2.ova
```

![Export](./../../../images/metasploitable3/windows_2008_r2/Export.png)

Remove

> Delete the virtual machine files and virtual hard disks

![Remove](./../../../images/metasploitable3/windows_2008_r2/Remove.png)

## 3. Deploy

Import, Take Snapshot: `deploy` 

```
ubuntu_1404.ova
```

```
windows_2008_r2.ova
```

## 4. Usage

Login `vagrant:vagrant` 

---

References

- [metasploitable3](https://github.com/rapid7/metasploitable3)
- [SMUMN Computer Science Department](http://cs.smumn.edu/)

