Fuzz 时常用的请求格式

```
GET /test?id=1 HTTP/2
Host: example.com
```

```
POST /test HTTP/2
Host: example.com
Content-Type: application/x-www-form-urlencoded

id=1
```

```
POST /test HTTP/2
Host: example.com
Content-Type: application/json

{"id":"1"}
```

```
POST /test HTTP/2
Host: example.com
Content-Type: application/x-www-form-urlencoded

<id>1</id>
```

