---
title: 安装python教程
date: 2023-01-01 18:25:30
---
安装python3
更新源
```
sudo apt update && sudo apt upgrade
```
安装依赖
```
sudo apt install wget build-essential libncursesw5-dev libssl-dev libsqlite3-dev tk-dev libgdbm-dev libc6-dev libbz2-dev libffi-dev zlib1g-dev
```
下载源码
```
cd ~
wget https://www.python.org/ftp/python/3.10.0/Python-3.10.0.tgz
```

解压源码
```
tar xzf Python-3.10.0.tgz
```

编译源码
```
cd Python-3.10.0
./configure --enable-optimizations
```
安装
```
make altinstall
```
软连接
```
sudo ln -s /usr/local/bin/python3.10 /usr/bin/python3
sudo ln -s /usr/local/bin/pip3.10 /usr/bin/pip3

```


