---
title: linuxdeploy开机自启
date: 2022-12-16 21:01:00
---
使用magisk

linux开机自启

在目录/data/adb/post-fs-data.d/
填入
```
sleep 100
nohup /data/user/0/ru.meefik.linuxdeploy/files/bin/linuxdeploy -p debian start -m >/dev/null 2>&1 &
```
上边的debian是你的容器名称
