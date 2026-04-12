XSS Fuzz。

## 1 安装

安装

```
┌──(root@debian)-[~]
└─# go install github.com/hahwul/dalfox/v2@latest
```

## 2 使用

扫描 GET 传参 XSS

```
┌──(root㉿kali)-[~]
└─# dalfox url '<URL>'
```

扫描存储在文件中的 GET 传参 XSS

```
┌──(root㉿kali)-[~]
└─# dalfox file '/root/urls.txt'
```

扫描 POST 传参 XSS

```
┌──(root㉿kali)-[~]
└─# dalfox url '<URL>' -d 'payload=1'
```

## 3 帮助

```
┌──(root㉿kali)-[~]
└─# dalfox -h
```

```
用法：
  dalfox [标志]
  dalfox [命令]

可用命令：
  completion  为指定的 shell 生成自动完成脚本
  file        使用文件模式（目标列表或原始数据）
  help        获取有关任何命令的帮助
  payload     负载模式，生成并枚举负载
  pipe        使用管道模式
  server      启动 API 服务器
  sxss        使用存储的 XSS 模式
  url         使用单个目标模式
  version     显示版本

标志：
  -b, --blind string                添加你的盲 XSS
                                      * 示例：-b your-callback-url
      --config string               使用文件中的配置
  -C, --cookie string               添加自定义 cookie
      --cookie-from-raw string      从 burp 的原始 HTTP 请求中加载 cookie
                                      * 示例：--cookie-from-raw request.txt
      --custom-alert-type string    更改警报值类型
                                      * 示例：--custom-alert-type=none / --custom-alert-type=str,none（默认为 "none"）
      --custom-alert-value string   更改警报值
                                      * 示例：--custom-alert-value=document.cookie（默认为 "1"）
      --custom-payload string       从文件中添加自定义负载
  -d, --data string                 使用 POST 方法并添加 Body 数据
      --debug                       调试模式，使用 -o 选项保存所有日志
      --deep-domxss                 在无头浏览器上使用更多负载进行 DOM XSS 测试[速度较慢]
      --delay int                   发送到同一主机之间的毫秒数（1000==1s）
  -F, --follow-redirects            跟随重定向
      --format string               标准输出格式
                                      * 支持：plain / json（默认为 "plain"）
      --found-action string         如果发现弱点/漏洞，执行下一步操作(cmd)
                                      * 示例：--found-action='./notify.sh'
      --found-action-shell string   为 --found-action 选择 shell 应用程序（默认为 "bash"）
      --grep string                 使用自定义 grep 文件
                                      * 示例：--grep ./samples/sample_grep.json
      --har-file-path string        保存扫描请求的 HAR 的路径
  -H, --header strings              添加自定义标头
  -h, --help                        获取 dalfox 的帮助
      --ignore-param strings        在扫描时忽略此参数。
                                      * 示例：--ignore-param api_token --ignore-param csrf_token
      --ignore-return string        忽略从返回代码进行扫描
                                      * 示例：--ignore-return 302,403,404
  -X, --method string               强制覆盖 HTTP 方法
                                      * 示例：-X PUT（默认为 "GET"）
      --mining-dict                 使用字典攻击查找新参数，默认为 Gf-Patterns=>XSS（默认为 true）
  -W, --mining-dict-word string     用于参数挖掘的自定义词表文件
                                      * 示例：--mining-dict-word word.txt
      --mining-dom                  在 DOM 中查找新参数（属性/js 值）（默认为 true）
      --no-color                    不使用着色
      --no-spinner                  不使用旋转器
      --only-custom-payload         仅测试自定义负载（需要 --custom-payload）
      --only-discovery              仅测试参数分析（与 '--skip-xss-scanning' 选项相同）
      --only-poc string             仅显示指定模式的 PoC 代码（g: grep / r: reflected / v: verified）
                                     * 示例：--only-poc='g,v'
  -o, --output string               写入输出文件（默认情况下，只保存 PoC 代码）
      --output-all                  所有日志写入模式（-o 或 stdout）
      --output-request              在结果中包含原始 HTTP 请求。
      --output-response             在结果中包含原始 HTTP 响应。
  -p, --param strings               仅测试选定的参数
      --poc-type string             选择 PoC 类型
                                     * 支持：plain/curl/httpie/http-request
                                     * 示例：--poc-type='curl'（默认为 "plain"）
      --proxy string                将所有请求发送到代理服务器
                                      * 示例：--proxy http://127.0.0.1:8080
      --remote-payloads string      使用远程负载进行 XSS 测试
                                      * 支持：portswigger/payloadbox
                                      * 示例：--remote-payloads=portswigger,payloadbox
      --remote-wordlists string     使用远程词表进行参数挖掘
                                      * 支持：burp/assetnote
                                      * 示例：--remote-wordlists=burp
      --report                      显示详细报告
      --report-format string        --report 标志的格式 [plain/json]（默认为 "plain"）
  -S, --silence                     仅打印 PoC 代码和进度（对于管道/文件模式）
      --skip-bav                    跳过 BAV（Basic Another Vulnerability）分析
      --skip-grepping               跳过内置 grep
      --skip-headless               跳过无头浏览器基础扫描[DOM XSS 和 inJS 验证]
      --skip-mining-all             跳过所有参数挖掘
      --skip-mining-dict            跳过基于字典的参数挖掘
      --skip-mining-dom             跳过基于 DOM 的参数挖掘
      --skip-xss-scanning           跳过 XSS 扫描（与 '--only-discovery' 选项相同）
      --timeout int                 超时秒数（默认为 10）
      --user-agent string           添加自定义 UserAgent
      --waf-evasion                 当检测到 WAF 时避免阻止调整速度（worker=1 delay=3s）
  -w, --worker int                  工作者数量（默认为 100）

有关命令的更多信息，请使用 "dalfox [command] --help"。
```

