A next-generation crawling and spidering framework.

## 1. Install

安装

```
┌──(root@kali)-[~]
└─$ go install github.com/projectdiscovery/katana/cmd/katana@latest
```

## 2. Usage

爬虫扫描

```
┌──(root@kali)-[~]
└─$ katana -u "http://example.com" -e cdn -iqp -ef gif,jpg,png,ico,css,woff,woff2,ttf,svg -kf all -o ~/results.txt
```

---

References

- [katana](https://github.com/projectdiscovery/katana)
