某些情况下 Burp 捕获的参数值会被加密;

可以从 Sources 中查找 `encrypt`, `AES`, `sm4` 或请求包中的参数名;

使用断点排查函数调用情况, 当输入值发生变化时说明加密函数就在附近;

找到加密后的执行代码中打上断点 (如登录, 发送短信);

此时可在 Console 中调用这个函数:

```javascript
console.log(self.encoder("Hello, World!"));
```

可以将 JS 文件上传到 AI 定位加密函数和执行代码的位置.