### 3.1 completion

```shell
┌──(root㉿kali)-[~]
└─# dalfox completion -h
```

```
为指定的 shell 生成 dalfox 的自动完成脚本。
有关如何使用生成的脚本的详细信息，请参阅每个子命令的帮助。

用法：
  dalfox completion [命令]

可用命令：
  bash        为 bash 生成自动完成脚本
  fish        为 fish 生成自动完成脚本
  powershell  为 powershell 生成自动完成脚本
  zsh         为 zsh 生成自动完成脚本

标志：
  -h, --help   获取完成的帮助

全局标志：
  -b, --blind string                添加你的盲 XSS
                                      * 示例：-b your-callback-url
      --config string               使用文件中的配置
  -C, --cookie string               添加自定义 cookie
      --cookie-from-raw string      从 burp 的原始 HTTP 请求中加载 cookie
                                      * 示例：--cookie-from-raw request.txt
      --custom-alert-type string    更改警报值类型
                                      * 示例：--custom-alert-type=none / --custom-alert-type=str,none（默认为 "none"）
      --custom-alert-value string   更改警报值
                                      * 示例：--custom-alert-value=document.cookie（默认为 "1"）
      --custom-payload string       从文件中添加自定义负载
  -d, --data string                 使用 POST 方法并添加 Body 数据
      --debug                       调试模式，使用 -o 选项保存所有日志
      --deep-domxss                 在无头浏览器上使用更多负载进行 DOM XSS 测试[速度较慢]
      --delay int                   发送到同一主机之间的毫秒数（1000==1s）
  -F, --follow-redirects            跟随重定向
      --format string               标准输出格式
                                      * 支持：plain / json（默认为 "plain"）
      --found-action string         如果发现弱点/漏洞，执行下一步操作(cmd)
                                      * 示例：--found-action='./notify.sh'
      --found-action-shell string   为 --found-action 选择 shell 应用程序（默认为 "bash"）
      --grep string                 使用自定义 grep 文件
                                      * 示例：--grep ./samples/sample_grep.json
      --har-file-path string        保存扫描请求的 HAR 的路径
  -H, --header strings              添加自定义标头
      --ignore-param strings        在扫描时忽略此参数。
                                      * 示例：--ignore-param api_token --ignore-param csrf_token
      --ignore-return string        忽略从返回代码进行扫描
                                      * 示例：--ignore-return 302,403,404
  -X, --method string               强制覆盖 HTTP 方法
                                      * 示例：-X PUT（默认为 "GET"）
      --mining-dict                 使用字典攻击查找新参数，默认为 Gf-Patterns=>XSS（默认为 true）
  -W, --mining-dict-word string     用于参数挖掘的自定义词表文件
                                      * 示例：--mining-dict-word word.txt
      --mining-dom                  在 DOM 中查找新参数（属性/js 值）（默认为 true）
      --no-color                    不使用着色
      --no-spinner                  不使用旋转器
      --only-custom-payload         仅测试自定义负载（需要 --custom-payload）
      --only-discovery              仅测试参数分析（与 '--skip-xss-scanning' 选项相同）
      --only-poc string             仅显示指定模式的 PoC 代码（g: grep / r: reflected / v: verified）
                                     * 示例：--only-poc='g,v'
  -o, --output string               写入输出文件（默认情况下，只保存 PoC 代码）
      --output-all                  所有日志写入模式（-o 或 stdout）
      --output-request              在结果中包含原始 HTTP 请求。
      --output-response             在结果中包含原始 HTTP 响应。
  -p, --param strings               仅测试选定的参数
      --poc-type string             选择 PoC 类型
                                     * 支持：plain/curl/httpie/http-request
                                     * 示例：--poc-type='curl'（默认为 "plain"）
      --proxy string                将所有请求发送到代理服务器
                                      * 示例：--proxy http://127.0.0.1:8080
      --remote-payloads string      使用远程负载进行 XSS 测试
                                      * 支持：portswigger/payloadbox
                                      * 示例：--remote-payloads=portswigger,payloadbox
      --remote-wordlists string     使用远程词表进行参数挖掘
                                      * 支持：burp/assetnote
                                      * 示例：--remote-wordlists=burp
      --report                      显示详细报告
      --report-format string        --report 标志的格式 [plain/json]（默认为 "plain"）
  -S, --silence                     仅打印 PoC 代码和进度（对于管道/文件模式）
      --skip-bav                    跳过 BAV（Basic Another Vulnerability）分析
      --skip-grepping               跳过内置 grep
      --skip-headless               跳过无头浏览器基础扫描[DOM XSS 和 inJS 验证]
      --skip-mining-all             跳过所有参数挖掘
      --skip-mining-dict            跳过基于字典的参数挖掘
      --skip-mining-dom             跳过基于 DOM 的参数挖掘
      --skip-xss-scanning           跳过 XSS 扫描（与 '--only-discovery' 选项相同）
      --timeout int                 超时秒数（默认为 10）
      --user-agent string           添加自定义 UserAgent
      --waf-evasion                 当检测到 WAF 时避免阻止调整速度（worker=1 delay=3s）
  -w, --worker int                  工作者数量（默认为 100）

有关命令的更多信息，请使用 "dalfox completion [command] --help"。
```

