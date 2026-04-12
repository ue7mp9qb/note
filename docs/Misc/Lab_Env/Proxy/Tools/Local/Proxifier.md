Proxifier allows network applications that do not support working through proxy servers to operate through a SOCKS or HTTPS proxy and chains.

## 1. Init

添加 Burp Suite 代理

![添加 burp 代理](./../../../../../../images/Proxifier/%E6%B7%BB%E5%8A%A0%20Burp%20Suite%20%E4%BB%A3%E7%90%86.png)

创建  java.exe 和 xray.exe 直连规则

![创建  java.exe 和 xray.exe 直连规则](./../../../../../../images/Proxifier/%E5%88%9B%E5%BB%BA%20%20java.exe%20%E5%92%8C%20xray.exe%20%E7%9B%B4%E8%BF%9E%E8%A7%84%E5%88%99.png)

## 2. Usage

![配置代理](./../../../../../../images/Proxifier/%E9%85%8D%E7%BD%AE%E4%BB%A3%E7%90%86.png)

找到 bluestacks 的进程名 HD-Player.exe

![找到 bluestacks 的进程名 HD-Player.exe](./../../../../../../images/Proxifier/%E6%89%BE%E5%88%B0%20bluestacks%20%E7%9A%84%E8%BF%9B%E7%A8%8B%E5%90%8D%20HD-Player.exe.png)

创建规则只转发 HD-Player.exe 的流量

![创建规则只转发 HD-Player.exe 的流量](./../../../../../../images/Proxifier/%E5%88%9B%E5%BB%BA%E8%A7%84%E5%88%99%E5%8F%AA%E8%BD%AC%E5%8F%91%20HD-Player.exe%20%E7%9A%84%E6%B5%81%E9%87%8F.png)

---

References

- [Proxifier](https://www.proxifier.com/)
