 Burp Suite is a comprehensive suite of tools for web application security testing.

## 1. Init

选择临时项目

![选择临时项目](./../../../image/Burp%20Suite/%E9%80%89%E6%8B%A9%E4%B8%B4%E6%97%B6%E9%A1%B9%E7%9B%AE.png)

使用默认配置启动

![使用默认配置启动](./../../../image/Burp%20Suite/%E4%BD%BF%E7%94%A8%E9%BB%98%E8%AE%A4%E9%85%8D%E7%BD%AE%E5%90%AF%E5%8A%A8.png)

浏览器配置 Burp Suite 代理打开以下链接，下载 `cacert.der` 

> http://burpsuite/

![浏览器配置 Burp Suite 代理打开以下链接，下载 cacert.der](./../../../image/Burp%20Suite/%E6%B5%8F%E8%A7%88%E5%99%A8%E9%85%8D%E7%BD%AE%20Burp%20Suite%20%E4%BB%A3%E7%90%86%E6%89%93%E5%BC%80%E4%BB%A5%E4%B8%8B%E9%93%BE%E6%8E%A5%EF%BC%8C%E4%B8%8B%E8%BD%BD%20cacert.der.png)

导入至 Trusted Root Certification Authorities

下载 [jython-standalone.jar](https://central.sonatype.com/artifact/org.python/jython-standalone/versions) 到 `Burp Suite Extensions` 文件夹

```
%UserProfile%\Documents\Github\configs\Burp Suite Extensions
```

> 所有的扩展都要下载到这个目录中

启动 Burp Suite, 配置 Python 环境

![启动 Burp Suite, 配置 Python 环境](./../../../image/Burp%20Suite/%E5%90%AF%E5%8A%A8%20Burp%20Suite,%20%E9%85%8D%E7%BD%AE%20Python%20%E7%8E%AF%E5%A2%83.png)

## 2. Usage

更改外观字体大小

![更改外观字体大小](./../../../image/Burp%20Suite/%E6%9B%B4%E6%94%B9%E5%A4%96%E8%A7%82%E5%AD%97%E4%BD%93%E5%A4%A7%E5%B0%8F.png)

修改 HTTP 消息显示字体

![修改 HTTP 消息显示字体](./../../../image/Burp%20Suite/%E4%BF%AE%E6%94%B9%20HTTP%20%E6%B6%88%E6%81%AF%E6%98%BE%E7%A4%BA%E5%AD%97%E4%BD%93.png)

> 英文推荐 Consolas, 中文推荐 SimHei

### 2.1. Proxy

Burp Suite 的抓包代理仅支持 HTTP 协议

### 2.1.1. Proxy listeners

在 Proxy listeners 中将本地回环地址修改为私有 IP 地址, 在局域网设备配置代理并安装证书即可侦听来自局域网设备的数据

### 2.2. Intruder

Intruder 中有四种 Attack type

```
Sniper        # 一个字典，逐个测试
Battering ram # 一个字典，同步测试
Pitchfork     # 多个字典, 并行测试
Cluster bomb  # 多个字典, 穷举测试
```

### 2.2.1. Grep - Match

用于统计当前页面指定字段出现的次数

使用 Grep - Match 匹配中文时需要使用 Regex 匹配编码

```python
print("登录失败".encode("utf-8"))
```

### 2.2.2. Grep - Extract

用于从当前页面提取数据, 并可在 Payload type 中选择 Recursive grep 使用, 记得填写 Initial payload for first request

Refences

- [Username enumeration via subtly different responses](https://portswigger.net/web-security/authentication/password-based/lab-username-enumeration-via-subtly-different-responses)

## 2.3. Repeater

Repeater 中有三种 Group send options

```
Send group in sequence (single connection)    # 单线程逐个发送
Send group in sequence (separate connections) # 多线程逐个发送
Send group in parallel (single-packet attack) # 多线程同步发送
```

### 2.4. Sessions

### 2.4.1. Macros

用于从其它页面提取数据, 配置到 Sessions 后即可在所有会话中使用, 注意不要添加测试页面

References

- [2FA bypass using a brute-force attack](https://portswigger.net/web-security/authentication/multi-factor/lab-2fa-bypass-using-a-brute-force-attack)

---

**References**

- [Burp Suite](https://portswigger.net/burp/pro)
- [Burp Suite documentation](https://portswigger.net/burp/documentation)