### 3.2 file

```shell
┌──(root㉿kali)-[~]
└─# dalfox file -h
```

```
使用文件模式（目标列表或原始数据）

用法：
  dalfox file [filePath] [标志]

标志：
      --har               [FORMAT] 使用 HAR 格式
  -h, --help              获取文件帮助
      --http              在原始数据模式下强制使用 http
      --mass              并行扫描 N*主机模式（仅显示 PoC 代码）
      --mass-worker int   并行扫描模式的工作线程数（默认为 10）
      --multicast         并行扫描 N*主机模式（仅显示 PoC 代码）
      --rawdata           [FORMAT] 使用来自 Burp/ZAP 的原始请求数据
      --silence-force     仅打印 PoC（不打印进度）

全局标志：
  -b, --blind string                添加你的盲 XSS
                                      * 示例：-b your-callback-url
      --config string               使用文件中的配置
  -C, --cookie string               添加自定义 cookie
      --cookie-from-raw string      从 burp 的原始 HTTP 请求中加载 cookie
                                      * 示例：--cookie-from-raw request.txt
      --custom-alert-type string    更改警报值类型
                                      * 示例：--custom-alert-type=none / --custom-alert-type=str,none（默认为 "none"）
      --custom-alert-value string   更改警报值
                                      * 示例：--custom-alert-value=document.cookie（默认为 "1"）
      --custom-payload string       从文件中添加自定义负载
  -d, --data string                 使用 POST 方法并添加 Body 数据
      --debug                       调试模式，使用 -o 选项保存所有日志
      --deep-domxss                 在无头浏览器上使用更多负载进行 DOM XSS 测试[速度较慢]
      --delay int                   发送到同一主机之间的毫秒数（1000==1s）
  -F, --follow-redirects            跟随重定向
      --format string               标准输出格式
                                      * 支持：plain / json（默认为 "plain"）
      --found-action string         如果发现弱点/漏洞，执行下一步操作(cmd)
                                      * 示例：--found-action='./notify.sh'
      --found-action-shell string   为 --found-action 选择 shell 应用程序（默认为 "bash"）
      --grep string                 使用自定义 grep 文件
                                      * 示例：--grep ./samples/sample_grep.json
      --har-file-path string        保存扫描请求的 HAR 的路径
  -H, --header strings              添加自定义标头
      --ignore-param strings        在扫描时忽略此参数。
                                      * 示例：--ignore-param api_token --ignore-param csrf_token
      --ignore-return string        忽略从返回代码进行扫描
                                      * 示例：--ignore-return 302,403,404
  -X, --method string               强制覆盖 HTTP 方法
                                      * 示例：-X PUT（默认为 "GET"）
      --mining-dict                 使用字典攻击查找新参数，默认为 Gf-Patterns=>XSS（默认为 true）
  -W, --mining-dict-word string     用于参数挖掘的自定义词表文件
                                      * 示例：--mining-dict-word word.txt
      --mining-dom                  在 DOM 中查找新参数（属性/js 值）（默认为 true）
      --no-color                    不使用着色
      --no-spinner                  不使用旋转器
      --only-custom-payload         仅测试自定义负载（需要 --custom-payload）
      --only-discovery              仅测试参数分析（与 '--skip-xss-scanning' 选项相同）
      --only-poc string             仅显示指定模式的 PoC 代码（g: grep / r: reflected / v: verified）
                                     * 示例：--only-poc='g,v'
  -o, --output string               写入输出文件（默认情况下，只保存 PoC 代码）
      --output-all                  所有日志写入模式（-o 或 stdout）
      --output-request              在结果中包含原始 HTTP 请求。
      --output-response             在结果中包含原始 HTTP 响应。
  -p, --param strings               仅测试选定的参数
      --poc-type string             选择 PoC 类型
                                     * 支持：plain/curl/httpie/http-request
                                     * 示例：--poc-type='curl'（默认为 "plain"）
      --proxy string                将所有请求发送到代理服务器
                                      * 示例：--proxy http://127.0.0.1:8080
      --remote-payloads string      使用远程负载进行 XSS 测试
                                      * 支持：portswigger/payloadbox
                                      * 示例：--remote-payloads=portswigger,payloadbox
      --remote-wordlists string     使用远程词表进行参数挖掘
                                      * 支持：burp/assetnote
                                      * 示例：--remote-wordlists=burp
      --report                      显示详细报告
      --report-format string        --report 标志的格式 [plain/json]（默认为 "plain"）
  -S, --silence                     仅打印 PoC 代码和进度（对于管道/文件模式）
      --skip-bav                    跳过 BAV（Basic Another Vulnerability）分析
      --skip-grepping               跳过内置 grep
      --skip-headless               跳过无头浏览器基础扫描[DOM XSS 和 inJS 验证]
      --skip-mining-all             跳过所有参数挖掘
      --skip-mining-dict            跳过基于字典的参数挖掘
      --skip-mining-dom             跳过基于 DOM 的参数挖掘
      --skip-xss-scanning           跳过 XSS 扫描（与 '--only-discovery' 选项相同）
      --timeout int                 超时秒数（默认为 10）
      --user-agent string           添加自定义 UserAgent
      --waf-evasion                 当检测到 WAF 时避免阻止调整速度（worker=1 delay=3s）
  -w, --worker int                  工作者数量（默认为 100）
```

