A high performance go implementation of Wappalyzer Technology Detection Library

## 1. Install

安装

```
┌──(nemo@debian)-[~]
└─$ go install -v github.com/projectdiscovery/wappalyzergo/cmd/update-fingerprints@latest
```

## 2. Usage

初始化模块

```
┌──(nemo@debian)-[~]
└─$ go mod init wappalyzer
```

安装依赖

```
┌──(nemo@debian)-[~]
└─$ go mod tidy
```

编写扫描脚本

```
┌──(nemo@debian)-[~]
└─$ nano wappalyzer.go
```

```go
package main

import (
	"fmt"
	"io"
	"log"
	"net/http"

	wappalyzer "github.com/projectdiscovery/wappalyzergo"
)

func main() {
	resp, err := http.DefaultClient.Get("https://www.example.com")
	if err != nil {
		log.Fatal(err)
	}
	data, _ := io.ReadAll(resp.Body) // Ignoring error for example

	wappalyzerClient, err := wappalyzer.New()
	fingerprints := wappalyzerClient.Fingerprint(resp.Header, data)
	fmt.Printf("%v\n", fingerprints)

	// Output: map[Acquia Cloud Platform:{} Amazon EC2:{} Apache:{} Cloudflare:{} Drupal:{} PHP:{} Percona:{} React:{} Varnish:{}]
}
```

运行

```
┌──(nemo@debian)-[~]
└─$ go run wappalyzer.go
```

---

References

- [wappalyzergo](https://github.com/projectdiscovery/wappalyzergo)