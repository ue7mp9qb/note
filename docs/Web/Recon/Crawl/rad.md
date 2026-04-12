用于 Chromium 的人机模拟爬虫.

## 1 安装

下载

```
┌──(root@debian)-[~]
└─# curl -L https://github.com/chaitin/rad/releases/download/1.0/rad_windows_amd64.zip
```

解压

```
┌──(root@debian)-[~]
└─# unzip ./rad_linux_amd64.zip -d /root/tools/rad \
&& rm -rf ./rad_linux_amd64.zip
```

编写脚本

```shell
┌──(root@debian)-[~]
└─# chmod +x /root/tools/rad/rad_linux_amd64 && vim /root/tools/rad/rad.sh
```

```sh
#!/bin/bash

# 错误检测
set -e

# 获取链接的实际路径并切换到该目录
cd "$(dirname "$(readlink -f "$0")")"

# 运行 rad
./rad_linux_amd64 "$@"
```

创建链接

```shell
┌──(root@debian)-[~]
└─# chmod +x /root/tools/rad/rad.sh && ln -sf /root/tools/rad/rad.sh /usr/local/bin/rad
```

> windows 中添加至环境变量（windws 中使用需要关闭 Microsoft Defender）
>
> ```
> D:\software\rad
> ```

## 2 初始化

首次运行后修改配置文件

```
C:\Users\sec\rad_config.yml
```

```
6	enable_image: false # 启用图片显示
6	enable_image: true  # 启用图片显示
```

## 3 使用

查看帮助

```shell
┌──(root@debian)-[~]
└─# rad -h
```

```
NAME:
   Rad - A powerful browser crawler [https://xray.cool]

USAGE:
    [global options] command [command options] [arguments...]

COMMANDS:
   help, h  显示命令列表或一条命令的帮助

GLOBAL OPTIONS:
   --target value, -t value, --url value, -u value  将被抓取的目标
   --url-file value, --uf value                     将被抓取的 url 文件
   --config value, -c value                         设置配置文件路径
   --text-output value, --text value                将 urls 输出到指定的文本文件
   --full-text-output value, --full value           将完整的请求转储输出到指定的文本文件
   --json-output value, --json value                将完整的请求转储输出到指定的 json 文件，正文使用 base64 编码
   --auto-index, --index                            如果文件存在，序列号会自动添加到文件名中
   --http-proxy value                               设置上游 HTTP 代理
   --disable-headless, --dh                         禁用浏览器无头模式
   --log-level value                                日志级别，可选择调试、信息、警告、错误、致命
   --no-banner                                      禁用横幅打印
   --help, -h                                       显示帮助
target can not be empty. 请输入需要爬取的目标
```

基本使用

```shell
┌──(root@debian)-[~]
└─# rad -t [url]
```

手动登录

```shell
┌──(root@debian)-[~]
└─# rad -t [url] -wait-login
```

---

References

- [Rad](https://github.com/chaitin/rad)
