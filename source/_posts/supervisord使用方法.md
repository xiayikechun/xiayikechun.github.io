---
title: 安卓linuxdeploy使用supervisor代替systemd
date: 2022-12-24 23:01:00
---
使用linuxdeploy搭建linux后，无法使用systemd，导致很多程序无法部署，研究后决定使用supervisor代替。
安装我用的是debian，可以直接命令安装
```
apt-get install supervisor
```
然后回自动配置好开机自启在init.d目录里边，
安装结束开始配置需要的程序
启动supervisord进程
```
supervisord -c  /etc/supervisor/supervisord.conf
```
修改自己的内容
```
[inet_http_server]
port=0.0.0.0:4001  ;默认配置只能本地访问，配置成0.0.0.0可以通过外部查看监控界面
username=     用户名
password=   密码

[supervisord]
logfile=/root/jianguo/log/supervisord.log  ; (main log file;default $CWD/supervisord.log)
logfile_maxbytes=10KB       ; (max main logfile bytes b4 rotation;default 50MB)
logfile_backups=10          ; (num of main logfile rotation backups;default 10)
loglevel=info               ; (log level;default info; others: debug,warn,trace)
pidfile=/run/supervisord.pid ; (supervisord pidfile;default supervisord.pid)
nodaemon=false              ; (start in foreground if true;default false)
minfds=1024                 ; (min. avail startup file descriptors;default 1024)
minprocs=200                ; (min. avail process descriptors;default 200)

[rpcinterface:supervisor]
supervisor.rpcinterface_factory = supervisor.rpcinterface:make_main_rpcinterface

[supervisorctl]
serverurl=unix:///run/supervisor/supervisor.sock ; use a unix:// URL  for a unix socket
serverurl=http://127.0.0.1:4001
username=用户名         ; should be same as http_username if set
password=密码          ; should be same as http_password if set

[program:frp]     ;进程名
command = /root/jianguo/frp/frpc -c /root/jianguo/frp/frpc.ini ;运行的命令
directory = /root/jianguo/frp/   ;运行命令所在的文件夹
stdout_logfile = /root/jianguo/log/x_ui.log  ;日志打印文件位置
autostart = true   ;是否设置启动后自动启进程
startsecs = 5 ;设置启动后几秒开始启进程
autorestart = true  ;是否自动重启
startretries = 9999999     ;重启次数
stdout_logfile_maxbytes = 10KB   ;日志最大容量，达到后新建一个日志文件
```
然后重启主进程supervisord:
```
supervisorctl reload
```


