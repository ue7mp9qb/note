“冰蝎”动态二进制加密网站管理客户端

## 1. Step

[JDK 11.0.27](https://www.oracle.com/java/technologies/javase/jdk11-archive-downloads.html)

## 2. Init

```
C:\Users\nemo\Apps\Behinder_v4.1.t00ls\Behinder.vbs
```

```
Dim WshShell, javaPath, jarPath, command

' 指定 JDK 路径
javaPath = "C:\Program Files\Java\jdk-11\bin\javaw.exe"

' 指定 JAR 路径
jarPath = "C:\Users\nemo\Apps\Behinder_v4.1.t00ls\Behinder.jar"

' 构建启动命令
command = Chr(34) & javaPath & Chr(34) & " -jar " & Chr(34) & jarPath & Chr(34)

' 执行命令 (隐藏窗口)
Set WshShell = CreateObject("WScript.Shell")
WshShell.Run command, 1

```

---

References

- [Behinder](https://github.com/rebeyond/Behinder)