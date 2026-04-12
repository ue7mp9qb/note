通过 `<iframe>` 标签将目标网站隐藏到伪造的页面中, 当目标用户被诱导使用伪造网站进行操作时, 用户的操作将直接作用到目标网站.

## 1. PoC

1. 使用 [ClickjackPoc](https://github.com/Raiders0786/ClickjackPoc/tree/master) 批量检测可能存在 Clickjacking 的目标

2. 使用 [Clickjacking.html](https://github.com/jadensalas469466/tools/blob/main/hack/PoC/Clickjacking.html) 测试

## 2. Exploit

1. 使用 [Burp Clickbandit](https://portswigger.net/burp/documentation/desktop/tools/clickbandit) 自动生成 PoC 检测目标

   ![使用 Burp Clickbandit 自动生成 PoC 检测目标](./../../../images/Clickjacking/%E4%BD%BF%E7%94%A8%20Burp%20Clickbandit%20%E8%87%AA%E5%8A%A8%E7%94%9F%E6%88%90%20PoC%20%E6%A3%80%E6%B5%8B%E7%9B%AE%E6%A0%87.png)

2. 使用 [Testing for Clickjacking](https://github.com/OWASP/www-project-web-security-testing-guide/blob/master/v41/4-Web_Application_Security_Testing/11-Client_Side_Testing/09-Testing_for_Clickjacking.md) 尝试手动绕过

---

References

- [OWASP Clickjacking](https://owasp.org/www-community/attacks/Clickjacking)
- [PortSwigger Clickjacking](https://portswigger.net/web-security/clickjacking)