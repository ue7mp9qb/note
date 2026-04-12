Get Shell 后

先升级为半交互 Shell

```shell
root@debian:~# python -c 'import pty;pty.spawn("/bin/bash")'
```

再升级为全交互 Shell

```shell
root@debian:~# stty raw -echo;fg;reset
```

