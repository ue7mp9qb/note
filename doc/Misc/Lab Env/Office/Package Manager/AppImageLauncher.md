Helper application for Linux distributions serving as a kind of "entry point" for running and integrating AppImages

## 1. Install

安装

```
┌──(nemo@debian)-[~]
└─$ cd /tmp \
&& curl -LO https://github.com/TheAssassin/AppImageLauncher/releases/download/v2.2.0/appimagelauncher_2.2.0-travis995.0f91801.bionic_amd64.deb \
&& sudo apt install -y ./appimagelauncher*.deb \
&& rm ./appimagelauncher*.deb
```

##  2. Init

创建目录文件

```
┌──(nemo@debian)-[~]
└─$ mkdir -p ~/.local/Applications
```

修改默认存储位置

```
/home/nemo/.local/Applications
```

![修改默认存储位置](./../../../../../image/AppImageLauncher/%E4%BF%AE%E6%94%B9%E9%BB%98%E8%AE%A4%E5%AD%98%E5%82%A8%E4%BD%8D%E7%BD%AE.png)


## 3. Usage

双击运行 app.AppImage 将应用集成到系统菜单中

![双击运行 app.AppImage 将应用集成到系统菜单中](./../../../../../image/AppImageLauncher/%E5%8F%8C%E5%87%BB%E8%BF%90%E8%A1%8C%20app.AppImage%20%E5%B0%86%E5%BA%94%E7%94%A8%E9%9B%86%E6%88%90%E5%88%B0%E7%B3%BB%E7%BB%9F%E8%8F%9C%E5%8D%95%E4%B8%AD.png)

---

**References**

- [AppImageLauncher](https://github.com/TheAssassin/AppImageLauncher)
