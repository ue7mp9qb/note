在 Windows 遇到的一些常见问题。

## 1. 中文程序乱码

打开一个中文程序会显示乱码

![](./../../../../images/Issues_of_Windows/%E6%89%93%E5%BC%80%E4%B8%80%E4%B8%AA%E4%B8%AD%E6%96%87%E7%A8%8B%E5%BA%8F%E4%BC%9A%E6%98%BE%E7%A4%BA%E4%B9%B1%E7%A0%81.png)

在控制面板中将 `系统区域` 设置为 `中文(简体, 中国)` 即可

![](./../../../../images/Issues_of_Windows/%E5%9C%A8%E6%8E%A7%E5%88%B6%E9%9D%A2%E6%9D%BF%E4%B8%AD%E5%B0%86%20%60%E7%B3%BB%E7%BB%9F%E5%8C%BA%E5%9F%9F%60%20%E8%AE%BE%E7%BD%AE%E4%B8%BA%20%60%E4%B8%AD%E6%96%87(%E7%AE%80%E4%BD%93,%20%E4%B8%AD%E5%9B%BD)%60%20%E5%8D%B3%E5%8F%AF.png)

再次打开程序显示正常

![](./../../../../images/Issues_of_Windows/%E5%86%8D%E6%AC%A1%E6%89%93%E5%BC%80%E7%A8%8B%E5%BA%8F%E6%98%BE%E7%A4%BA%E6%AD%A3%E5%B8%B8.png)

## 2. 静态 IP

有时在 Windows 设置中配置静态 IP 无法同步到控制面板, 建议在控制面板校准一次

## 3. 自动唤醒

计算机进入睡眠状态后有时会被自动唤醒, 可通过以下命令查看唤醒历史记录

```powershell
PS C:\Users\sec> powercfg -lastwake
```

```
唤醒历史记录计数 - 1
唤醒历史记录 [0]
  唤醒源计数 - 1
  唤醒源 [0]
    类型: 设备
    实例路径: PCI\VEN_8086&DEV_1A1D&SUBSYS_86721043&REV_11\3&11583659&0&FE
    友好名称: Intel(R) Ethernet Connection (17) I219-V
    描述: Intel(R) Ethernet Connection (17) I219-V
    制造商: Intel
```

禁用指定设备的唤醒功能

```powershell
PS C:\Users\sec> powercfg -devicedisablewake "Intel(R) Ethernet Connection (17) I219-V"
```

查看当前允许唤醒的设备

```powershell
PS C:\Users\sec> powercfg -devicequery wake_armed
```

## 4. 输入字符变宽

在输入字符时, 有时会出现字节变宽的现象

```
This is a test text.            # 正常字符
Ｔｈｉｓ　ｉｓ　ａ　ｔｅｓｔ　ｔｅｘｔ．# 宽字符
```

原因是开启了全角输入, 切换为半角输入即可恢复正常

![](./../../../../images/Issues_of_Windows/%E5%8E%9F%E5%9B%A0%E6%98%AF%E5%BC%80%E5%90%AF%E4%BA%86%E5%85%A8%E8%A7%92%E8%BE%93%E5%85%A5,%20%E5%88%87%E6%8D%A2%E4%B8%BA%E5%8D%8A%E8%A7%92%E8%BE%93%E5%85%A5%E5%8D%B3%E5%8F%AF%E6%81%A2%E5%A4%8D%E6%AD%A3%E5%B8%B8.png)

为避免再次误触还应该关闭输入法设置的 `全半角切换` 快捷键

![](./../../../../images/Issues_of_Windows/%E4%B8%BA%E9%81%BF%E5%85%8D%E5%86%8D%E6%AC%A1%E8%AF%AF%E8%A7%A6%E8%BF%98%E5%BA%94%E8%AF%A5%E5%85%B3%E9%97%AD%E8%BE%93%E5%85%A5%E6%B3%95%E8%AE%BE%E7%BD%AE%E7%9A%84%20%60%E5%85%A8%E5%8D%8A%E8%A7%92%E5%88%87%E6%8D%A2%60%20%E5%BF%AB%E6%8D%B7%E9%94%AE.png)

## 5. Windows11 跳过联网

1. 断掉网络设备
2. 在安装界面按 Shift+F10 调出 CMD
3. 执行 `oobe\bypassnro`
4. 重新进入安装界面后可跳过联网

## 6. 任务栏安全中心图标消失

创建快捷方式

```
C:\Windows\System32\SecurityHealthSystray.exe
```

到以下目录

```
C:\Users\nemo\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup
```

