AccessKey ID 和 AccessKey Secret 是调用云服务器 API 时用于完成身份验证的凭证,

AccessKey 泄露会对该账号下所有资源的安全带来威胁.

## 1. 查找

在 Search Public Code 或 GitHub 中审计相关仓库

```
"example.com"
```

在 FindSomething 中查找 Access 字样

使用 HaE 匹配正则表达式

- 阿里云 (Alibaba Cloud) 的 Access Key 开头标识一般是 "LTAI"

  ```
  ^LTAI[A-Za-z0-9]{12,20}$
  ```

- 腾讯云 (Tencent Cloud) 的 Access Key 开头标识一般是 "AKID"

  ```
  ^AKID[A-Za-z0-9]{13,20}$
  ```

## 2. 接管

得到 API 凭证后后可尝试接管

- 阿里云: [oss-browser](https://github.com/aliyun/oss-browser)
- 腾讯云: [cosbrowser](https://github.com/TencentCloud/cosbrowser)

---

References

- [云业务 AccessKey 标识特征整理](https://wiki.teamssix.com/cloudservice/more/)
- [记一次AccessKey值泄露的挖掘和分析](https://rivers.chaitin.cn/blog/cqq5arp0lnec5jjugkqg)

