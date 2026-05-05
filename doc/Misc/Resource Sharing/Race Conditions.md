Race conditions are a common type of vulnerability closely related to business logic flaws. They occur when websites process requests concurrently without adequate safeguards. This can lead to multiple distinct threads interacting with the same data at the same time, resulting in a "collision" that causes unintended behavior in the application. A race condition attack uses carefully timed requests to cause intentional collisions and exploit this unintended behavior for malicious purposes.

## 1. Principle

通过并发绕过限制次数

### 1.1. Location

限制了次数的位置如: 抽奖\优惠\限购

## 2. Test

### 2.1. Limit Overrun

目标限制优惠次数为一次, 可在 `Repeater` 中使用 `Send group (parallel)` 突破限制

---

**References**

- [Race conditions](https://portswigger.net/web-security/race-conditions)
- [CWE-366](https://hackerone.com/hacktivity/cwe_discovery?id=cwe-366)