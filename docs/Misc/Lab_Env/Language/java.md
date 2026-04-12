一种面向跨平台的编程语言.

## 1. Step

### 1.1. Debian

导入公钥

```
┌──(nemo@debian)-[~]
└─$ sudo mkdir -p /etc/apt/keyrings \
&& curl -fsSL https://packages.adoptium.net/artifactory/api/gpg/key/public | sudo tee /etc/apt/keyrings/adoptium.asc
```

添加 Adoptium 仓库

```
┌──(nemo@debian)-[~]
└─$ echo "deb [signed-by=/etc/apt/keyrings/adoptium.asc arch=amd64] https://packages.adoptium.net/artifactory/deb $(lsb_release -c -s) main" | sudo tee /etc/apt/sources.list.d/adoptium.list
```

获取更新

```
┌──(nemo@debian)-[~]
└─$ sudo apt update
```

## 2. Install

### 2.1. Debian

安装 JRE 8

```
┌──(nemo@debian)-[~]
└─$ sudo apt install -y temurin-8-jre
```

## 3. Usage

### 3.1. Debian

修改 JDK 版本

```
┌──(nemo@debian)-[~]
└─$ sudo update-alternatives --config java
```

```shell
There are 2 choices for the alternative java (providing /usr/bin/java).

  Selection    Path                                        Priority   Status
------------------------------------------------------------
* 0            /usr/lib/jvm/temurin-11-jdk-amd64/bin/java   1111      auto mode
  1            /usr/lib/jvm/temurin-11-jdk-amd64/bin/java   1111      manual mode
  2            /usr/lib/jvm/temurin-8-jdk-amd64/bin/java    1081      manual mode

Press <enter> to keep the current choice[*], or type selection number: 2
```

查看版本

```
┌──(nemo@debian)-[~]
└─$ java -version
```

运行 JAR

```
┌──(nemo@debian)-[~]
└─$ java -jar ./app.jar
```

### 3.2. Windows

指定 JDK 版本运行 JAR

```
app.vbs
```

```
Dim WshShell, javaPath, jarPath, command

' 指定 JDK 路径
javaPath = "C:\Program Files\Java\jdk-num\bin\javaw.exe"

' 指定 JAR 路径
jarPath = "C:\path\app.jar"

' 构建启动命令
command = Chr(34) & javaPath & Chr(34) & " -jar " & Chr(34) & jarPath & Chr(34)

' 执行命令 (隐藏窗口)
Set WshShell = CreateObject("WScript.Shell")
WshShell.Run command, 1

```

---

References

- [Adoptium Java](https://adoptium.net/temurin/releases)
- [Oracle Java](https://www.oracle.com/java/technologies/java-se-glance.html)

