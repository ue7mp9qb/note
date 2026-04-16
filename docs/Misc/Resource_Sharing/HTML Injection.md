向目标网站注入标签并解析, 不同于 XSS 的是无法执行 JS 代码.

> 只可作用于触发 HTML Injection 的网站, 如: 
>
> 在 example.com 上传 `html_injection.svg`, 打开时会调用 test.com 提供的 HTML 解析器并触发 XSS;
>
> 那么此时的 XSS 只作用于 test.com, 不会影响 example.com.

> 需要对目标网站造成危害;
>
> 如: 篡改网页信息, 注入超链接.

Reflected 和 DOM 时常不收, 留意公告

## 1. 原理

在评论区发送一个 HTML 标签

```
<a href=https://evil.com>CLICK</a>
```

会在评论区解析为 <a href=https://evil.com>CLICK</a>, 若用户点击则会跳转到恶意网站

### 1.1. 存在位置

- 输入框

- URL

- 文件上传处

- 由请求包控制的值

  ```
  GET /?search=test HTTP/2
  Host: example.com
  Referer: https://example.com
  ```

  ```
  <a href=https://example.com>CLICK</a>
  ```

  > 响应包中 `value` 的值由请求包中的 `Referer` 的值控制, 那么可能存在 HTML Injection

## 2. 数据提交类

在目标网站提交使用 HTML 构造的数据.

### 2.1 测试步骤

### 2.1.1. 查找注入点

- 测试输入框
- 测试 URL 
- 在 Burp Suite 的 Intruder 模块中标记请求包中所有可能触发 HTML Injection 的位置, 然后使用 Sniper attack 模式注入一个随机字符串, 若在响应包中匹配到这个随机字符串则可能存在 HTML Injection

### 2.1.2 无害化测试

> 在 Burp 中测试 GET 请求时需要对空格进行 URL 编码, 若成功解析则标签是彩色的

使用无危害标签测试是否可以解析, 若可以解析则存在 HTML Injection, 可进一步测试造成危害

```
<s>1
%3Cs%3E1
'"><s>1
%27%22%3E%3Cs%3E1
'"><s>1#//--+
%27%22%3E%3Cs%3E1%23%2F%2F--%2B
'"></xmp></title></style></iframe></script></noembed></noscript></textarea><s>1#//--+
%27%22%3E%3C%2Fxmp%3E%3C%2Ftitle%3E%3C%2Fstyle%3E%3C%2Fiframe%3E%3C%2Fscript%3E%3C%2Fnoembed%3E%3C%2Fnoscript%3E%3C%2Ftextarea%3E%3Cs%3E1%23%2F%2F--%2B
```

某些标签的内容无法解析,需要闭合

```
</xmp>
</title>
</style>
</iframe>
</script>
</noembed>
</noscript>
</textarea>
```

### 2.1.2 标签注入

使用可能对目标造成危害的标签

```
<s>
<h1>hack</h1>
<a href=https://evil.com>CLICK</a>
<img src=https://evil.com/test.svg>
```

## 3. 文件解析类

在文件中嵌入恶意标签后上传, 当目标加载文件时会触发 HTML Injection 攻击.

> 待补充

---

References

- [CWE-80](https://hackerone.com/hacktivity/cwe_discovery?id=cwe-80)
- [HTML Injection in LinkedIn Premium Support Chat](https://hackerone.com/reports/3079966)
- [腾讯某分站插入HTML代码-后台未授权访问-泄露用户信息打包](https://wy.zone.ci/bug_detail.php?wybug_id=wooyun-2015-0156818)