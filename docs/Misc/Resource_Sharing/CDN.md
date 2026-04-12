查询 A 记录, 若存在多个 IP, 则可能存在 CDN

```
┌──(nemo@debian)-[~]
└─$ echo example.com | dnsx -a -ro
```

确认 IP 是否使用了 CDN

```
┌──(nemo@debian)-[~]
└─$ cdncheck -i 1.1.1.1 -cdn
```

若无法确认, 则通过查询 WHOIS 判断

```
┌──(nemo@debian)-[~]
└─$ whois 1.1.1.1
```

使用国外的 VPS 探测

```
┌──(nemo@debian)-[~]
└─$ ping 1.1.1.1
```

若目标网站可使用邮箱注册, 可从邮件中得到源 IP

```
Received: from out203-205-221-235.mail.example.com (out203-205-221-235.mail.example.com. [1.1.1.1])
```

![](./../../../images/CDN/%E8%8B%A5%E7%9B%AE%E6%A0%87%E7%BD%91%E7%AB%99%E5%8F%AF%E4%BD%BF%E7%94%A8%E9%82%AE%E7%AE%B1%E6%B3%A8%E5%86%8C,%20%E5%8F%AF%E4%BB%8E%E9%82%AE%E4%BB%B6%E4%B8%AD%E5%BE%97%E5%88%B0%E6%BA%90%20IP.png)

---

References

- [绕过cdn查找网站源IP的方法](https://www.bilibili.com/video/BV1tX4y1n7Ha/?spm_id_from=333.1387.favlist.content.click&vd_source=2dcc7806c9580af60063ca1edb63852d)
