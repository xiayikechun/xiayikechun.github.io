---
title: 甲骨文DD系统
date: 2022-04-15 15:30:40.894
updated: 2022-04-15 15:30:40.894
url: /archives/jia-gu-wen-dd-xi-tong
categories: 
tags: 
---

安装AMD服务器选择ubuntu20.04系统，不要选择mino的
安装的时候记得下载秘钥
登录名
```al
ubuntu
```
登录上去切换到root用户
```al
sudo -i
```
执行脚本
```al
echo root:RUYO |sudo chpasswd root
sudo sed -i 's/^#\?PermitRootLogin.*/PermitRootLogin yes/g' /etc/ssh/sshd_config;
sudo sed -i 's/^#\?PasswordAuthentication.*/PasswordAuthentication yes/g' /etc/ssh/sshd_config;
sudo service sshd restart
```
以上完成以后开始DD
```al
#更新apt源
apt-get update
#安装需要的工具包
apt-get install -y xz-utils openssl gawk file
```
debian10
```al
bash <(wget --no-check-certificate -qO- 'https://moeclub.org/attachment/LinuxShell/InstallNET.sh') -d 10.3 -v 64 -a -firmware
```

DD安装完毕之后，请立即更新密码。默认用户名为：root，默认密码为：MoeClub.org