### 3.3 payload

```shell
┌──(root㉿kali)-[~]
└─# dalfox payload -h
```

```
负载模式，创建和枚举负载

用法：
  dalfox payload [标志]

标志：
      --encoder-url            编码输出 [URL]
      --entity-event-handler   枚举 xss 的事件处理程序
      --entity-gf              枚举 gf-patterns xss 参数
      --entity-special-chars   枚举 xss 的特殊字符
      --entity-useful-tags     枚举 xss 的有用标签
      --enum-attr              枚举在属性中的 xss 负载
      --enum-common            枚举常见的 xss 负载
      --enum-html              枚举在 HTML 中的 xss 负载
      --enum-injs              枚举在 JavaScript 中的 xss 负载
  -h, --help                   获取负载帮助
      --make-bulk              为存储的 xss 创建批量负载
      --remote-payloadbox      枚举 payloadbox 的 xss 负载
      --remote-portswigger     枚举 portswigger xss cheatsheet 负载

全局标志：
  -b, --blind string                添加你的盲 XSS
                                      * 示例：-b your-callback-url
      --config string               使用文件中的配置
  -C, --cookie string               添加自定义 cookie
      --cookie-from-raw string      从 burp 的原始 HTTP 请求中加载 cookie
                                      * 示例：--cookie-from-raw request.txt
      --custom-alert-type string    更改警报值类型
                                      * 示例：--custom-alert-type=none / --custom-alert-type=str,none（默认为 "none"）
      --custom-alert-value string   更改警报值
                                      * 示例：--custom-alert-value=document.cookie（默认为 "1"）
      --custom-payload string       从文件中添加自定义负载
  -d, --data string                 使用 POST 方法并添加 Body 数据
      --debug                       调试模式，使用 -o 选项保存所有日志
      --deep-domxss                 在无头浏览器上使用更多负载进行 DOM XSS 测试[速度较慢]
      --delay int                   发送到同一主机之间的毫秒数（1000==1s）
  -F, --follow-redirects            跟随重定向
      --format string               标准输出格式
                                      * 支持：plain / json（默认为 "plain"）
      --found-action string         如果发现弱点/漏洞，执行下一步操作(cmd)
                                      * 示例：--found-action='./notify.sh'
      --found-action-shell string   为 --found-action 选择 shell 应用程序（默认为 "bash"）
      --grep string                 使用自定义 grep 文件
                                      * 示例：--grep ./samples/sample_grep.json
      --har-file-path string        保存扫描请求的 HAR 的路径
  -H, --header strings              添加自定义标头
      --ignore-param strings        在扫描时忽略此参数。
                                      * 示例：--ignore-param api_token --ignore-param csrf_token
      --ignore-return string        忽略从返回代码进行扫描
                                      * 示例：--ignore-return 302,403,404
  -X, --method string               强制覆盖 HTTP 方法
                                      * 示例：-X PUT（默认为 "GET"）
      --mining-dict                 使用字典攻击查找新参数，默认为 Gf-Patterns=>XSS（默认为 true）
  -W, --mining-dict-word string     用于参数挖掘的自定义词表文件
                                      * 示例：--mining-dict-word word.txt
      --mining-dom                  在 DOM 中查找新参数（属性/js 值）（默认为 true）
      --no-color                    不使用着色
      --no-spinner                  不使用旋转器
      --only-custom-payload         仅测试自定义负载（需要 --custom-payload）
      --only-discovery              仅测试参数分析（与 '--skip-xss-scanning' 选项相同）
      --only-poc string             仅显示指定模式的 PoC 代码（g: grep / r: reflected / v: verified）
                                     * 示例：--only-poc='g,v'
  -o, --output string               写入输出文件（默认情况下，只保存 PoC 代码）
      --output-all                  所有日志写入模式（-o 或 stdout）
      --output-request              在结果中包含原始 HTTP 请求。
      --output-response             在结果中包含原始 HTTP 响应。
  -p, --param strings               仅测试选定的参数
      --poc-type string             选择 PoC 类型
                                     * 支持：plain/curl/httpie/http-request
                                     * 示例：--poc-type='curl'（默认为 "plain"）
      --proxy string                将所有请求发送到代理服务器
                                      * 示例：--proxy http://127.0.0.1:8080
      --remote-payloads string      使用远程负载进行 XSS 测试
                                      * 支持：portswigger/payloadbox
                                      * 示例：--remote-payloads=portswigger,payloadbox
      --remote-wordlists string     使用远程词表进行参数挖掘
                                      * 支持：burp/assetnote
                                      * 示例：--remote-wordlists=burp
      --report                      显示详细报告
      --report-format string        --report 标志的格式 [plain/json]（默认为 "plain"）
  -S, --silence                     仅打印 PoC 代码和进度（对于管道/文件模式）
      --skip-bav                    跳过 BAV（Basic Another Vulnerability）分析
      --skip-grepping               跳过内置 grep
      --skip-headless               跳过无头浏览器基础扫描[DOM XSS 和 inJS 验证]
      --skip-mining-all             跳过所有参数挖掘
      --skip-mining-dict            跳过基于字典的参数挖掘
      --skip-mining-dom             跳过基于 DOM 的参数挖掘
      --skip-xss-scanning           跳过 XSS 扫描（与 '--only-discovery' 选项相同）
      --timeout int                 超时秒数（默认为 10）
      --user-agent string           添加自定义 UserAgent
      --waf-evasion                 当检测到 WAF 时避免阻止调整速度（worker=1 delay=3s）
  -w, --worker int                  工作者数量（默认为 100）
```

