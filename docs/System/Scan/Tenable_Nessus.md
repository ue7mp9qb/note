 Windows 系统漏洞扫描工具。

## 1 安装

下载

```shell
┌──(root㉿kali-23)-[~]
└─# proxychains4 wget 'https://www.tenable.com/downloads/api/v1/public/pages/nessus/downloads/22331/download?i_agree_to_tenable_license_agreement=true' -O /root/nessus.deb
```

安装

```shell
┌──(root㉿kali-23)-[~]
└─# sudo gdebi /root/nessus.deb && rm -rf /root/nessus.deb
```

配置别名

```shell
┌──(root㉿kali-23)-[~]
└─# vim ~/.zshrc
```

```
   243  # some more ls aliases
   244  alias ll='ls -l'
   245  alias la='ls -A'
   246  alias l='ls -CF'
   247  alias share='sudo vmhgfs-fuse .host:/ /mnt/hgfs'
   248  alias nessus='sudo systemctl start nessusd.service && firefox https://localhost:8834'
```

使配置生效

```shell
┌──(root㉿kali)-[~]
└─# source ~/.zshrc
```

运行

```shell
┌──(root㉿kali)-[~]
└─# nessus
```

等待一段时间后勾选离线安装

![等待一段时间后勾选离线安装](./../../../images/Tenable_Nessus/%E7%AD%89%E5%BE%85%E4%B8%80%E6%AE%B5%E6%97%B6%E9%97%B4%E5%90%8E%E5%8B%BE%E9%80%89%E7%A6%BB%E7%BA%BF%E5%AE%89%E8%A3%85.png)

选择试用专业版

![选择试用专业版](./../../../images/Tenable_Nessus/%E9%80%89%E6%8B%A9%E8%AF%95%E7%94%A8%E4%B8%93%E4%B8%9A%E7%89%88.png)

复制设备识别码

![](./../../../images/Tenable_Nessus/%E5%A4%8D%E5%88%B6%E8%AE%BE%E5%A4%87%E8%AF%86%E5%88%AB%E7%A0%81.png)

访问申请界面

![](./../../../images/Tenable_Nessus/%E8%AE%BF%E9%97%AE%E7%94%B3%E8%AF%B7%E7%95%8C%E9%9D%A2.png)

输入设备识别码

![](./../../../images/Tenable_Nessus/%E8%BE%93%E5%85%A5%E8%AE%BE%E5%A4%87%E8%AF%86%E5%88%AB%E7%A0%81.png)

访问激活码申请链接

> https://www.tenable.com/products/nessus/nessus-essentials

![](./../../../images/Tenable_Nessus/%E8%AE%BF%E9%97%AE%E6%BF%80%E6%B4%BB%E7%A0%81%E7%94%B3%E8%AF%B7%E9%93%BE%E6%8E%A5.png)

登录邮箱查看激活码

![登录邮箱查看激活码](./../../../images/Tenable_Nessus/%E7%99%BB%E5%BD%95%E9%82%AE%E7%AE%B1%E6%9F%A5%E7%9C%8B%E6%BF%80%E6%B4%BB%E7%A0%81.png)

提交激活码

![提交激活码](./../../../images/Tenable_Nessus/%E6%8F%90%E4%BA%A4%E6%BF%80%E6%B4%BB%E7%A0%81.png)

下载插件

> https://plugins.nessus.org/v2/offline.php

![下载插件](./../../../images/Tenable_Nessus/%E4%B8%8B%E8%BD%BD%E6%8F%92%E4%BB%B6.png)

复制许可证

![](./../../../images/Tenable_Nessus/%E5%A4%8D%E5%88%B6%E8%AE%B8%E5%8F%AF%E8%AF%81.png)

填写许可证

![填写许可证](./../../../images/Tenable_Nessus/%E5%A1%AB%E5%86%99%E8%AE%B8%E5%8F%AF%E8%AF%81.png)

创建用户

> 用户名：sec
>
> 密码：123456

![](./../../../images/Tenable_Nessus/%E5%88%9B%E5%BB%BA%E7%94%A8%E6%88%B7.png)

查看有效期

![查看有效期](./../../../images/Tenable_Nessus/%E6%9F%A5%E7%9C%8B%E6%9C%89%E6%95%88%E6%9C%9F.png)

安装插件

```shell
┌──(root㉿kali-23)-[~]
└─# sudo /opt/nessus/sbin/nessuscli update /root/下载/all-2.0.tar.gz
```

重启 nessus 服务

```shell
┌──(root㉿kali-23)-[~]
└─# systemctl restart nessusd.service && rm -rf /root/下载/all-2.0.tar.gz
```

重新登录

> https://localhost:8834
>
> 用户名：sec
>
> 密码：123456

> 等待插件编译完成

刷新页面直到出现如下界面

