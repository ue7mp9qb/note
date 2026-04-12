常用命令

```bat
rem 关闭命令行中的回显
@echo off

rem 切换到脚本所在的目录
cd /d %~dp0

rem 运行 program
./program.exe %*

rem 暂停脚本的执行
pause

```

---

References

- [BatchScript](https://learn.microsoft.com/zh-cn/azure/devops/pipelines/tasks/reference/batch-script-v1?view=azure-pipelines)
