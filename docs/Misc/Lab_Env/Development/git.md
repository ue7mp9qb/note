Git is a free and open source distributed version control system designed to handle everything from small to very large projects with speed and efficiency.

```
工作区 add-> 暂存区 commit-> 本地仓库 <-pull||push-> 远程仓库
```

> HEAD 指的是最新一次的提交

## 1. Install

**Windows**

添加到环境变量

![添加到环境变量](./../../../../images/git/%E6%B7%BB%E5%8A%A0%E5%88%B0%E7%8E%AF%E5%A2%83%E5%8F%98%E9%87%8F.png)

使用系统 OpenSSH

![使用系统 OpenSSH](./../../../../images/git/%E4%BD%BF%E7%94%A8%E7%B3%BB%E7%BB%9F%20OpenSSH.png)

默认使用命令行启动

![默认使用命令行启动](./../../../../images/git/%E9%BB%98%E8%AE%A4%E4%BD%BF%E7%94%A8%E5%91%BD%E4%BB%A4%E8%A1%8C%E5%90%AF%E5%8A%A8.png)

## 2. Init

在 Github 开启私人电子邮箱

![在 Github 开启私人电子邮箱](./../../../../images/git/%E5%9C%A8%20Github%20%E5%BC%80%E5%90%AF%E7%A7%81%E4%BA%BA%E7%94%B5%E5%AD%90%E9%82%AE%E7%AE%B1.png)

选择凭证管理器

![选择凭证管理器](./../../../../images/git/%E9%80%89%E6%8B%A9%E5%87%AD%E8%AF%81%E7%AE%A1%E7%90%86%E5%99%A8.png)

配置代理

```
PS C:\Users\null> git config --global http.proxy "socks5://127.0.0.1:10808"
```

```
PS C:\Users\null> git config --global https.proxy "socks5://127.0.0.1:10808"
```

```
┌──(root㉿kali)-[~]
└─# git config --global http.proxy "socks5://10.0.2.2:10808"
```

```
┌──(root㉿kali)-[~]
└─# git config --global https.proxy "socks5://10.0.2.2:10808"
```

配置用户名

```
PS C:\Users\null> git config --global user.name "null"
```

配置邮箱

```
PS C:\Users\null> git config --global user.email "<private-name>@users.noreply.github.com"
```

部署公钥到 Git 服务器

![部署公钥到 Git 服务器](./../../../../images/git/%E9%83%A8%E7%BD%B2%E5%85%AC%E9%92%A5%E5%88%B0%20Git%20%E6%9C%8D%E5%8A%A1%E5%99%A8.png)

## 3. Usage

### 3.1. 基础

克隆仓库

```
git clone <repository-url>           # 完整克隆
git clone --depth 1 <repository-url> # 只克隆最新的一次提交
```

添加到暂存区

```
git add .      # 全部文件
git add <file> # 指定文件
```

提交到本地仓库

```
git commit -m "comment"
```

与远程仓库同步

```
git push # 从本地推送到远程
git pull # 从远程拉取到本地
```

### 3.2. 进阶

查看提交日志

```
git log           # Commit ID + Author + Date + Comment
git log --stat    # Commit ID + Author + Date + Comment + Changes
git log --oneline # Commit ID + Comment
```

查看提交内容

```
git show HEAD        # 查看最新提交的内容
git show <commit-id> # 查看指定提交的内容
git show <branch>    # 查看指定分支最新提交的内容
```

查看更改或比较

```
git diff          # 查看工作区已更改但未添加到暂存区的文件
git diff HEAD     # 查看工作区已更改但未提交到本地仓库的文件
git diff --staged # 查看已添加到暂存区但未提交到本地仓库的文件
git diff <commit-id-1> <commit-id-2> # 比较两个提交之间的差异
git diff <branch-1> <branch-2>       # 比较两个分支之间的差异
```

撤回

```
git revert <commit-id>      # 撤回某次提交并提交最新状态到本地仓库
git revert -m 1 <commit-id> # 撤回指定分支的某次提交并提交最新状态到本地仓库
```

回退

```
git reset HEAD^       # 回退到最新提交前的第一个版本
git reset HEAD~1      # 回退到最新提交前的第一个版本
git reset HEAD~3      # 回退到最新提交前的第三个版本
git reset HEAD~1 File # 指定文件回退到最新提交前的第一个版本
git reset --soft <commit-id>  # 回退到已添加到暂存区但未提交到本地仓库时
git reset --mixed <commit-id> # 回退到工作区已改动但未添加到暂存区时 (默认)
git reset --hard <commit-id>  # 回退到工作区未改动时
```

切换提交节点但不重置

```
git checkout <commit-id>
```

### 3.3. 发布

在 GitHub 创建一个仓库, 克隆仓库到本地即可

```
git clone <repository-url>
```

### 3.4. 配置

列出全局配置

```
git config --global --list
```

镜像加速

```
git config --global --get-regexp "url\..*\.insteadOf" # 查看镜像加速
git config --global url."https://ghp.ci/https://github.com/".insteadOf https://github.com/                                   # 配置镜像加速
git config --global --unset url."https://ghp.ci/https://github.com/.insteadOf" # 移除镜像加速
```

代理

```
git config --global http.proxy                          # 查看代理
git config --global http.proxy "http://127.0.0.1:10808" # 配置代理
git config --global --unset http.proxy                  # 移除代理
```

---

**References**

- [git](https://git-scm.com/)

