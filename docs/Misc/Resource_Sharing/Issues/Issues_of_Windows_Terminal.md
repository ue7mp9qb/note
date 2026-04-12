在 WindowsTerminal 中输入空格会出现黑色背景

![](./../../../../images/Issues_of_Windows_Terminal/%E5%9C%A8%20WindowsTerminal%20%E4%B8%AD%E8%BE%93%E5%85%A5%E7%A9%BA%E6%A0%BC%E4%BC%9A%E5%87%BA%E7%8E%B0%E9%BB%91%E8%89%B2%E8%83%8C%E6%99%AF.png)

安装或更新 PSReadline 即可 (以管理员身份运行)

```
Install-Module -Name PSReadLine -Scope AllUsers -AllowClobber -Force # 安装
Update-Module -Name PSReadline # 更新
```

> 重启 WindowsTerminal 后生效

---

References

- [Windows Terminal 文本出现纯色背景问题解决方法](https://www.gerenbiji.com/blog/2025%E5%B9%B4/Windows%20Terminal%20%E6%96%87%E6%9C%AC%E5%87%BA%E7%8E%B0%E7%BA%AF%E8%89%B2%E8%83%8C%E6%99%AF%E9%97%AE%E9%A2%98%E8%A7%A3%E5%86%B3%E6%96%B9%E6%B3%95/)