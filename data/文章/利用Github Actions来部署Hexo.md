---
title: 利用Github Actions来部署Hexo
date: 2024-01-23 12:24:42
categories: 
tags: []
---
Hexo在本地运行时，每次都需要运行一套命令(hexo cl,hexo g,hexo d,hexo s),而且上传时长抽风(长期大量上传文件)，本地基础文件又容易丢失。所以，我用github Actions来修补以上问题。  
最开始很多关于github actions的文章大多没啥用，运行也会报错什么的。这里使用https://cloud.tencent.com/developer/article/2369534的教程。  
我简单来描述教程，尽量多写代码，少文字。因为文字实在是太多，有的文章的看着很烦。 
首先你需要准备:  
1.新github闭源仓库一个(没有hexo d到库过的得还要个开源仓库)  
2.(可有可无)Github desktop软件一个+github加速器(或改host host文件下载链接:https://raw.hellogithub.com/hosts)  
3.配置好的hexo源文件一份  
4.github tokens一份(新版github已改为Github => Settings => Developer settings => Tokens (classic) => Generate new token => Generate new token (classic)  

上述准备后，把下面的这个代码放在_config.yml就行了.
```
deploy:
  - type: git
    repository:
      github: https://#你的密钥#@github.com/nuoyis/blog.git,master
```
然后在.github里创建workflows,再创建个action.yml(.yml前面随便你起个名)(文本创建全选我这个名字再粘贴进去,没看到.txt需要网上搜索展开扩展名)  
然后打开，把下面放入这个yml文件中  
修改如下两行  
          git config --global user.name "doubleam"#github名字  
          git config --global user.email "admin@biugle.cn"#github邮箱
```
name: DoubleAm's Blog CI/CD # 脚本 workflow 名称

on:
  push:
    branches: [main, master] # 当监测 main,master 的 push
    paths: # 监测所有 source 目录下的文件变动，所有 yml,json 后缀文件的变动。
      - '*.json'
      - '**.yml'
      - '**/source/**'

jobs:
  blog: # 任务名称
    timeout-minutes: 30 # 设置 30 分钟超时
    runs-on: ubuntu-latest # 指定最新 ubuntu 系统
    steps:
      - uses: actions/checkout@v2 # 拉取仓库代码
      - uses: actions/setup-node@v2 # 设置 node.js 环境
      - name: Cache node_modules # 缓存 node_modules，提高编译速度，毕竟每月只有 2000 分钟。
        uses: actions/cache@v2 # 亲测 Github 服务器编译速度比我自己电脑都快，如果每次构建按5分钟计算，我们每个月可以免费部署 400 次，Github yyds！！！
        env:
          cache-name: cache-node-modules
        with:
          path: ~/.npm
          key: ${{ runner.os }}-build-${{ env.cache-name }}-${{ hashFiles('**/package-lock.json') }}
          restore-keys: |
            ${{ runner.os }}-build-${{ env.cache-name }}-
            ${{ runner.os }}-build-
            ${{ runner.os }}-
      - name: Init Node.js # 安装源代码所需插件
        run: |
          npm install
          echo "init node successful"
      - name: Install Hexo-cli # 安装 Hexo
        run: |
          npm install -g hexo-cli --save
          echo "install hexo successful"
      - name: Build Blog # 编译创建静态博客文件
        run: |
          hexo clean
          hexo g
          echo "build blog successful"
      - name: Deploy DoubleAm's Blog # 设置 git 信息并推送静态博客文件
        run: |
          git config --global user.name "doubleam"
          git config --global user.email "admin@biugle.cn"
          hexo deploy

      - run: echo "Deploy Successful!"
```
再用github desktop上传或者其他方式上传，action直接就开始运行了。全绿就配置正常