### 3.4 pipe

```shell
┌──(root㉿kali)-[~]
└─# dalfox pipe -h
```

```
使用管道模式

用法：
  dalfox pipe [标志]

标志：
  -h, --help              获取管道帮助
      --mass              并行扫描 N*Host 模式（仅显示 poc 代码）
      --mass-worker int   --mass 和 --multicast 选项的并行工作者（默认为 10）
      --multicast         并行扫描 N*Host 模式（仅显示 poc 代码）
      --silence-force     仅打印 PoC（不打印进度）

全局标志：
  -b, --blind string                添加你的盲 XSS
                                      * 示例：-b your-callback-url
      --config string               使用文件中的配置
  -C, --cookie string               添加自定义 cookie
      --cookie-from-raw string      从 burp 的原始 HTTP 请求中加载 cookie
                                      * 示例：--cookie-from-raw request.txt
      --custom-alert-type string    更改警报值类型
                                      * 示例：--custom-alert-type=none / --custom-alert-type=str,none（默认为 "none"）
      --custom-alert-value string   更改警报值
                                      * 示例：--custom-alert-value=document.cookie（默认为 "1"）
      --custom-payload string       从文件中添加自定义负载
  -d, --data string                 使用 POST 方法并添加 Body 数据
      --debug                       调试模式，使用 -o 选项保存所有日志
      --deep-domxss                 在无头浏览器上使用更多负载进行 DOM XSS 测试[速度较慢]
      --delay int                   发送到同一主机之间的毫秒数（1000==1s）
  -F, --follow-redirects            跟随重定向
      --format string               标准输出格式
                                      * 支持：plain / json（默认为 "plain"）
      --found-action string         如果发现弱点/漏洞，执行下一步操作(cmd)
                                      * 示例：--found-action='./notify.sh'
      --found-action-shell string   为 --found-action 选择 shell 应用程序（默认为 "bash"）
      --grep string                 使用自定义 grep 文件
                                      * 示例：--grep ./samples/sample_grep.json
      --har-file-path string        保存扫描请求的 HAR 的路径
  -H, --header strings              添加自定义标头
      --ignore-param strings        在扫描时忽略此参数。
                                      * 示例：--ignore-param api_token --ignore-param csrf_token
      --ignore-return string        忽略从返回代码进行扫描
                                      * 示例：--ignore-return 302,403,404
  -X, --method string               强制覆盖 HTTP 方法
                                      * 示例：-X PUT（默认为 "GET"）
      --mining-dict                 使用字典攻击查找新参数，默认为 Gf-Patterns=>XSS（默认为 true）
  -W, --mining-dict-word string     用于参数挖掘的自定义词表文件
                                      * 示例：--mining-dict-word word.txt
      --mining-dom                  在 DOM 中查找新参数（属性/js 值）（默认为 true）
      --no-color                    不使用着色
      --no-spinner                  不使用旋转器
      --only-custom-payload         仅测试自定义负载（需要 --custom-payload）
      --only-discovery              仅测试参数分析（与 '--skip-xss-scanning' 选项相同）
      --only-poc string             仅显示指定模式的 PoC 代码（g: grep / r: reflected / v: verified）
                                     * 示例：--only-poc='g,v'
  -o, --output string               写入输出文件（默认情况下，只保存 PoC 代码）
      --output-all                  所有日志写入模式（-o 或 stdout）
      --output-request              在结果中包含原始 HTTP 请求。
      --output-response             在结果中包含原始 HTTP 响应。
  -p, --param strings               仅测试选定的参数
      --poc-type string             选择 PoC 类型
                                     * 支持：plain/curl/httpie/http-request
                                     * 示例：--poc-type='curl'（默认为 "plain"）
      --proxy string                将所有请求发送到代理服务器
                                      * 示例：--proxy http://127.0.0.1:8080
      --remote-payloads string      使用远程负载进行 XSS 测试
                                      * 支持：portswigger/payloadbox
                                      * 示例：--remote-payloads=portswigger,payloadbox
      --remote-wordlists string     使用远程词表进行参数挖掘
                                      * 支持：burp/assetnote
                                      * 示例：--remote-wordlists=burp
      --report                      显示详细报告
      --report-format string        --report 标志的格式 [plain/json]（默认为 "plain"）
  -S, --silence                     仅打印 PoC 代码和进度（对于管道/文件模式）
      --skip-bav                    跳过 BAV（Basic Another Vulnerability）分析
      --skip-grepping               跳过内置 grep
      --skip-headless               跳过无头浏览器基础扫描[DOM XSS 和 inJS 验证]
      --skip-mining-all             跳过所有参数挖掘
      --skip-mining-dict            跳过基于字典的参数挖掘
      --skip-mining-dom             跳过基于 DOM 的参数挖掘
      --skip-xss-scanning           跳过 XSS 扫描（与 '--only-discovery' 选项相同）
      --timeout int                 超时秒数（默认为 10）
      --user-agent string           添加自定义 UserAgent
      --waf-evasion                 当检测到 WAF 时避免阻止调整速度（worker=1 delay=3s）
  -w, --worker int                  工作者数量（默认为 100）
```

