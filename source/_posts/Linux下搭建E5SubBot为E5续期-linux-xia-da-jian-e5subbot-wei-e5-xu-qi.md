---
title: Linux下搭建E5SubBot为E5续期
date: 2022-05-12 16:13:54.628
updated: 2022-05-12 16:13:54.628
url: /archives/linux-xia-da-jian-e5subbot-wei-e5-xu-qi
categories: 
tags: 
---

####   这里以宝塔为基础
定位到准备存放数据的目录
例如：/www/wwwroot/e5/bot
选择远程下载
访问E5SubBot项目地址
E5SubBot项目Github地址：https://github.com/iyear/E5SubBot/releases

选择合适的版本
右键选择复制链接地址，粘贴到离线下载框内。
解压文件，设置   E5SubBot   权限为  755/root

####   配置E5Subbot

目录下创建一个config.yml文件，配置telegram bot和mysql的具体信息。
示例：
```
bot_token: 你的TG机器人的TOKEN
notice: "这里可以填写机器人的通知信息"
admin: 填写你tg的id，使其作为管理员
# socks5: 127.0.0.1:1080
bindmax: 3
goroutine: 10
errlimit: 999
  aaa
  bbb
  ccc
cron: "1 */1 * * *"
db: sqlite
table: users
# mysql:
#  host: 127.0.0.1
#  port: 3306
#  user: root
#  password: pwd
#  database: e5sub
sqlite:
  db: data.db
```
#### 设置自动启动

在/etc/systemd/system 新建 e5sub.service
填写以下配置（根据实际情况修改）：
```
[Unit]
Description=Telegram E5Sub Bot
 
[Service]
Type=simple
WorkingDirectory=/www/wwwroot/e5/bot
ExecStart=/www/wwwroot/e5/bot/E5SubBot
Restart=always
RestartSec=30
 
[Install]
WantedBy=multi-user.target
```

#### 设置开机自启
systemd单元文件编辑完成后需要重新载入服务配置文件。

```
systemctl daemon-reload
```
然后开启服务。

```
systemctl start e5sub
```
查看服务状态。

```
systemctl status e5sub
```
最后设置服务自启。


```
systemctl enable e5sub
```
