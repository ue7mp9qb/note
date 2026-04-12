MariaDB Server: the innovative open source database.

## 1. Usage

运行服务

```
┌──(nemo@debian)-[~]
└─$ sudo systemctl start mysql.service
```

登录

```
┌──(nemo@debian)-[~]
└─$ mysql -u root -p
```

修改密码

```
MariaDB [(none)]> ALTER USER 'root'@'localhost' IDENTIFIED BY '123456'; # 新版语法

MariaDB [(none)]> SET PASSWORD FOR 'root'@'localhost' = PASSWORD('123456'); # 旧版语法

MariaDB [(none)]> FLUSH PRIVILEGES; # 刷新
```

退出

```
MariaDB [(none)]> exit;
```

列出数据库

```
MariaDB [(none)]> show databases;
```

选择数据库

```
MariaDB [(none)]> use nemo;
```

列出数据表

```
MariaDB [root]> show tables;
```

### 1.1. SUBSTRING

`SUBSTRING` 函数用于提取字符串的子串。在 MySQL 中，`SUBSTRING` 函数的语法如下：

```
sqlCopy code
SUBSTRING(str, pos, len)
```

其中：

- `str` 是要提取子串的字符串。
- `pos` 是子串的起始位置，索引从1开始。
- `len` 是要提取的子串的长度。如果省略 `len` 参数，则将提取从 `pos` 开始到字符串末尾的所有字符。

 从用户名为 `admin` 的用户的密码字段中提取第一个字符的子串

```mysql
MariaDB [security]> SELECT SUBSTRING(password, 1, 1) FROM users WHERE username = 'admin';
```

```
+---------------------------+
| SUBSTRING(password, 1, 1) |
+---------------------------+
| a                         |
+---------------------------+
1 row in set (0.00 sec)
```

### 1.2. LIMIT

`LIMIT 1,2` 表示从第二行开始取二行相当于

```
MariaDB [security]> select * from users limit 1,2;
```

| id   | username | password     |
| ---- | -------- | ------------ |
| 2    | Angelina | I- kill- you |
| 3    | Dummy    | p@ssword     |

> 在 sql 中第一行为 `0` 

`LIMIT 3` 表示取前三行

```
MariaDB [security]> select * from users limit 3;
```

| id   | username | password     |
| ---- | -------- | ------------ |
| 1    | Dumb     | Dumb         |
| 2    | Angelina | I- kill- you |
| 3    | Dummy    | p@ssword     |

### 1.3. order by

根据 `id` 列升序列出 `users` 数据表

```
MariaDB [security]> select * from users order by id;
```

| id   | username | password     |
| ---- | -------- | ------------ |
| 1    | Dumb     | Dumb         |
| 2    | Angelina | I- kill- you |
| 3    | Dummy    | p@ssword     |

也可以通过字段的序号表示 `id`

```mysql
MariaDB [security]> select * from users order by 1;
```

根据 `id` 列降序列出 `users` 数据表

```mysql
MariaDB [security]> select * from users order by id desc;
```

| id   | username | password     |
| ---- | -------- | ------------ |
| 3    | Dummy    | p@ssword     |
| 2    | Angelina | I- kill- you |
| 1    | Dumb     | Dumb         |

> 若是字符列则通过字符的 `ascci` 码排序

---

References

- [MariaDB](https://mariadb.org/)

