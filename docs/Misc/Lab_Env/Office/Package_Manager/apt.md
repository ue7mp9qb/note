[Advanced Package Tool (or APT)](https://en.wikipedia.org/wiki/APT_(Package_Manager)), the main command-line [package manager](https://wiki.debian.org/PackageManagement) for Debian and its derivatives.

## 1. Usage

常用命令

```
apt list              # 列出已安装软件
apt update            # 获取更新
apt search <some_app> # 搜索软件
apt show <some_app>   # 显示软件信息
apt -s <command>      # 模拟运行
```

软件安装

```
apt install <some_app>                       # 安装软件
apt install <some_app> -t bookworm-backports # 指定软件源安装
apt reinstall <some_app>              # 重装软件
apt install --only-upgrade <some_app> # 升级软件
```

系统更新

```
apt upgrade      # 保守更新 (存在依赖的软件包不予更新，即使版本兼容，建议用于小版本更新)
apt dist-upgrade # 智能更新 (存在依赖的软件包会在确认其兼容性后更新，建议用于大版本更新)
apt full-upgrade # 完整更新 (完全更新整个系统，忽略兼容性，建议用于大版本更新)

```

软件卸载

```
apt remove <some_app>  # 卸载软件 (保留配置文件)
apt purge <some_app>   # 卸载软件 (不保留配置文件)
```

安装包清理

```
apt clean                # 删除所有的安装包
apt autoclean            # 删除已过期的安装包
apt autoremove           # 自动删除未使用的依赖 (保留配置文件)
apt autoremove --purge   # 自动删除未使用的依赖 (不保留配置文件)
apt autoremove --dry-run # 模拟试删除未使用的依赖
```

---

References

- [apt](https://wiki.debian.org/Apt)

