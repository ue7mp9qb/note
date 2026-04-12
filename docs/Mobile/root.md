## 1. MIUI

### 1.1. 绑定账号和设备

连续点击多次版本号, 进入开发者模式

开启 `OEM 解锁` 

在 `设备解锁状态` 中绑定账号和设备

### 1.2. 解锁 bootloader

下载 [miflash_unlock](https://www.miui.com/unlock/index.html) 

> 需要将浏览器设置为中文访问

将手机和电脑通过 USB 连接

在手机开机状态下使用 miflash_unlock 检测并安装驱动后重新连接手机

将手机关机，同时按住开机键和音量下键进入 fastboot 模式

最后使用 miflash_unlock 解锁

### 1.3. 修补 boot

从 [XiaomiROM](https://xiaomirom.com/series/) 中下载相应的 fastboot 线刷包, 解压后将 `boot.img` 传入手机

使用 [Magisk](https://github.com/topjohnwu/Magisk) 将修补 `boot.img` 后的 ` magisk.img` 传入电脑

### 1.4. 获取 root 权限

将手机和电脑通过 USB 连接

将手机关机，同时按住开机键和音量下键进入 fastboot 模式

打开终端切换到 [SDK Platform Tools](https://developer.android.com/tools/releases/platform-tools) 目录文件中

识别设备

```
PS C:\Users\sec\platform-tools> .\fastboot devices
b65b7917         fastboot
```

> 若无法识别设备则换一个 USB 接口

将 `magisk.img` 写入 boot 分区

```
PS C:\Users\sec\platform-tools> .\fastboot flash boot .\magisk.img
```

将手机重启后运行 shell

```
PS C:\Users\sec\platform-tools> .\adb shell
```

切换为 root 用户

```
vangogh:/ $ su
```

此时手机会弹出一个授权窗口, 授权后用户身份标识会变为 `#` 

```
vangogh:/ #
```

### 1.5. 隐藏 root

隐藏 Magisk 应用

开启 Zygisk 后重启手机

安装 [Shamiko](https://github.com/LSPosed/LSPosed.github.io/releases) 模块后重启手机

配置排除列表并禁用遵守排除列表

---

References

- [root 基础指南](https://www.bilibili.com/video/BV1BY4y1H7Mc/?spm_id_from=333.1387.favlist.content.click&vd_source=2dcc7806c9580af60063ca1edb63852d)

