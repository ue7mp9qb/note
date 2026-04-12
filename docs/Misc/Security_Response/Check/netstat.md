Displays active TCP connections, ports on which the computer is listening, Ethernet statistics, the IP routing table, IPv4 statistics (for the IP, ICMP, TCP, and UDP protocols), and IPv6 statistics (for the IPv6, ICMPv6, TCP over IPv6, and UDP over IPv6 protocols). Used without parameters, this command displays active TCP connections.

## 1. Usage

筛查可疑连接

```
PS C:\Users\sec> netstat -ano | findstr ESTABLISHED
```

定位可疑进程

```
PS C:\Users\sec> tasklist /FI "PID eq <PID>"
```

---

References

- [netstat](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/netstat)

