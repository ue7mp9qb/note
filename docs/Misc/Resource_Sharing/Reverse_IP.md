查询 PTR 记录

```
┌──(nemo@debian)-[~]
└─$ dig -x 1.1.1.1 +short
```

```
┌──(nemo@debian)-[~]
└─$ echo 1.1.1.1 | dnsx -ptr -ro
```

Certificate 反查

```
Common Name (CN)
Subject Alternative Name (SAN)
```

![](./../../../images/Reverse_IP/Certificate%20%E5%8F%8D%E6%9F%A5.png)

HUNTER

```
ip=="1.1.1.1"
```

FOFA

```
ip=="1.1.1.1"
```

QUAKE

```
ip:"1.1.1.1"
```

ZoomEye

```
ip=="1.1.1.1"
```
