插入 U 盘，运行 WePE

选择安装 PE 到 U 盘

安装方法

```
方案一：UEFI/Legacy全能三分区方式(推荐)
```

待写入 U 盘

```
可移动磁盘
```

格式化

```
exFAT USB-HDD
```

U 盘卷标

```
可移动磁盘
```

立即安装 PE 到 U 盘

开始制作 

完成安装 

将系统运维工具移动到 PE 的 U 盘分区

```
D:\local\系统运维
```

重启系统进入引导界面

```
需要关闭 Bitlocker
ASUS 启动引导界面按 Del 
TinkPad 启动引导界面按 F12 
```

选择 U 盘启动进入 WePE

进入 `Windows.iso` ，运行 `setup.exe` ，进行安装

---

References

- [WePE](https://www.wepe.com.cn/)