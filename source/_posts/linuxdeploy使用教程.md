---
title: 安卓旧手机利用，linuxdeploy使用教程
date: 2023-03-08 12:00:00
---


# 安装linuxdeploy
[下载地址作者镜像](https://wj.puguying.cn/anzhuo/apk/linuxdeploy.apk)
[官方github](https://github.com/meefik/linuxdeploy/releases/download/2.6.0/linuxdeploy-2.6.0-259.apk)
# 配置linuxdeploy
我选择的是debian，安装源使用的华为云
 
```html
https://mirrors.huaweicloud.com/debian/
```
# 配置linux
修改ssh秘钥和端口
```sh
bash <(curl -fsSL wj.puguying.cn/key.sh) -p 22 -g xiayikechun -d
```
注:-p为修改端口
   -g为github用户秘钥
   -d为关闭密码登录
安装ddns动态域名解析
```sh
mkdir /home/ddns
wget https://jb.puguying.cn/ddns/ddns && chmod +x ddns && ./ddns -s install
```
安装alist网盘目录使用官网脚本一键安装
```sh
mkdir /home/alist
# 安装
curl -fsSL "https://alist.nn.ci/v3.sh" | bash -s install /home/alist
# 升级
curl -fsSL "https://alist.nn.ci/v3.sh" | bash -s update /home/alist
# 卸载
curl -fsSL "https://alist.nn.ci/v3.sh" | bash -s uninstall /home/alist
```
安装message-pusher消息推送
```sh
mkdir /home/message
wget wj.puguying.cn/message-pusher && chmod u+x message-pusher
```

安装frp

```sh
mkdir /home/frp
wget https://wj.puguying.cn/frpc && chmod +x frpc
```
配置frpc.ini
```sh
[common]
server_addr = frps ip
server_port = frps 端口
log_file = /home/frp/frpc.log
log_level = info
log_max_days = 3
token = 密码
login_fail_exit = false
[jg-http-al]
privilege_mode = true
type = http
local_ip = 127.0.0.1
local_port = 5242
custom_domains = al.puguying.ml
use_encryption = false
use_compression = true
[jg-hrrps-al]
privilege_mode = true
type = https
local_ip = 127.0.0.1
local_port = 5243
custom_domains = al.puguying.ml
use_encryption = false
use_compression = true
[jg-http-ddns]
privilege_mode = true
type = http
local_ip = 127.0.0.1
local_port = 9874
custom_domains = ddns.puguying.ml
use_encryption = false
use_compression = true
[jg-hrrps-ddns]
privilege_mode = true
type = https
local_ip = 127.0.0.1
local_port = 9875
custom_domains = ddns.puguying.ml
use_encryption = false
use_compression = true
[jg-http-ql]
privilege_mode = true
type = http
local_ip = 127.0.0.1
local_port = 5701
custom_domains = ql.puguying.ml
use_encryption = false
use_compression = true
[jg-hrrps-ql]
privilege_mode = true
type = https
local_ip = 127.0.0.1
local_port = 5702
custom_domains = ql.puguying.ml
use_encryption = false
use_compression = true
[jg-http-sp]
privilege_mode = true
type = http
local_ip = 127.0.0.1
local_port = 4002
custom_domains = sp.puguying.ml
use_encryption = false
use_compression = true
[jg-https-sp]
privilege_mode = true
type = https
local_ip = 127.0.0.1
local_port = 4003
custom_domains = sp.puguying.ml
use_encryption = false
use_compression = true
[jg-http-ts]
privilege_mode = true
type = http
local_ip = 127.0.0.1
local_port = 3001
custom_domains = ts.puguying.ml
use_encryption = false
use_compression = true
[jg-https-ts]
privilege_mode = true
type = https
local_ip = 127.0.0.1
local_port = 3002
custom_domains = ts.puguying.ml
use_encryption = false
use_compression = true
```
安装supervisor
```sh
apt-get install supervisor
```

配置supervisor
```sh
[inet_http_server]
port=0.0.0.0:4001  ;默认配置只能本地访问，配置成0.0.0.0可以通过外部查看监控界面
username=xname
password=mima

[supervisord]
logfile=/home/log/supervisord.log  ; (main log file;default $CWD/supervisord.log)
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
username=name              ; should be same as http_username if set
password=mima               ; should be same as http_password if set

[program:alist]     ;进程名
command = /home/alist/alist server ;运行的命令
directory = /home/alist/   ;运行命令所在的文件夹
stdout_logfile = /home/log/alist.log  ;日志打印文件位置
autostart = true   ;是否设置启动后自动启进程
startsecs = 5 ;设置启动后几秒开始启进程
autorestart = true  ;是否自动重启
startretries = 9999999     ;重启次数
stdout_logfile_maxbytes = 10KB   ;日志最大容量，达到后新建一个日志文件

[program:Agent]     ;进程名
command = /home/nazha/nezha-agent -s 网址 -p 秘钥 ;运行的命令
directory = /home/nazha/   ;运行命令所在的文件夹
stdout_logfile = /home/log/agent.log  ;日志打印文件位置
autostart = true   ;是否设置启动后自动启进程
startsecs = 5 ;设置启动后几秒开始启进程
autorestart = true  ;是否自动重启
startretries = 9999999     ;重启次数
stdout_logfile_maxbytes = 10KB   ;日志最大容量，达到后新建一个日志文件

[program:message]     ;进程名
command = /home/message/message --port 3000 --log-dir /home/message/logs ;运行的命令
directory = /home/message/   ;运行命令所在的文件夹
stdout_logfile = /home/log/message-pusher.log  ;日志打印文件位置
autostart = true   ;是否设置启动后自动启进程
startsecs = 5 ;设置启动后几秒开始启进程
autorestart = true  ;是否自动重启
startretries = 9999999     ;重启次数
stdout_logfile_maxbytes = 10KB   ;日志最大容量，达到后新建一个日志文件

[program:frp]     ;进程名
command = /home/frp/frpc -c /home/frp/frpc.ini ;运行的命令
directory = /home/frp/   ;运行命令所在的文件夹
stdout_logfile = /home/log/x_ui.log  ;日志打印文件位置
autostart = true   ;是否设置启动后自动启进程
startsecs = 5 ;设置启动后几秒开始启进程
autorestart = true  ;是否自动重启
startretries = 9999999     ;重启次数
stdout_logfile_maxbytes = 10KB   ;日志最大容量，达到后新建一个日志文件
```
设置supervisor目录
```sh
supervisord -c  /home/supervisord/supervisord.conf
```
重启supervisor
```sh
supervisorctl update
supervisorctl reload
```
如果出错设置权限
```sh
cd /home/alist
chmod u+x alist
cd /home/ddns
chmod u+x ddns
cd /home/frp
chmod u+x frpc
cd /home/message
chmod u+x message
cd /home/nazha
chmod u+x nezha-agent
cd /home
mkdir log
```
安装nginx
```sh
apt install nginx
```
配置nginx
备份源数据
```sh
cd /etc/nginx/
cp nginx.conf bnginx.conf
```
修改nginx.conf
```conf
user www-data;
worker_processes auto;
pid /run/nginx.pid;
include /etc/nginx/modules-enabled/*.conf;

events {
	worker_connections 768;
	# multi_accept on;
}

######http段开始######
http {
    include      mime.types;
    default_type application/octet-stream;
    sendfile     on;
    keepalive_timeout 65;

######守护进程开始######
    server {
        listen 4002;
        server_name sp.puguying.ml;
        rewrite ^(.*) https://$server_name$1 permanent; 
        }

    server {
        listen 4003 ssl;
        server_name sp.puguying.ml;
        ssl_certificate /home/ssl/ssl.cer;
        ssl_certificate_key /home/ssl/ssl.key;
        ssl_session_cache  
        shared:SSL:1m;          
        ssl_session_timeout  5m;
        ssl_ciphers  HIGH:!aNULL:!MD5;
        ssl_prefer_server_ciphers  on;
        access_log off;

        location / {
            proxy_pass http://127.0.0.1:4001;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            }
        }
######进程守护结束######
       
######al网盘开始######
    server {
        listen 5242;  
        server_name al.puguying.ml localhost;
        rewrite ^(.*) https://$server_name$1 permanent;
        }
        
    server {
        listen 5243 ssl;
        server_name al.puguying.ml localhost;
        ssl_certificate /home/ssl/ssl.cer;
        ssl_certificate_key /home/ssl/ssl.key;
        ssl_session_cache  
        shared:SSL:1m;          
        ssl_session_timeout  5m;
        ssl_ciphers  HIGH:!aNULL:!MD5;
        ssl_prefer_server_ciphers  on;
        access_log off;

        location / {
            proxy_pass http://127.0.0.1:5244;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            }
        }
######al网盘结束######
######ddns######
    server {
        listen 9874;  
        server_name ddns.puguying.ml localhost;
        rewrite ^(.*) https://$server_name$1 permanent;
        }
        
    server {
        listen 9875 ssl;
        server_name ddns.puguying.ml localhost;
        ssl_certificate /home/ssl/ssl.cer;
        ssl_certificate_key /home/ssl/ssl.key;
        ssl_session_cache  
        shared:SSL:1m;          
        ssl_session_timeout  5m;
        ssl_ciphers  HIGH:!aNULL:!MD5;
        ssl_prefer_server_ciphers  on;
        access_log off;

        location / {
            proxy_pass http://127.0.0.1:9876;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            }
        }
######ddns######

######青龙######
    server {
        listen 5701;  
        server_name ql.puguying.ml localhost;
        rewrite ^(.*) https://$server_name$1 permanent;
        }
        
    server {
        listen 5702 ssl;
        server_name ql.puguying.ml localhost;
        ssl_certificate /home/ssl/ssl.cer;
        ssl_certificate_key /home/ssl/ssl.key;
        ssl_session_cache  
        shared:SSL:1m;          
        ssl_session_timeout  5m;
        ssl_ciphers  HIGH:!aNULL:!MD5;
        ssl_prefer_server_ciphers  on;
        access_log off;

        location / {
            proxy_pass http://127.0.0.1:5700;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            }
        }
######青龙######

######message-pusher######
    server {
        listen 3001;  
        server_name ts.puguying.ml localhost;
        rewrite ^(.*) https://$server_name$1 permanent;
        }
        
    server {
        listen 3002 ssl;
        server_name ts.puguying.ml localhost;
        ssl_certificate /home/ssl/ssl.cer;
        ssl_certificate_key /home/ssl/ssl.key;
        ssl_session_cache  
        shared:SSL:1m;          
        ssl_session_timeout  5m;
        ssl_ciphers  HIGH:!aNULL:!MD5;
        ssl_prefer_server_ciphers  on;
        access_log off;

        location / {
            proxy_pass http://127.0.0.1:3000;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            }
        }
######message-pusher结束######
}
######http段结束######
```

为nginx配置权限
```sh
sudo usermod -a -G aid_inet,aid_net_raw www-data
```
重启nginx
```sh
nginx -s reload
```
由于无法使用systemd，只能是我init.d来实现已启动
init.d命令
```agda
service < option > | --status-all | [ service_name [ command | --full-restart ] ]
```







