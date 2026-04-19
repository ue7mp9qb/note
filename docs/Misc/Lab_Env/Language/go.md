Build simple, secure, scalable systems with Go.

## 1. Install

安装

```
┌──(root㉿kali)-[~]
└─# apt install -y golang-go
```

添加至环境变量

```
┌──(root㉿kali)-[~]
└─# echo 'export PATH="$HOME/go/bin:$PATH"' >> ~/.zshrc && source ~/.zshrc
```

## 2. Init

配置镜像加速

```
┌──(root@kali)-[~]
└─$ go env -w GOPROXY="https://goproxy.cn,https://goproxy.io,https://proxy.golang.org,direct"
```

## 3. Usage

查看配置

```
┌──(root@kali)-[~]
└─$ go env
```

创建项目 `~/main.go` 

```go
package main

import "fmt"

func main() {
    fmt.Println("Hello, World!")
}

```

---

**References**

- [go](https://go.dev/)

