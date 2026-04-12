安装

```
┌──(sec@debian)-[~]
└─$ sudo apt install -y tcpdump
```

抓取 ssh 远程登录Kali时，产生的 tcp 三次握手包

```
┌──(root㉿Kali)-[~]
└─# tcpdump -n -c 3 port 22 -i eth0
tcpdump: verbose output suppressed, use -v[v]... for full protocol decode
listening on eth0, link-type EN10MB (Ethernet), snapshot length 262144 bytes
```

`监听状态，无数据`

打开另一个终端，使用ssh连接Kali

```
[root@CentOS ~]# ssh 192.168.1.9
root@192.168.1.9's password: 
```

请求连接，产生数据

```
┌──(root㉿Kali)-[~]
└─# tcpdump -n -c 3 port 22 -i eth0
tcpdump: verbose output suppressed, use -v[v]... for full protocol decode
listening on eth0, link-type EN10MB (Ethernet), snapshot length 262144 bytes
18:21:20.063388 IP 192.168.1.7.45794 > 192.168.1.9.22: Flags [S], seq 976547325, win 29200, options [mss 1460,sackOK,TS val 5934231 ecr 0,nop,wscale 7], length 0
18:21:20.063436 IP 192.168.1.9 > 192.168.1.7.45794: Flags [S.], seq 1730531831, ack 976547326, win 65160, options [mss 1460,sackOK,TS val 4196746332 ecr 5934231,nop,wscale 7], length 0
18:21:20.063531 IP 192.168.1.7.45794 > 192.168.1.9: Flags [.], ack 1, win 229, options [nop,nop,TS val 5934232 ecr 4196746332], length 0
3 packets captured
5 packets received by filter
0 packets dropped by kernel
```

`client 主机返回 ACK，包序号为 ack=1 ，这是相对序号，如果需要看绝对序号，可以在 tcpdump命令中加-S`

SYN

`在连接建立时用来同步序号。当 SYN=1 而 ACK=0 时，表明这是一个连接请求报文。对方若同意建立连接，则应在响应报文中使 SYN=1 和 ACK=1. 因此, SYN 置 1 就表示这是一个连接请求或连接接受报文。`

FIN

`即完，终结的意思， 用来释放一个连接。当 FIN = 1 时，表明此报文段的发送方的`
`数据已经发送完毕，并要求释放连接。`



三次握手的核心是确认每一次包的序列号

tcp 三次握手过程：
`1、首先由 Client 发出连接请求即 SYN=1，声明自己的序号是 seq=x`
`2、然后 Server 进行回复确认，即 SYN=1 ，声明自己的序号是 seq=y, 并设置为 ack=x+1`
`3、最后 Client 再进行一次确认，设置 ack=y+1`



端口即服务，服务即端口。

例如开启80端口表明打开了apache服务，同样打开了apache服务表明80端口开启



TCP 连接状态详解：
服务器端：LISTEN：侦听来自远方的 TCP 端口的连接请求
客户端：SYN-SENT：在发送连接请求后等待匹配的连接请求
服务器端：SYN-RECEIVED：在收到和发送一个连接请求后等待对方对连接请求的确认
客户端/服务器端：ESTABLISHED：代表一个打开的连接

在物理机查看打开的连接

```
C:\Users\Windows 10>netstat -ano | findstr "ESTABLISHED"
  TCP    127.0.0.1:65170        127.0.0.1:65171        ESTABLISHED     4016
  TCP    192.168.1.9:49694      36.110.273.8:80        ESTABLISHED     3700
  TCP    [::1]:50172            [::1]:50173            ESTABLISHED     10840
  TCP    [::1]:50173            [::1]:50172            ESTABLISHED     10840
  TCP    [2409:8a3c:100b:5cf0:708e:e220:7c44:ca92]:53485  [240e:ff:f100:8019::89]:443  ESTABLISHED     6228
  TCP    [2409:8a3c:100b:5cf0:708e:e220:7c44:ca92]:53486  [240e:97c:2f:3001::f]:443  ESTABLISHED     6228
  TCP    [2409:8a3c:100b:5cf0:708e:e220:7c44:ca92]:53488  [2402:4e00:1020:1768:0:979b:7413:b564]:443  ESTABLISHED     6228
  TCP    [2409:8a3c:100b:5cf0:708e:e220:7c44:ca92]:53561  [240e:ff:f100:13::5b]:443  ESTABLISHED     6228
  TCP    [2409:8a3c:100b:5cf0:708e:e220:7c44:ca92]:53570  [2409:8c28:c05:3:0:4:0:4]:443  ESTABLISHED     12276
  TCP    [2409:8a3c:100b:5cf0:708e:e220:7c44:ca92]:53571  [2409:8c3c:901:1:3::3fc]:443  ESTABLISHED     12276
```

127.0.0.1为本地不需要排查，通过末尾的PID在任务管理器中查找

找到后打开文件所在位置，陌生文件可上传到沙盒检测。

---

References

- [tcpdump](https://www.kali.org/tools/tcpdump/)

