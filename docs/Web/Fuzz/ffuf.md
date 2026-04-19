Fast web fuzzer written in Go.

## 1. Usage

GET 扫描

```
┌──(root㉿kali)-[~]
└─$ ffuf -recursion-depth 3 \
-w ~/wordlist.txt:FUZZ \
-H "X-Originating-Ip: 127.0.0.1" \
-H "X-Remote-Ip: 127.0.0.1" \
-H "X-Forwarded-For: 127.0.0.1" \
-H "X-Remote-Addr: 127.0.0.1" \
-H "Cf-Connecting-Ip: 127.0.0.1" \
-u http://example.com/FUZZ -fs 0 \
-o ~/ffuf_example.com.json
```

---

**References**

- [ffuf](https://www.kali.org/tools/ffuf/)