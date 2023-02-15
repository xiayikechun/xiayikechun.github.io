---
title: liunx常用命令
date: 2022-04-15 15:10:44.255
updated: 2022-04-15 15:10:44.255
url: /archives/liunx-chang-yong-ming-ling
categories: 
tags: 
---

# liunx修改服务器时区
首先查看服务器时区
```
date
```
如果不是中国时区输入命令
```
tzselect
```

选择  4Asia  亚洲  在选择 9China  然后选择北京回车确认
复制文件到/etc目录下
```
 cp /usr/share/zoneinfo/Asia/Shanghai  /etc/localtime
```

在次查看时间
```
date
```



# 修改root密码

```
passwd
```
