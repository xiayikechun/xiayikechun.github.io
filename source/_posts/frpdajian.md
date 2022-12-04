title : frp搭建教程


## 1.  下载frp程序
[Frp开源地址](https://github.com/fatedier/frp/releases)
注意选择对应的系统版本
## 2. 编辑服务端frps.ini文件（可以直接访问的）可以参考下边的配置
```
[common]
# 监听ipv4和ipv6
bind_addr = ::
# frp监听的端口，默认是7000，可以改成其他的
bind_port = 700
# 授权码，请改成更复杂的
token = 123456789
# 这个token之后在客户端会用到

# frp管理后台端口，请按自己需求更改
dashboard_port = 7500
# frp管理后台用户名和密码，请改成自己的
dashboard_user = admin
dashboard_pwd = admin
enable_prometheus = true

# 配置http和https服务要监听的端口，这两个端口也要在阿里云安全组上放开，且不能是服务器已经使用的端口
vhost_http_port = 80
vhost_https_port = 443
#默认404界面
custom_404_page = /frp/404.html

# frp日志配置
log_file = /frp/frps.log
log_level = info
log_max_days = 3
```
## 3.编辑frps开机自启这里使用systemd
在 /etc/systemd/system 目录下创建一个 frps.service 文件
内容参考下边的
```
[Unit]
Description=frp service
After=network.target

[Service]
TimeoutStartSec=30
#注意下边的路径换成你自己的
ExecStart=/frp/frps -c /frp/frps.ini
ExecStop=/bin/kill $MAINPID

[Install]
WantedBy=multi-user.target
```
-------

到这里服务端配置就完成了
## 4.配置客户端frpc.ini配置（需要穿透的服务器）
可以参考下边的配置
```
[common]
# frps服务器地址
server_addr = frp.puguying.cn
上边配置的端口
server_port = 7000
log_max_days = 3
上边配置的密码
token = 123456
login_fail_exit = false

[ssh-xxx]
type = tcp
local_ip = 127.0.0.1
local_port = 22
remote_port = 10086
```
## 5.配置客户端开机自启
在/etc/systemd/system 目录下创建一个 frpc.service 文件
参考配置
```
[Unit]
Description=frp service
After=network.target

[Service]
TimeoutStartSec=30
#修改自己的目录
ExecStart=/frp/frpc -c /frp/frpc.ini
ExecStop=/bin/kill $MAINPID

[Install]
WantedBy=multi-user.target
```
-------

到这里所有配置工作完成

## 6.使用 systemd 命令，管理 frps。
```

# 启动frp
服务端
systemctl start frps
客户端
systemctl start frpc
# 停止frp
服务端
systemctl stop frps
客户端
systemctl stop frpc
# 重启frp
服务端
systemctl restart frps
客户端
systemctl restart frpc
# 查看frp状态
服务端
systemctl status frps
客户端
systemctl status frpc


```
## 7.配置 frps 开机自启。

```
服务端
systemctl enable frps
客户端
systemctl enable frpc

```
