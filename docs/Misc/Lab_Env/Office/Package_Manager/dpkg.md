deb 安装包管理工具.

## 1. 使用

软件安装

```
dpkg -i fileName.deb
```

查找已安装的软件

```
dpkg -l | grep <app-name>
```

软件卸载

```
dpkg -r      <app-name> # 卸载软件 (保留配置文件)
dpkg --purge <app-name> # 卸载软件 (不保留配置文件)
```
