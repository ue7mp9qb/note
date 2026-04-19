使用 git 时出现的一些错误及其解决方法。

## 1. git 部署 ssh 公钥后无法使用 ssh 操作

在网络环境不佳时，我们往往会配置 ssh 用于连接 git 服务器

将公钥部署到 git 服务器后，使用 ssh 进行 git 操作会出现以下错误

![](./../../../../images/Issues_of_git/%E5%B0%86%E5%85%AC%E9%92%A5%E9%83%A8%E7%BD%B2%E5%88%B0%20git%20%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%90%8E%EF%BC%8C%E4%BD%BF%E7%94%A8%20ssh%20%E8%BF%9B%E8%A1%8C%20git%20%E6%93%8D%E4%BD%9C%E4%BC%9A%E5%87%BA%E7%8E%B0%E4%BB%A5%E4%B8%8B%E9%94%99%E8%AF%AF.png)

这是由于配置时使用的是 PowerShell 的 ssh 服务，而 git 时使用的是自带的 ssh 服务

使用以下命令将 git 的 ssh 服务修改为 PowerShell 的 ssh 服务即可

```
PS C:\Users\nemo> git config --global core.sshCommand "C:/Windows/System32/Openssh/ssh.exe"
```

## 2. git 操作时显示主机密钥验证失败

```
> git pull --tags origin master
CreateProcessW failed error:193
ssh_askpass: posix_spawnp: Unknown error
Host key verification failed.
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
```

可能是删除缓存造成的

```powershell
PS C:\Windows\system32> Remove-Item C:\Users\sec\.ssh\known_hosts
```

在客户端重新加载 github 的缓存即可

```powershell
PS C:\Users\sec> ssh -T git@github.com
The authenticity of host 'github.com (20.205.243.166)' can't be established.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
```

## 3. 配置代理后无法正常使用

```
remote: Enumerating objects: 15483, done.
error: RPC failed; curl 92 HTTP/2 stream 5 was not closed cleanly: CANCEL (err 8)
error: 7317 bytes of body are still expected
fetch-pack: unexpected disconnect while reading sideband packet
fatal: early EOF
fatal: fetch-pack: invalid index-pack output
```

原因是代理服务器配置了自签名证书, 导致无法通过 git 的验证;

将证书加入本地, 配置使用 Windows 证书校验

```
git config --global http.sslBackend schannel
```

或者强制 git 使用 HTTP/1.1 协议即可.

## 4.  最新配置的设备无法使用 Git 提交

使用命令行运行 `git clone <url>` ,将目标服务器添加到 `known hosts` 即可

---

**References**

- [Git](https://git-scm.com/) 