![刷新页面直到出现如下界面](./../../../images/Tenable_Nessus/%E5%88%B7%E6%96%B0%E9%A1%B5%E9%9D%A2%E7%9B%B4%E5%88%B0%E5%87%BA%E7%8E%B0%E5%A6%82%E4%B8%8B%E7%95%8C%E9%9D%A2.png)

> 插件编译完成

## 2 使用

### 2.1 配置扫描 Windows 主机

创建新扫描

![](./../../../images/Tenable_Nessus/%E5%88%9B%E5%BB%BA%E6%96%B0%E6%89%AB%E6%8F%8F.png)

选择规则

![](./../../../images/Tenable_Nessus/%E9%80%89%E6%8B%A9%E8%A7%84%E5%88%99.png)

`这些带升级标志的插件都是需要 NESSUS 升级付费版本才可以使用。`

添加一个高级扫描

![](./../../../images/Tenable_Nessus/%E6%B7%BB%E5%8A%A0%E4%B8%80%E4%B8%AA%E9%AB%98%E7%BA%A7%E6%89%AB%E6%8F%8F.png)

填入配置信息

![](./../../../images/Tenable_Nessus/%E5%A1%AB%E5%85%A5%E9%85%8D%E7%BD%AE%E4%BF%A1%E6%81%AF.png)

`如果写多个 IP，每个 IP 以英文逗号分隔。`

`目标文件，每行一个目标`

开启计划任务

![](./../../../images/Tenable_Nessus/%E5%BC%80%E5%90%AF%E8%AE%A1%E5%88%92%E4%BB%BB%E5%8A%A1.png)

自定义计划

![](./../../../images/Tenable_Nessus/%E8%87%AA%E5%AE%9A%E4%B9%89%E8%AE%A1%E5%88%92.png)

设置邮件通知

![](./../../../images/Tenable_Nessus/%E8%AE%BE%E7%BD%AE%E9%82%AE%E4%BB%B6%E9%80%9A%E7%9F%A5.png)

`需要配置 SMTP 服务器`

配置主机发现

![](./../../../images/Tenable_Nessus/%E9%85%8D%E7%BD%AE%E4%B8%BB%E6%9C%BA%E5%8F%91%E7%8E%B0.png)

可添加凭证

![](./../../../images/Tenable_Nessus/%E5%8F%AF%E6%B7%BB%E5%8A%A0%E5%87%AD%E8%AF%81%20(1).png)

![](./../../../images/Tenable_Nessus/%E5%8F%AF%E6%B7%BB%E5%8A%A0%E5%87%AD%E8%AF%81%20(2).png)

添加插件

![](./../../../images/Tenable_Nessus/%E6%B7%BB%E5%8A%A0%E6%8F%92%E4%BB%B6.png)

`可点击关闭不必要的插件，保存即可。`

点击运行

![](./../../../images/Tenable_Nessus/%E7%82%B9%E5%87%BB%E8%BF%90%E8%A1%8C.png)

点击任务，查看任务进程

![](./../../../images/Tenable_Nessus/%E6%9F%A5%E7%9C%8B%E4%BB%BB%E5%8A%A1%E8%BF%9B%E7%A8%8B.png)

点击任务进程，查看漏洞

![](./../../../images/Tenable_Nessus/%E6%9F%A5%E7%9C%8B%E6%BC%8F%E6%B4%9E.png)

列出以下漏洞

![](./../../../images/Tenable_Nessus/%E5%88%97%E5%87%BA%E4%BB%A5%E4%B8%8B%E6%BC%8F%E6%B4%9E.png)

`是否存在这些漏洞还需要进行验证。`

### 2.2 配置扫描 Web 服务

新建扫描任务

![](./../../../images/Tenable_Nessus/%E6%96%B0%E5%BB%BA%E6%89%AB%E6%8F%8F%E4%BB%BB%E5%8A%A1.png)

到模板找到 Web 应用程序测试

![](./../../../images/Tenable_Nessus/Web%20%E5%BA%94%E7%94%A8%E7%A8%8B%E5%BA%8F%E6%B5%8B%E8%AF%95.png)

填入信息

![](./../../../images/Tenable_Nessus/%E5%A1%AB%E5%85%A5%E4%BF%A1%E6%81%AF.png)

`目标可以直接填写域名，多个域名之间用英文逗号间隔`

选择扫描所有端口

![](./../../../images/Tenable_Nessus/%E9%80%89%E6%8B%A9%E6%89%AB%E6%8F%8F%E6%89%80%E6%9C%89%E7%AB%AF%E5%8F%A3.png)

点击漏洞

![](./../../../images/Tenable_Nessus/%E7%82%B9%E5%87%BB%E6%BC%8F%E6%B4%9E.png)

查看漏洞信息

![](./../../../images/Tenable_Nessus/%E6%9F%A5%E7%9C%8B%E6%BC%8F%E6%B4%9E%E4%BF%A1%E6%81%AF.png)

---

References

- [Tenable Nessus](https://www.tenable.com/)
- [Tenable Nessus Downloads](https://www.tenable.com/downloads/nessus)

