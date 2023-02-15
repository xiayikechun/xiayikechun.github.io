---
title: typecho去掉index.php
date: 2022-04-15 15:28:35.144
updated: 2022-04-15 15:28:35.144
url: /archives/typecho-qu-diao-indexphp
categories: 
tags: 
---

后台永久链接设置为启动，自定义/{cid}.html
宝塔网站配置文件增加
```
if (!-e $request_filename) {

  rewrite ^(.*)$ /index.php$1 last;

}
```