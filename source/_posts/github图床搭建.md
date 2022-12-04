---
title: github图床搭建
date: 2022-12-04 19:25:30
---
这里是用的PicX 图床
我用的是宝塔搭建的
1.创建网站
2.进去网站目录
执行下边命令拉库
```
git clone -b gh-pages https://github.com/XPoet/picx.git
```
3.回到网站设置，设置网站目录为picx目录
4.设置定时任务
```
#!/bin/sh
cd /www/wwwroot/网站目录
git pull
```

其余的可以参考官网设置
[官方文档](https://picx-docs.xpoet.cn)

测试图床
微信收款码
![](https://cdn.staticaly.com/gh/xiayikeqiu/tuchuang@master/20221204/微信收款码.webp)
支付宝收款码
![](https://cdn.staticaly.com/gh/xiayikeqiu/tuchuang@master/20221204/支付宝收款码.webp)

