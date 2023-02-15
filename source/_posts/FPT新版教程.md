 
---
title: FRP搭建教程半自动
date: 2023-02-01 18:25:30
---

# 自用脚本备份
1.来自[stilleshan](https://github.com/stilleshan)大佬的FRP脚本
FRPS
 Linux 服务器 一键脚本安装
> *本脚本目前同时支持 Linux X86 和 ARM 架构*

安装
```shell
wget https://raw.githubusercontent.com/xiayikechun/jiaoben/frp/azfrpc.sh && chmod +x azfrpc.sh && ./azfrpc.sh
```
以下为国内镜像
```shell
wget https://jb.puguying.cn/frp/azfrpc.sh && chmod +x azfrpc.sh && ./azfrpc.sh
```

使用
```shell
vi /usr/local/frp/frpc.ini
# 修改 frpc.ini 配置
sudo systemctl restart frpc
# 重启 frpc 服务即可生效
```

卸载
```shell
wget https://raw.githubusercontent.com/xiayikechun/jiaoben/frp/xzfrpc.sh && chmod +x xzfrpc.sh && ./xzfrpc.sh
```
以下为国内镜像
```shell
wget https://jb.puguying.cn/frp/xzfrpc.sh && chmod +x xzfrpc.sh && ./xzfrpc.sh
```
FRPS
安装
```shell
wget https://raw.githubusercontent.com/xiayikechun/frp/azfrps.sh && chmod +x azfrps.sh && ./azfrps.sh
```
以下为国内镜像
```shell
wget https://jb.puguying.cn/frp/azfrps.sh && chmod +x azfrps.sh && ./azfrps.sh
```

使用
```shell
vi /usr/local/frp/frps.ini
# 修改 frps.ini 配置
sudo systemctl restart frps
# 重启 frps 服务即可生效
```

卸载

```shell
wget https://raw.githubusercontent.com/xiayikechun/frp/xzfrps.sh && chmod +x xzfrps.sh && ./xzfrps.sh
```
以下为国内镜像
```shell
wget https://jb.puguying.cn/frp/xzfrps.sh && chmod +x xzfrps.sh && ./xzfrps.sh
```

### frps相关命令
```shell
sudo systemctl start frps
# 启动服务 
sudo systemctl enable frps
# 开机自启
sudo systemctl status frps
# 状态查询
sudo systemctl restart frps
# 重启服务
sudo systemctl stop frps
# 停止服务
```
修改frpc.ini和frps.ini的方法参考
```
rm -rf /usr/local/frp/frpc.ini
cat >/usr/local/frp/frpc.ini <<EOF
[common]
server_addr = frps地址
server_port = frps端口
log_file = /usr/local/frp/al.log ＃日志文件目录
log_level = info
log_max_days = 3
token = 密码
login_fail_exit = false

[web-ssh] #自定义，不能重复
type = tcp
local_ip = 127.0.0.1
local_port = 22
remote_port = 22
EOF

systemctl restart frpc

```
如有不足可以留言哈