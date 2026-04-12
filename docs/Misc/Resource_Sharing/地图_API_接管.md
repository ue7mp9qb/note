地图 API 接管漏洞会导致该 Key 被出售给其它开发者或恶意消耗 API;

将会导致地图加载异常，显示报错信息, 无法获取使用者的地理定位等.

## 1. 高德

### 1.1. 特征

```
https://restapi.amap.com/v3/geocode/regeo?key=82f7dba16fb54f5999f41e8be703502b
```

```
https://webapi.amap.com/maps?v=1.4.15&key=156728ffb66357b1f257ec8000468f91
```

HUNTER

```
web.body="https://restapi.amap.com/v3/geocode/regeo?"
web.body="https://webapi.amap.com/maps?"
```

FOFA

```
body="https://restapi.amap.com/v3/geocode/regeo?"
body="https://webapi.amap.com/maps?"
```

QUAKE

```
body:"https://restapi.amap.com/v3/geocode/regeo?"
body:"https://webapi.amap.com/maps?"
```

ZoomEye

```
http.body="https://restapi.amap.com/v3/geocode/regeo?"
http.body="https://webapi.amap.com/maps?"
```

### 1.2. PoC

```
https://restapi.amap.com/v3/geocode/regeo?key=82f7dba16fb54f5999f41e8be703502b&s=rsv3&location=31.434446,121.90816&callback=jsonp_258885_&platform=JS
```

## 2. 百度

### 1.1. 特征

```
https://api.map.baidu.com/api?v=1.0&&type=webgl&ak=SA1vncDqp8vNAVIn1WUGIQkE7G5cDdbP
```

```
https://api.map.baidu.com/api?type=webgl&v=1.0&ak=tAQtwUHGUDdlDKBzXOb5bIGTRV7B0wv0
```

```
http://api.map.baidu.com/api?key=Pu6dflqnQUgUWFGUQ1dzkBcDkGFEbbcj&v=1.0&services=true
```

```
https://api.map.baidu.com/api?v=2.0&ak=3pmX1uL2kYs76UzGeUS5nB01NwourpyB
```

HUNTER

```
web.body="https://api.map.baidu.com/api?"
```

FOFA

```
body="https://api.map.baidu.com/api?"
```

QUAKE

```
body:"https://api.map.baidu.com/api?"
```

ZoomEye

```
http.body="https://api.map.baidu.com/api?"
```

### 1.2. PoC

```
https://api.map.baidu.com/place/v2/search?query=ATM&tag=银行&region=上海&output=json&ak=SA1vncDqp8vNAVIn1WUGIQkE7G5cDdbP
```

## 3. 腾讯

### 2.1. 特征

```
https://map.qq.com/api/gljs?v=1.exp&key=DKCBZ-SYCCU-OZ2VP-2ES3Z-ZQHQ6-6FFW6
```

```
https://map.qq.com/api/js?v=2.exp&key=BSXBZ-XFPRK-RMDJR-A6N36-E6C3O-NRBC6&libraries=convertor,place,geometry
```

```
https://map.qq.com/api/js?v=2.exp&key=OYDBZ-ROWCQ-EF35D-2Z6E3-WUXR6-RVFQ7
```

```
https://map.qq.com/api/js?v=2.exp&libraries=place&key=225d6c323c15ed3391a890f834bc4533
```

HUNTER

```
web.body="https://map.qq.com/api/"
```

FOFA

```
body="https://map.qq.com/api/"
```

QUAKE

```
body:"https://map.qq.com/api/"
```

ZoomEye

```
http.body="https://map.qq.com/api/"
```

### 2.2. PoC

```
https://apis.map.qq.com/ws/place/v1/search?keyword=银行&boundary=nearby(31.2304,121.4737,1000)&key=BSXBZ-XFPRK-RMDJR-A6N36-E6C3O-NRBC6
```

---

References

- [“地图API后台配置错误”:挖SRC的新玩具？](https://www.freebuf.com/articles/web/360331.html)

