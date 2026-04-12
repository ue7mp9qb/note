SourceMap 中存储了大量 JS 文件的索引, 可用于下载网站的 JS 文件.

sourceMappingURL 一般会在 JS 文件中

![](./../../../images/SourceMap_%E6%B3%84%E9%9C%B2/sourceMappingURL%20%E4%B8%80%E8%88%AC%E4%BC%9A%E5%9C%A8%20JS%20%E6%96%87%E4%BB%B6%E4%B8%AD.png)

安装 [Tampermonkey](https://chromewebstore.google.com/detail/tampermonkey/dhdgffkkebhmkfjojejmpbldmpobfkfo?hl=en-US&utm_source=ext_sidebar) 脚本 [sourcemap-searcher](https://greasyfork.org/zh-CN/scripts/447335-sourcemap-searcher) 后在控制台执行 `sms()` 检测 sourceMappingURL

```
Console
> sms()
------bingo------
http://www.example.com/test.js.map 疑似存在sourcemap文件泄露
------bingo------
http://www.example.com/test.css.map 疑似存在sourcemap文件泄露
```

分析 [shuji](https://github.com/paazmaya/shuji) 逆向后的 JS 文件中有无敏感信息

---

References

- [0x3 漏洞二：sourcemap文件泄露漏洞](https://mp.weixin.qq.com/s/xFGHpYQSP5sQ_5kEXtolHA)

- [Vue 源码泄露](https://cloud.tencent.com/developer/article/2149039?from=15425)

- [安服水洞系列|Vue源码泄露](https://www.freebuf.com/articles/web/362520.html)

- ```
  aHR0cDovL3d3dy5qaW5iYW9tdXNpYy5jb20v
  ```