### 3.5 server

```shell
┌──(root㉿kali)-[~]
└─# dalfox server -h
```

```
启动 API 服务器

用法：
  dalfox server [标志]

标志：
  -h, --help          获取服务器帮助
      --host string   绑定地址（默认为 "0.0.0.0"）
      --port int      绑定端口（默认为 6664）

全局标志：
  -b, --blind string                添加你的盲 XSS
                                      * 示例：-b your-callback-url
      --config string               使用文件中的配置
  -C, --cookie string               添加自定义 cookie
      --cookie-from-raw string      从 burp 的原始 HTTP 请求中加载 cookie
                                      * 示例：--cookie-from-raw request.txt
      --custom-alert-type string    更改警报值类型
                                      * 示例：--custom-alert-type=none / --custom-alert-type=str,none（默认为 "none"）
      --custom-alert-value string   更改警报值
                                      * 示例：--custom-alert-value=document.cookie（默认为 "1"）
      --custom-payload string       从文件中添加自定义负载
  -d, --data string                 使用 POST 方法并添加 Body 数据
      --debug                       调试模式，使用 -o 选项保存所有日志
      --deep-domxss                 在无头浏览器上使用更多负载进行 DOM XSS 测试[速度较慢]
      --delay int                   发送到同一主机之间的毫秒数（1000==1s）
  -F, --follow-redirects            跟随重定向
      --format string               标准输出格式
                                      * 支持：plain / json（默认为 "plain"）
      --found-action string         如果发现弱点/漏洞，执行下一步操作(cmd)
                                      * 示例：--found-action='./notify.sh'
      --found-action-shell string   为 --found-action 选择 shell 应用程序（默认为 "bash"）
      --grep string                 使用自定义 grep 文件
                                      * 示例：--grep ./samples/sample_grep.json
      --har-file-path string        保存扫描请求的 HAR 的路径
  -H, --header strings              添加自定义标头
      --ignore-param strings        在扫描时忽略此参数。
                                      * 示例：--ignore-param api_token --ignore-param csrf_token
      --ignore-return string        忽略从返回代码进行扫描
                                      * 示例：--ignore-return 302,403,404
  -X, --method string               强制覆盖 HTTP 方法
                                      * 示例：-X PUT（默认为 "GET"）
      --mining-dict                 使用字典攻击查找新参数，默认为 Gf-Patterns=>XSS（默认为 true）
  -W, --mining-dict-word string     用于参数挖掘的自定义词表文件
                                      * 示例：--mining-dict-word word.txt
      --mining-dom                  在 DOM 中查找新参数（属性/js 值）（默认为 true）
      --no-color                    不使用着色
      --no-spinner                  不使用旋转器
      --only-custom-payload         仅测试自定义负载（需要 --custom-payload）
      --only-discovery              仅测试参数分析（与 '--skip-xss-scanning' 选项相同）
      --only-poc string             仅显示指定模式的 PoC 代码（g: grep / r: reflected / v: verified）
                                     * 示例：--only-poc='g,v'
  -o, --output string               写入输出文件（默认情况下，只保存 PoC 代码）
      --output-all                  所有日志写入模式（-o 或 stdout）
      --output-request              在结果中包含原始 HTTP 请求。
      --output-response             在结果中包含原始 HTTP 响应。
  -p, --param strings               仅测试选定的参数
      --poc-type string             选择 PoC 类型
                                     * 支持：plain/curl/httpie/http-request
                                     * 示例：--poc-type='curl'（默认为 "plain"）
      --proxy string                将所有请求发送到代理服务器
                                      * 示例：--proxy http://127.0.0.1:8080
      --remote-payloads string      使用远程负载进行 XSS 测试
                                      * 支持：portswigger/payloadbox
                                      * 示例：--remote-payloads=portswigger,payloadbox
      --remote-wordlists string     使用远程词表进行参数挖掘
                                      * 支持：burp/assetnote
                                      * 示例：--remote-wordlists=burp
      --report                      显示详细报告
      --report-format string        --report 标志的格式 [plain/json]（默认为 "plain"）
  -S, --silence                     仅打印 PoC 代码和进度（对于管道/文件模式）
      --skip-bav                    跳过 BAV（Basic Another Vulnerability）分析
      --skip-grepping               跳过内置 grep
      --skip-headless               跳过无头浏览器基础扫描[DOM XSS 和 inJS 验证]
      --skip-mining-all             跳过所有参数挖掘
      --skip-mining-dict            跳过基于字典的参数挖掘
      --skip-mining-dom             跳过基于 DOM 的参数挖掘
      --skip-xss-scanning           跳过 XSS 扫描（与 '--only-discovery' 选项相同）
      --timeout int                 超时秒数（默认为 10）
      --user-agent string           添加自定义 UserAgent
      --waf-evasion                 当检测到 WAF 时避免阻止调整速度（worker=1 delay=3s）
  -w, --worker int                  工作者数量（默认为 100）
```

