综合靶场.

## 1. Install

克隆仓库到

```
┌──(root㉿kali)-[~]
└─$ git clone --depth 1 https://github.com/vulhub/vulhub.git ~/.local/vulhub
```

## 2. Usage

查看漏洞环境

```
┌──(root㉿kali)-[~]
└─$ ls  ~/.local/vulhub
```

修改端口映射

```
┌──(root㉿kali)-[~]
└─$ cd ~/.local/vulhub/nday && nano ./docker-compose.yml
```

```
services:
 web:
   image: vulhub/nday
   ports:
    - "60081:8080"
```

启动漏洞环境

```
┌──(nemo@debian)-[~/.local/vulhub/nday]
└─$ docker compose build && docker compose up -d
```

Port Forwarding Rules

| Name    | Host Port | Guest Port |
| ------- | --------- | ---------- |
| example | 60081     | 60081      |

删除环境

```
┌──(nemo@debian)-[~/.local/vulhub/nday]
└─# docker compose down -v
```

---

References

- [vulhub](https://vulhub.org/)

