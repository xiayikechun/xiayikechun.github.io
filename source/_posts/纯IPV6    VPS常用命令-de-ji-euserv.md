---
title: 纯IPV6    VPS常用命令
date: 2022-04-15 15:18:00.716
updated: 2022-04-22 17:14:04.59
url: /archives/de-ji-euserv
categories: 
tags: 
---

#### 安装WARP
```
wget -N https://raw.githubusercontent.com/fscarmen/warp/main/menu.sh && bash menu.sh [option] [lisence]
```
#### 安装宝塔
##### Centos安装命令：
```
yum install -y wget && wget -O install.sh http://download.bt.cn/install/install_6.0.sh && sh install.sh 12f2c1d72
```
##### Ubuntu/Deepin安装命令：
```
wget -O install.sh http://download.bt.cn/install/install-ubuntu_6.0.sh && sudo bash install.sh 12f2c1d72
```
##### Debian安装命令：
```
wget -O install.sh http://download.bt.cn/install/install-ubuntu_6.0.sh && bash install.sh 12f2c1d72
```


注意：如果套了cloudflare后访问还是提示错误521的话，那么就输入如下命令：
```html
echo '::' > /www/server/panel/data/ipv6.pl && /etc/init.d/bt restart
```

如果访问还是520错误之类的，有可能没有开启防火墙端口，输入如下命令分别回车：
```html
firewall-cmd --permanent --zone=public --add-port=8080/tcp
firewall-cmd --reload
```