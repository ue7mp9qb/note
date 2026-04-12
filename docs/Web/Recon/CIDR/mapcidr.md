Utility program to perform multiple operations for a given subnet/CIDR ranges.

## 1. Install

安装

```
┌──(sec@debian)-[~]
└─$ go install -v github.com/projectdiscovery/mapcidr/cmd/mapcidr@latest
```

## 2. Usage

C 段扫描

```
┌──(sec@debian)-[~]
└─$ mapcidr -cidr 173.0.84.0/24
```

---

References

- [mapcidr](https://github.com/projectdiscovery/mapcidr)