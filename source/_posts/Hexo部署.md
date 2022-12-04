---
title: hexo搭建以及使用Github Actions自动部署
date: 2022-12-04 19:01:00
---
# 安装HEXO依赖程序
1. 安装git
termux安装命令
```
pkg install git
```
Linux (Ubuntu, Debian)：
```
sudo apt-get install git-core
```
Linux (Fedora, Red Hat, CentOS)：
```
sudo yum install git-core
```
2.安装node
termux安装命令
```
pkg install nodejs-lts
```
linux安装方法参考github
[链接地址](https://github.com/xiayikechun/node-)
# 安装HEXO
```
npm install -g hexo-cli
hexo init hexo
cd hexo
npm install
```
-------
# 接下来开始设置自动部署
1.新建github仓库
2.在hexo目录执行下边的命令
```
git init   // 1. 初始化项目文件夹
 
git add .  // 2. 将所有文件添加到暂存区
 
git commit -m "first commit"   // 3. 提交到本地仓库，双引号内是提交的备注信息
 
git remote add origin XXX     //  4. （XXX就是你github或者码云等远程仓库的地址，git branch这个命令可以看到你所在的分支，删除某个仓库地址使用git remote rm origin）
 
git pull    // 5. 拉取远程主分支信息，首次拉取合并信息
 
git push -u -f origin hexo  // 6. 提交到远程仓库，这个命令中的 -f 是强制推送，因为远程仓库只有初始化的文件，所以强制推送上去就行了，不加-f 会报当前分支没有远程分支，强制推送可以覆盖master，这样就完成了第一次提交的步骤)
```
3.代码推送完成以后
到github仓库新建

未完待续
