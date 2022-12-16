---
title: 安卓linuxdeploy使用DDNS
date: 2022-12-16 19:01:00
---
## 前言不知道为什么下载最新版的一直报错建议大家可以先使用3.1版安装下边
3.1版
[github 3.1链接](https://github.com/jeessy2/ddns-go/releases/download/v3.1.0/ddns-go_3.1.0_Linux_arm64.tar.gz)
[站长的镜像国内可下载](https://wj.puguying.cn/ddns-go_3.1.0_Linux_arm64.tar.gz)
```
wget https://github.com/jeessy2/ddns-go/releases/download/v3.1.0/ddns-go_3.1.0_Linux_arm64.tar.gz
# 如果下载失败换成站长的镜像
wget https://wj.puguying.cn/ddns-go_3.1.0_Linux_arm64.tar.gz
tar -zxvf ddns-go_3.1.0_Linux_arm64.tar.gz
sudo ./ddns-go -s install
```
访问  你的ip:9876  进行设置
接下来就可以愉快的玩耍啦
