---
title: 服务器开启秘钥登录
date: 2022-04-15 15:31:12.225
updated: 2022-04-18 18:22:57.548
url: /archives/fu-wu-qi-kai-qi-mi-yao-deng-lu
categories: 
tags: 
---

### 1：登录服务器：并执行以下命令生成密钥和公钥
```
ssh-keygen -m PEM -t rsa -C "xiayikechun"
```
### 2：配置 ssh 使用密钥

#### 进入 ssh 目录
 ``` 
cd ~/.ssh 或者 cd /root/.ssh/
  ```
#### 然后安装公钥 authorized_keys
  ```
cp id_rsa.pub authorized_keys
 ``` 
#### 注意，如果存在 authorized_keys 则采用写入方式
 ``` 
cat id_rsa.pub >> authorized_keys
 ```
#### 设置公钥权限

```
chmod 600 authorized_keys
chmod 700 ~/.ssh
```

### 3：修改 ssh 配置文件
```
#打开修改
vi  /etc/ssh/sshd_config
 
然后对应如下修改：
 
StrictModes no  #此项默认为注释关闭
 
PubkeyAuthentication yes
RSAAuthentication yes    #默认不存在，可放到上面一行的下边
AuthorizedKeysFile      .ssh/authorized_keys      #ssh文件位置，此项默认设置相同
PasswordAuthentication yes                        #使用密码  no为不使用密码
AuthenticationMethods publickey,password          #如果密码和密钥都使用在末尾加上此行代码
```
### 完成以上步骤设置后，重启 sshd 服务：
```
systemctl restart sshd.service
```