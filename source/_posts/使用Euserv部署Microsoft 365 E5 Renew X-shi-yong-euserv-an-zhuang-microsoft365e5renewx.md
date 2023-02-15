---
title: 使用Euserv部署Microsoft 365 E5 Renew X
date: 2022-04-23 17:51:15.068
updated: 2022-05-12 14:49:49.395
url: /archives/shi-yong-euserv-an-zhuang-microsoft365e5renewx
categories: 
tags: 
---

####    1、安装系统选择Debian10或者其他，本文以Debian10为例
####    2、安装WARP，是服务器可以访问ipv4
```
wget -N https://raw.githubusercontent.com/fscarmen/warp/main/menu.sh && bash menu.sh [option] [lisence]
```
####    3、安装宝塔方便管理文件
```
wget -O install.sh http://download.bt.cn/install/install-ubuntu_6.0.sh && bash install.sh 12f2c1d72
```
使用cloudflare使服务器可以用ipv4访问，修改端口为cloudflare支持的端口，可以参考：[CLOUDFLARE支持的端口](https://puguying.cn/archives/cloudflare-zhi-chi-de-duan-kou)
我这里以8080为参考
注意：如果套了cloudflare后访问还是提示错误521的话，那么就输入如下命令：
```
echo '::' > /www/server/panel/data/ipv6.pl && /etc/init.d/bt restart
```
如果访问还是520错误之类的，有可能没有开启防火墙端口，输入如下命令分别回车：
```
firewall-cmd --permanent --zone=public --add-port=8080/tcp
firewall-cmd --reload
```
####   4、安装 Asp.Net Core SDK 3.1 运行环境
这里的示例是Debian10 ，其它 OS 请参考在 [Linux 发行版上安装 .NET | Microsoft Docs](https://docs.microsoft.com/zh-cn/dotnet/core/install/linux)
```
wget https://packages.microsoft.com/config/debian/10/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
rm packages-microsoft-prod.deb
```
```
sudo apt-get update; \
  sudo apt-get install -y apt-transport-https && \
  sudo apt-get update && \
  sudo apt-get install -y dotnet-sdk-3.1
```
####   5、创建 SSL 证书
这里使用的是阿里云的免费证书，可以直接下载  PFX  类型证书

![image](/upload/2022/05/image.png)

选择IIS类型证书下载，里边包含PFX证书和密码。

####   6、下载部署文件

[发布地址](https://blog.csdn.net/qq_33212020/article/details/119747634)
[下载地址](https://sundayrx.lanzoui.com/aW09Lsss755)

####   7、上传文件到任意目录
这里以  /www/wwwroot/   目录为例
![image-1652336408618](/upload/2022/05/image-1652336408618.png)
![image-1652336431778](/upload/2022/05/image-1652336431778.png)


####   8、配置Config.xml
示例：

```
﻿<?xml version="1.0" encoding="utf-8" ?>
<Configuration>
	<!--站点服务器基本配置-->
	<Serivce>
		<!--服务访问端口-->
		<Port>1066</Port>
		<!--管理员密码(管理员登录路由/Admin/Login) 重要：首次启动前必须更改-->
		<LoginPassword>123456</LoginPassword>
		<!--是否启用内核多线程支持-->
		<CoreMultiThread>true</CoreMultiThread>
		<!--网站备案号（选填）-->
		<ICP></ICP>
		<!--备案管理查询机构跳转链接（选填）-->
		<ICPLink>https://beian.miit.gov.cn</ICPLink>
	</Serivce>
	<!--站点Kestrel服务器HTTPS配置 （只支持IIS证书类型 即PFX格式的证书）-->
	<HTTPS>
		<!--Kestrel是否启用HTTPS(SSL加密传输)-->
		<Enable>true</Enable>
		<!--SSL证书文件名 (需要将PFX格式的SSL证书放置于该配置文件的同级目录Deploy文件夹下) 如e5.sundayrx.net.pfx-->
		<!--不填则默认使用Dev localhost 本地证书-->
		<Certificate></Certificate>
		<!--SSL证书密钥(PFX证书的访问密钥)-->
		<Password></Password>
	</HTTPS>
	<!--共享站点配置,不共享可无视以下内容 (若要共享站点 请自备以下所需的配置信息 且配置中HTTPS必须启用)-->
	<ShareSite>
		<!--是否启用站点共享-->
		<Enable>true</Enable>
		<!--SMTP邮件发送支持-->
		<SMTP>
			<!--发件邮箱-->
			<Email></Email>
			<!--邮箱密钥-->
			<Password></Password>
			<!--SMTP服务器地址-->
			<Host></Host>
			<!--SMTP服务器端口-->
			<Port></Port>
			<!--SMTP服务器是否使用SSL传输-->
			<EnableSSL>true</EnableSSL>
		</SMTP>
		<!--第三方OAuth登录支持(至少启用以下一种OAuth否则其他用户无法注册)-->
		<OAuth>
			<!--微软登录授权-->
			<Microsoft>
				<!--是否启用该OAuth-->
				<Enable>true</Enable>
				<!--应用程序Id-->
				<ClientId></ClientId>
				<!--应用程序访问机密-->
				<ClientSecret></ClientSecret>
			</Microsoft>
			<!--GitHub登录授权-->
			<Github>
				<!--是否启用该OAuth-->
				<Enable>false</Enable>
				<!--应用程序Id-->
				<ClientId></ClientId>
				<!--应用程序访问机密-->
				<ClientSecret></ClientSecret>
			</Github>
		</OAuth>
		<!--站点系统设置-->
		<System>
			<!--站点启动后默认是否允许用户注册 建议为false-->
			<AllowRegister>false</AllowRegister>
			<!--站点启动后默认公告（换行符请使用 &#x000D;&#x000A; 进行换行）-->
			<Notice></Notice>
			<!--站点运营者-->
			<Master></Master>
			<!--站点运营者推广链接-->
			<MasterLink></MasterLink>
			<!--站点新用户默认配额数-->
			<DefaultQuota>1</DefaultQuota>
			<!--站点自动特赦时间间隔 （单位：天 至少30天）-->
			<AutoSpecialPardonInterval>30</AutoSpecialPardonInterval>
		</System>
	</ShareSite>
</Configuration>
```

####   9、配置 Nginx反代
示例：https://127.0.0.1:1066
![image-1652337960167](/upload/2022/05/image-1652337960167.png)

####   10、 制作守护程序

在/etc/systemd/system 文件夹新建 e5renewx.service  文件
复制以下内容到e5renewx.service(修改对应的内容)
```
[Unit]
Description="Microsoft E5 Renew API Web dotnet Service"
After=network.target
StartLimitIntervalSec=0

[Service]
Type=simple
GuessMainPID=true
Environment=DOTNET_CLI_HOME=/tmp
WorkingDirectory=/www/wwwroot/e5
StandardOutput=journal
StandardError=journal
ExecStart=/usr/bin/dotnet /www/wwwroot/e5/Microsoft365_E5_Renew_X.dll
Restart=always
RestartSec=1

[Install]
WantedBy=multi-user.target

```

```
#重新加载配置文件
systemctl daemon-reload
#开机自启动
systemctl enable e5renewx.service
#启动服务
systemctl start e5renewx.service
```