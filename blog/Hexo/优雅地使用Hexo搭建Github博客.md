---
title: 优雅地使用Hexo搭建Github Blog
date: 2018-01-15 14:59:30
categories: 编码
tags: HEXO
---
### 前提条件
在github上新建一个仓库
{% asset_img new_repository.jpg New Repository %}
### 安装Hexo
#### 依赖环境
在安装Hexo之前，需要首先安装以下依赖，其中Node.js中已经包含npm：
- Git
- Npm
- Node.js (LTS)

#### 开始安装
若已经安装好git，且利用git安装好npm。即可进行hexo的安装。在Git Bash中输入此命令行：

    npm install -g hexo-cli

### 环境部署
#### 初始化博客

    # 新建博客文件夹
    mkdir Blog

    # 初始化博客文件夹
    hexo init Blog

    # 进入博客文件夹
    cd Blog

    # node.js 命令安装依赖dependencies
    npm install

    # 输入hexo g,首次体验Hexo
    hexo g

{% asset_img build_blog.jpg Build Blog %}

    # 开启服务器,开始体验
    hexo s

#### 生成博客

    # 新建博客
    hexo new post "Hello, Hexo"

{% asset_img new_blog.jpg New Blog %}

    # 将md生产html之前需要安装一个扩展
    npm install hexo-deployer-git --save

在部署之前，需要设置与github关联，在文件**_config.yml**中进行配置
{% asset_img config_github.jpg config github %}

    # 接下来利用hexo g生成，利用hexo d部署到github上。结合起来：
    hexo d -g

部署成功后即可访问地址：http://username.github.io。访问博客了


---
<small><font color= "gray">本文[Quenwaz](http://quenwaz.github.io)版权所有，转载请说明出处</font></small>
