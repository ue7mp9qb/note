常用命令

```shell
#!/bin/bash

# 后台运行
nohup

# 错误检测
set -e

# 开启调试模式
set -x

# 打印脚本所在的目录
echo "Script directory: $(dirname "$0")"

# 切换到脚本所在的目录
cd "$(dirname "$0")"

# 获取链接的实际路径并切换到该目录
cd "$(dirname "$(readlink -f "$0")")"

# 切换回之前所在的目录
cd $(pwd)

# 开启虚拟环境
source /path/venv/bin/activat

# 退出虚拟环境
deactivate

# 运行 program
./program "$@"

```

---

References

- [Bash](https://www.gnu.org/software/bash/)

