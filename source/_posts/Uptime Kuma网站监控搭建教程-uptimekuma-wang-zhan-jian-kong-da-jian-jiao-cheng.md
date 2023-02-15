---
title: Uptime Kuma网站监控搭建教程
date: 2022-04-15 15:13:06.119
updated: 2022-04-15 15:14:29.566
url: /archives/uptimekuma-wang-zhan-jian-kong-da-jian-jiao-cheng
categories: 
tags: 
---

这里介绍非docker搭建方法

1. 安装好Node.js >= 14, git
2. 安装pm2

```
npm install pm2 -g
```
3.按照官网提示操作
```
npm install npm -g
```
```
git clone https://github.com/louislam/uptime-kuma.git
```
```
cd uptime-kuma
```
```
npm run setup
```
```
node server/server.js
```
```
pm2 start server/server.js --name uptime-kuma
```
宝塔反代
编辑反代哪里配置
```
 location / {proxy_set_header X-Real-IP $remote_addr;proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;proxy_pass http://127.0.0.1:3001/;proxy_http_version 1.1;proxy_set_header Upgrade $http_upgrade;proxy_set_header Connection "upgrade";}
```