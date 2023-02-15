---
title: 笔记本配置内网+外网双网卡教程
date: 2022-06-26 17:25:40.672
updated: 2022-06-26 17:26:21.661
url: /archives/bi-ji-ben-pei-zhi-nei-wang--wai-wang-shuang-wang-ka-jiao-cheng
categories: 
tags: 
---

# 重点先设置这个：
将上内网的网卡地址设为静态地址，不设网关，此时重启系统时就不会产生默认路由了

配置静态路由表：
```
route add -p 10.0.0.0（内网地址） MASK 255.255.0.0 172.16.0.1（网关）
```