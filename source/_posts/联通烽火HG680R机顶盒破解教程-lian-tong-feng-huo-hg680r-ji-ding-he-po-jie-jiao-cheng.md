---
title: 联通烽火HG680R机顶盒破解教程
date: 2022-04-15 15:27:55.032
updated: 2022-04-15 15:27:55.032
url: /archives/lian-tong-feng-huo-hg680r-ji-ding-he-po-jie-jiao-cheng
categories: 
tags: 
---

#### 1. 首先在淘宝买了一根ttl线，5快包邮的
#### 2. 卖家提供的驱动安装好
#### 3. 首先把TTL不接任何东西插到电脑上面需要注意如下设置：TTL设置电平3.3V，计算机（我的电脑）->管理->设备管理器->CH340->波特率选择115200点确定
#### 4. 下载Putty
 [下载地址](http://down.tvapk.com//data/1606/putty.zip)

#### 5.打开putty
设置com口与波特率，然后点击打开，接下来打开盒子的电源，putty就开始接收到数据了
#### 6.输入
 

```
df
cd /mnt/usb/sda1
ls
pm install 软件包名

```

Success （提示这个说明安装成功）

最后
 

```apacheconf
am start com.dangbei.tvlauncher
```
就进去当贝桌面了



接下来说一下问题
1，网上有的视频说在机顶盒开机的时候需要按回车！但是我实际操作下来时不需要的，按回车反而会报错！
2，不用去管他自己跑的那些代码，等代码跑的慢一点或者机器已经开机后，敲一下回车，只要出现****@****：#这个样子的时候就可以属于上面的代码就行了


有朋友遇到问题的可以留言哈！

