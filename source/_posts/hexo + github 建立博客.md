---
title: hexo + github 建立个人博客
date: 2018-03-13 23:30:56
tags: hexo github
---

### 一、为什么建立博客

我自己不擅长表达，写作能力也一般。一直有心锻炼自己，但是众所周知的懒癌，让这个计划搁置了n年之久，
刚好，最近工作业务上，不是特别忙，于是乎，列出了一个任务清单。建立本博客是第一个任务。

任务第一步完成，在此记录。。。

<!-- more -->

### 二、hexo + github 新建个人博客步骤

ps: Hexo是一个基于Node.js的快速简单的静态博客框架，利用它通过简单的几个命令就可以搭建一个个人博客。

```
安装Hexo
npm install hexo -g

初始化博客
hexo init blog

blog源代码部署到GitHub
1、新建 github 仓库
2、blog 项目根目录 _congig.yml 文件 设置代码托管仓库,下面是我的。

# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repository: https://github.com/WangPeng2017/Blog.git
  branch: gh-pages

3、git push ---> 源代码托管在master

GitHub 管理博客的逻辑是，master 分支托管源代码，gh-pages 分支托管 hexo g 生成的博客代码。
每次新写博客，首先push到master分支，然后 hexo g 、hexo d --- 生成、部署博客

生成博客
hexo generate （hexo g）

本地部署
hexo server (hexo s)

博客文件部署到GitHub --> gh-pages
hexo d

```

### 三、添加新主题 [官方主题](https://hexo.io/themes/)

首先切换到博客根目录下，使用如下命令安装landscap-plus:
```
git clone https://github.com/xiangming/landscape-plus.git themes/landscape-plus

```

* 然后修改根目录下的配置文件_config.yml, 把theme选项的值设置为：landscape-plus。

* 配置 <b>主题目录(themes/even)<b>下的配置文件_config.yml，把menu菜单项中的各选项配置为自己喜欢的样式，比如把英文的菜单改为中文的

```
menu:
  首页: /
  文章列表: /archives
  关于: /about

```





