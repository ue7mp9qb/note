数字证书管理工具。

## 1 初始化

创建目录用于存储 TLS 密钥和证书

```shell
root@server:~# mkdir /etc/ssl/caddy
```

生成 TLS 密钥

```shell
root@server:~# openssl ecparam -genkey -name secp384r1 -out /etc/ssl/caddy/ssl-test.pem
```

修改密钥的权限

```shell
root@server:~# chmod 600 /etc/ssl/caddy/ssl-test.pem
```

生成 TLS 证书

```shell
root@server:~# openssl req -new -x509 -key /etc/ssl/caddy/ssl-test.pem -out /etc/ssl/caddy/ssl-test.crt -days 365000 -subj "/CN=[server_ip]" -addext "subjectAltName = IP:[server_ip]"
```

修改证书的权限

```shell
root@server:~# chmod 644 /etc/ssl/caddy/ssl-test.crt
```

---

References

- [OpenSSL](https://github.com/openssl/openssl)