### 3.6 sxss

```shell
┌──(root㉿kali)-[~]
└─# dalfox sxss -h
```

```
使用存储的 XSS 模式

用法：
  dalfox sxss [目标] [标志]

标志：
  -h, --help                    获取 sxss 帮助
      --request-method string   发送到服务器的请求方法（默认为 "GET"）
      --sequence int            设置序列为第一个数字
                                  * 示例：--trigger=https://~/view?no=SEQNC --sequence=3（默认为 -1）
      --trigger string          注入 sxss 代码后检查此 URL
                                  * 示例：--trigger=https://~~/profile

全局标志：
  -b, --blind string                添加你的盲 XSS
                                      * 示例：-b your-callback-url
      --config string               使用文件中的配置
  -C, --cookie string               添加自定义 cookie
      --cookie-from-raw string      从 burp 的原始 HTTP 请求中加载 cookie
                                      * 示例：--cookie-from-raw request.txt
      --custom-alert-type string    更改警报值类型
                                      * 示例：--custom-alert-type=none / --custom-alert-type=str,none（默认为 "none"）
      --custom-alert-value string   更改警报值
                                      * 示例：--custom-alert-value=document.cookie（默认为 "1"）
      --custom-payload string       从文件中添加自定义负载
  -d, --data string                 使用 POST 方法并添加 Body 数据
      --debug                       调试模式，使用 -o 选项保存所有日志
      --deep-domxss                 在无头浏览器上使用更多负载进行 DOM XSS 测试[速度较慢]
      --delay int                   发送到同一主机之间的毫秒数（1000==1s）
  -F, --follow-redirects            跟随重定向
      --format string               标准输出格式
                                      * 支持：plain / json（默认为 "plain"）
      --found-action string         如果发现弱点/漏洞，执行下一步操作(cmd)
                                      * 示例：--found-action='./notify.sh'
      --found-action-shell string   为 --found-action 选择 shell 应用程序（默认为 "bash"）
      --grep string                 使用自定义 grep 文件
                                      * 示例：--grep ./samples/sample_grep.json
      --har-file-path string        保存扫描请求的 HAR 的路径
  -H, --header strings              添加自定义标头
      --ignore-param strings        在扫描时忽略此参数。
                                      * 示例：--ignore-param api_token --ignore-param csrf_token
      --ignore-return string        忽略从返回代码进行扫描
                                      * 示例：--ignore-return 302,403,404
  -X, --method string               强制覆盖 HTTP 方法
                                      * 示例：-X PUT（默认为 "GET"）
      --mining-dict                 使用字典攻击查找新参数，默认为 Gf-Patterns=>XSS（默认为 true）
  -W, --mining-dict-word string     用于参数挖掘的自定义词表文件
                                      * 示例：--mining-dict-word word.txt
      --mining-dom                  在 DOM 中查找新参数（属性/js 值）（默认为 true）
      --no-color                    不使用着色
      --no-spinner                  不使用旋转器
      --only-custom-payload         仅测试自定义负载（需要 --custom-payload）
      --only-discovery              仅测试参数分析（与 '--skip-xss-scanning' 选项相同）
      --only-poc string             仅显示指定模式的 PoC 代码（g: grep / r: reflected / v: verified）
                                     * 示例：--only-poc='g,v'
  -o, --output string               写入输出文件（默认情况下，只保存 PoC 代码）
      --output-all                  所有日志写入模式（-o 或 stdout）
      --output-request              在结果中包含原始 HTTP 请求。
      --output-response             在结果中包含原始 HTTP 响应。
  -p, --param strings               仅测试选定的参数
      --poc-type string             选择 PoC 类型
                                     * 支持：plain/curl/httpie/http-request
                                     * 示例：--poc-type='curl'（默认为 "plain"）
      --proxy string                将所有请求发送到代理服务器
                                      * 示例：--proxy http://127.0.0.1:8080
      --remote-payloads string      使用远程负载进行 XSS 测试
                                      * 支持：portswigger/payloadbox
                                      * 示例：--remote-payloads=portswigger,payloadbox
      --remote-wordlists string     使用远程词表进行参数挖掘
                                      * 支持：burp/assetnote
                                      * 示例：--remote-wordlists=burp
      --report                      显示详细报告
      --report-format string        --report 标志的格式 [plain/json]（默认为 "plain"）
  -S, --silence                     仅打印 PoC 代码和进度（对于管道/文件模式）
      --skip-bav                    跳过 BAV（Basic Another Vulnerability）分析
      --skip-grepping               跳过内置 grep
      --skip-headless               跳过无头浏览器基础扫描[DOM XSS 和 inJS 验证]
      --skip-mining-all             跳过所有参数挖掘
      --skip-mining-dict            跳过基于字典的参数挖掘
      --skip-mining-dom             跳过基于 DOM 的参数挖掘
      --skip-xss-scanning           跳过 XSS 扫描（与 '--only-discovery' 选项相同）
      --timeout int                 超时秒数（默认为 10）
      --user-agent string           添加自定义 UserAgent
      --waf-evasion                 当检测到 WAF 时避免阻止调整速度（worker=1 delay=3s）
  -w, --worker int                  工作者数量（默认为 100）
```

