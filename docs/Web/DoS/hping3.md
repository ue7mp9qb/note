一款网络工具，能发送自定义的 ICMP/UDP/TCP 数据包。

## 1 使用

查看手册

```shell
┌──(root㉿kali)-[~]
└─# man hping3
```

对目标的指定端口进行压力测试

```shell
┌──(root㉿kali)-[~]
└─# hping3 -c 999999999 -d 128 -S -p [port] --flood --rand-source [ip]
```

> `-c` 数据包个数
>
> `-d` 数据包大小
>
> `-S` 只发送 `syn` 数据包
>
> `-p` 指定端口
>
> `--flood` 不需要回复
>
> `--rand-source` 伪造源 `ip`

---

References

- [hping3](https://www.kali.org/tools/hping3/)