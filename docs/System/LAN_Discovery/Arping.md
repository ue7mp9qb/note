向主机发送 ARP 和/或 ICMP 请求，获取回复信息。

发送 1 次 ARP 请求，检测指定 IP 的 MAC 地址

```shell
┌──(root㉿kali)-[~]
└─# arping -c 1 [ip]
```

---

References

- [Arping](https://www.kali.org/tools/arping/)