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
新建
.github/workflows/deployment.yml
填入内容
```
name: Deployment

on:
  push:
    branches: [hexo] # only push events on source branch trigger deployment

jobs:
  hexo-deployment:
    runs-on: ubuntu-latest
    env:
      TZ: Asia/Shanghai

    steps:
    - name: Checkout source
      uses: actions/checkout@v2
      with:
        submodules: true

    - name: Setup Node.js
      uses: actions/setup-node@v1
      with:
        node-version: '12.x'

    - name: Install dependencies & Generate static files
      run: |
        node -v
        npm i -g hexo-cli
        npm i
        hexo clean
        hexo g
    - name: Deploy to Github Pages
      env:
        GIT_NAME: github用户名
        GIT_EMAIL: github邮箱
        REPO: github.com/github用户名/博客仓库名
        GH_TOKEN: ${{ secrets.HEXO_TOKEN }}     # can be created or changed in the Github/Repository/Setting/Secrets/Actions
      run: |
        cd ./public && echo 自定义域名 > CNAME && git init && git add .
        git config --global user.name $GIT_NAME
        git config --global user.email $GIT_EMAIL
        git commit -m "Site deployed by GitHub Actions"
        git push --force --quiet "https://$GH_TOKEN@$REPO" master:master
```
修改代码中的自定义域名,用户名,邮箱,博客仓库地址

4.创建github token
路径：github设置~开发者设置~个人访问令牌~令牌（经典）~生成新的令牌（经典）~权限选择repo
记得及时复制，只显示一次
5.设置GH_TOKEN
返回博客仓库~仓库设置~设置秘钥
6.快速写博客
```
https://github.com/xiayikechun/xiayikechun.github.io/new/hexo/source/_posts
```
修改成自己的仓库地址和分支名


如有不足请补充

