Next generation web scanner.

## 1. Usage

识别单个目标使用的 Web 服务

```
┌──(root㉿kali)-[~]
└─$ whatweb -a 3 https://example.com/ --color=never | tee ~/whatweb_example.com.txt
```

识别多个目标使用的 Web 服务

```
┌──(root㉿kali)-[~]
└─$ whatweb -a 3 -i ~/url.txt --color=never | tee ~/whatweb_url.txt
```

---

References

- [whatweb](https://www.kali.org/tools/whatweb/)