### 3.7 url

```shell
┌──(root㉿kali)-[~]
└─# dalfox url -h
```

```
使用单目标模式

用法：
  dalfox url [目标] [标志]

标志：
  -h, --help   获取 URL 帮助

全局标志：
  -b, --blind string                添加你的盲 XSS
                                      * 示例：-b your-callback-url
      --config string               使用文件中的配置
  -C, --cookie string               添加自定义 cookie
      --cookie-from-raw string      从 burp 的原始 HTTP 请求中加载 cookie
                                      * 示例：--cookie-from-raw request.txt
      --custom-alert-type string    更改警报值类型
                                      * 示例：--custom-alert-type=none / --custom-alert-type=str,none（默认为 "none"）
      --custom-alert-value string   更改警报值
                                      * 示例：--custom-alert-value=document.cookie（默认为 "1"）
      --custom-payload string       从文件中添加自定义负载
  -d, --data string                 使用 POST 方法并添加 Body 数据
      --debug                       调试模式，使用 -o 选项保存所有日志
      --deep-domxss                 在无头浏览器上使用更多负载进行 DOM XSS 测试[速度较慢]
      --delay int                   发送到同一主机之间的毫秒数（1000==1s）
  -F, --follow-redirects            跟随重定向
      --format string               标准输出格式
                                      * 支持：plain / json（默认为 "plain"）
      --found-action string         如果发现弱点/漏洞，执行下一步操作(cmd)
                                      * 示例：--found-action='./notify.sh'
      --found-action-shell string   为 --found-action 选择 shell 应用程序（默认为 "bash"）
      --grep string                 使用自定义 grep 文件
                                      * 示例：--grep ./samples/sample_grep.json
      --har-file-path string        保存扫描请求的 HAR 的路径
  -H, --header strings              添加自定义标头
      --ignore-param strings        在扫描时忽略此参数。
                                      * 示例：--ignore-param api_token --ignore-param csrf_token
      --ignore-return string        忽略从返回代码进行扫描
                                      * 示例：--ignore-return 302,403,404
  -X, --method string               强制覆盖 HTTP 方法
                                      * 示例：-X PUT（默认为 "GET"）
      --mining-dict                 使用字典攻击查找新参数，默认为 Gf-Patterns=>XSS（默认为 true）
  -W, --mining-dict-word string     用于参数挖掘的自定义词表文件
                                      * 示例：--mining-dict-word word.txt
      --mining-dom                  在 DOM 中查找新参数（属性/js 值）（默认为 true）
      --no-color                    不使用着色
      --no-spinner                  不使用旋转器
      --only-custom-payload         仅测试自定义负载（需要 --custom-payload）
      --only-discovery              仅测试参数分析（与 '--skip-xss-scanning' 选项相同）
      --only-poc string             仅显示指定模式的 PoC 代码（g: grep / r: reflected / v: verified）
                                     * 示例：--only-poc='g,v'
  -o, --output string               写入输出文件（默认情况下，只保存 PoC 代码）
      --output-all                  所有日志写入模式（-o 或 stdout）
      --output-request              在结果中包含原始 HTTP 请求。
      --output-response             在结果中包含原始 HTTP 响应。
  -p, --param strings               仅测试选定的参数
      --poc-type string             选择 PoC 类型
                                     * 支持：plain/curl/httpie/http-request
                                     * 示例：--poc-type='curl'（默认为 "plain"）
      --proxy string                将所有请求发送到代理服务器
                                      * 示例：--proxy http://127.0.0.1:8080
      --remote-payloads string      使用远程负载进行 XSS 测试
                                      * 支持：portswigger/payloadbox
                                      * 示例：--remote-payloads=portswigger,payloadbox
      --remote-wordlists string     使用远程词表进行参数挖掘
                                      * 支持：burp/assetnote
                                      * 示例：--remote-wordlists=burp
      --report                      显示详细报告
      --report-format string        --report 标志的格式 [plain/json]（默认为 "plain"）
  -S, --silence                     仅打印 PoC 代码和进度（对于管道/文件模式）
      --skip-bav                    跳过 BAV（Basic Another Vulnerability）分析
      --skip-grepping               跳过内置 grep
      --skip-headless               跳过无头浏览器基础扫描[DOM XSS 和 inJS 验证]
      --skip-mining-all             跳过所有参数挖掘
      --skip-mining-dict            跳过基于字典的参数挖掘
      --skip-mining-dom             跳过基于 DOM 的参数挖掘
      --skip-xss-scanning           跳过 XSS 扫描（与 '--only-discovery' 选项相同）
      --timeout int                 超时秒数（默认为 10）
      --user-agent string           添加自定义 UserAgent
      --waf-evasion                 当检测到 WAF 时避免阻止调整速度（worker=1 delay=3s）
  -w, --worker int                  工作者数量（默认为 100）
```

---

References

- [dalfox](https://github.com/hahwul/dalfox)
