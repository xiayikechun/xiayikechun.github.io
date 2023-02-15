---
title: Docker教程命令
date: 2022-05-04 13:12:19.237
updated: 2022-05-04 13:12:19.237
url: /archives/docker-jiao-cheng-ming-ling
categories: 
tags: 
---

### 安装Docker
```
curl -sSL https://get.docker.com/ | sh
```
### 启动Docker
```
systemctl start docker
```
### 开机启动
```
systemctl enable docker
```
### 删除所有镜像
```
docker rmi $(docker images -q)
```
### 停止所有容器
```
docker stop $(docker ps -a -q)
```
### 删除所有容器
```
docker rm $(docker ps -a -q)
```
### 启动所有容器
```
docker start $(docker ps -a -q)
```

