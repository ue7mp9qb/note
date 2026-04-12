一款 Windows 平台的安卓模拟器.

## 1. 初始化

配置设置中心

![配置设置中心](./../../../images/MuMuPlayer/%E9%85%8D%E7%BD%AE%E8%AE%BE%E7%BD%AE%E4%B8%AD%E5%BF%83.png)

安装 [酷安](https://www.coolapk.com/)

### 1.1. 安装 Burp 证书

安装 Amaze

![安装 Amaze](./../../../images/MuMuPlayer/%E5%AE%89%E8%A3%85%20Amaze.png)

授予权限

![授予权限](./../../../images/MuMuPlayer/%E6%8E%88%E4%BA%88%E6%9D%83%E9%99%90.png)

授予所有文件的管理权限

![授予所有文件的管理权限](./../../../images/MuMuPlayer/%E6%8E%88%E4%BA%88%E6%89%80%E6%9C%89%E6%96%87%E4%BB%B6%E7%9A%84%E7%AE%A1%E7%90%86%E6%9D%83%E9%99%90.png)

打开根目录浏览权限

![打开根目录浏览权限](./../../../images/MuMuPlayer/%E6%89%93%E5%BC%80%E6%A0%B9%E7%9B%AE%E5%BD%95%E6%B5%8F%E8%A7%88%E6%9D%83%E9%99%90.png)

下载 cacert.der 到 debian

> http://127.0.0.1:8080/

将 cacert.der 转换为 cacert.pem

```
openssl x509 -inform DER -in cacert.der -out cacert.pem
```

获取 hash

```
openssl x509 -inform PEM -subject_hash_old -in cacert.pem
```

重命名为 9a5ba575.0 并复制到 C:\Users\sec\Documents\MuMu共享文件夹\Download

使用 Amaze 将 9a5ba575.0 复制到 cacerts 目录

```
/system/etc/security/cacerts/9a5ba575.0
```

![使用 Amaze 将 9a5ba575.0 复制到 cacerts 目录](./../../../images/MuMuPlayer/%E4%BD%BF%E7%94%A8%20Amaze%20%E5%B0%86%209a5ba575.0%20%E5%A4%8D%E5%88%B6%E5%88%B0%20cacerts%20%E7%9B%AE%E5%BD%95.png)

修改文件权限

![修改文件权限](./../../../images/MuMuPlayer/%E4%BF%AE%E6%94%B9%E6%96%87%E4%BB%B6%E6%9D%83%E9%99%90.png)

关闭浏览器的安全警告

![关闭浏览器的安全警告](./../../../images/MuMuPlayer/%E5%85%B3%E9%97%AD%E6%B5%8F%E8%A7%88%E5%99%A8%E7%9A%84%E5%AE%89%E5%85%A8%E8%AD%A6%E5%91%8A.png)

添加 burp 代理

![添加 burp 代理](./../../../images/MuMuPlayer/%E6%B7%BB%E5%8A%A0%20burp%20%E4%BB%A3%E7%90%86.png)

## 2. 使用

### 2.1. 配置 ADB 调试

将 ADB 加入环境变量

```
C:\Program Files\Netease\MuMu Player 12\shell
```

获取 ADB 调试端口

![获取 ADB 调试端口](./../../../images/MuMuPlayer/%E8%8E%B7%E5%8F%96%20ADB%20%E8%B0%83%E8%AF%95%E7%AB%AF%E5%8F%A3.png)

连接安卓模拟器

```
adb connect 10.0.2.15:16384
```

```
* daemon not running; starting now at tcp:5037
* daemon started successfully
connected to 10.0.2.15:16384
```

列出当前设备

```powershell
adb devices
```

```
List of devices attached
10.0.2.15:16384 device
```

进入设备 shell

```powershell
adb shell
```

```
Welcome! If you need help getting started, check out our developer FAQ page at:

    https://g.126.fm/04jewvw

We're committed to making our emulator as useful as possible for developers,
so if you have any specific requirements or features that you'd like to see
in the emulator, please let us know. We're always open to new ideas and suggestions.
You can find our contact information on the FAQ page as well.

Thanks for using our emulator, happy coding!
22021211RC:/ $
```

切换到管理员权限

```shell
1|22021211RC:/ $ su
```

![切换到管理员权限](./../../../images/MuMuPlayer/%E5%88%87%E6%8D%A2%E5%88%B0%E7%AE%A1%E7%90%86%E5%91%98%E6%9D%83%E9%99%90.png)

查看用户名

```shell
:/ # whoami
root
```

获取当前设备上所有活动的信息，找到当前处于活动状态的  APP 包名

```shell
:/ # dumpsys activity activities
```

```
com.tx.zqzs.yofun.mumu
```

### 2.2. 使用 CSNAS 捕获流量

在 mumuplayer 中运行任意联网程序，使用 CSNAS 找到运行环境路径

```
C:\Program Files\MuMuVMMVbox\Hypervisor\MuMuVMMHeadless.exe
```

![在 mumuplayer 中运行任意联网程序，使用 CSNAS 找到运行环境路径](./../../../images/MuMuPlayer/%E5%9C%A8%20mumuplayer%20%E4%B8%AD%E8%BF%90%E8%A1%8C%E4%BB%BB%E6%84%8F%E8%81%94%E7%BD%91%E7%A8%8B%E5%BA%8F%EF%BC%8C%E4%BD%BF%E7%94%A8%20CSNAS%20%E6%89%BE%E5%88%B0%E8%BF%90%E8%A1%8C%E7%8E%AF%E5%A2%83%E8%B7%AF%E5%BE%84.png)

### 2.3. 安装 fiddler 证书

导出证书

![导出证书](./../../../images/MuMuPlayer/%E5%AF%BC%E5%87%BA%E8%AF%81%E4%B9%A6.png)

将 FiddlerRoot.cer 转换为 FiddlerRoot.pem

```shell
┌──(root㉿kali-23)-[~]
└─# openssl x509 -inform der -in FiddlerRoot.cer -out FiddlerRoot.pem
```

获取 hash

```shell
┌──(root㉿kali-23)-[~]
└─# openssl x509 -inform PEM -subject_hash_old -in FiddlerRoot.pem
```

```
269953fb
-----BEGIN CERTIFICATE-----
```

复制到共享文件夹

```shell
┌──(root㉿kali-23)-[~]
└─# share && cp FiddlerRoot.pem /mnt/share/mnt/share/mumuplayer
```

> 重命名为 269953fb.0

使用系统应用文件将 269953fb.0 复制到 Download

![使用系统应用文件将 9a5ba575.0 复制到 Download](./../../../images/MuMuPlayer/%E4%BD%BF%E7%94%A8%E7%B3%BB%E7%BB%9F%E5%BA%94%E7%94%A8%E6%96%87%E4%BB%B6%E5%B0%86%209a5ba575.0%20%E5%A4%8D%E5%88%B6%E5%88%B0%20Download.png)

使用 Amaze 将 269953fb.0 复制到 cacerts 目录

```
/system/etc/security/cacerts/269953fb.0
```

![使用 Amaze 将 9a5ba575.0 复制到 cacerts 目录](./../../../images/MuMuPlayer/%E4%BD%BF%E7%94%A8%20Amaze%20%E5%B0%86%209a5ba575.0%20%E5%A4%8D%E5%88%B6%E5%88%B0%20cacerts%20%E7%9B%AE%E5%BD%95.png)

修改文件权限

![修改文件权限](./../../../images/MuMuPlayer/%E4%BF%AE%E6%94%B9%E6%96%87%E4%BB%B6%E6%9D%83%E9%99%90.png)

关闭浏览器的安全警告

![关闭浏览器的安全警告](./../../../images/MuMuPlayer/%E5%85%B3%E9%97%AD%E6%B5%8F%E8%A7%88%E5%99%A8%E7%9A%84%E5%AE%89%E5%85%A8%E8%AD%A6%E5%91%8A.png)

添加 fiddler 代理

![添加 fiddler 代理](./../../../images/MuMuPlayer/%E6%B7%BB%E5%8A%A0%20fiddler%20%E4%BB%A3%E7%90%86.png)

---

References

- [MuMuPlayer](https://mumu.163.com/)
