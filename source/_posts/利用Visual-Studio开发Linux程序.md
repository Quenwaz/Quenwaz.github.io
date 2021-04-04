---
title: 利用Visual Studio开发Linux程序
date: 2019-01-06 10:38:59
categories: 编码
tags:
    - Visual Studio
---

# 目的
为了能在Windows平台进行Linux程序开发，本文着重讲解如何部署开发环境

### 环境说明
- visual studio 2017
- centos 7.0

# Visual Studio环境部署
[下载](https://visualstudio.microsoft.com/zh-hans/downloads/)visual studio 2017，按照步骤进行安装。**记得勾选【使用C++的Linux开发】**

# Linux环境部署
1、 首先确保你拥有Linux系统，本文基于CentOs系统
2、 安装Openssh
```
yum install openssh-server
```
3、 安装g++
```
yum install gcc-c++
```
4、 安装gdb+gdbserver
首先[下载]( http://ftp.gnu.org/gnu/gdb/)gdb-8.2.tar.gz，下载完成将压缩文件上传到Centos用户目录。Linux gdb-8.2.tar.gz所在目录<br/>
解压缩
```
tar  -zxvf  gdb-7.12.tar.gz
```

解压后会出现 gdb-8.2 文件目录，进入此目录，执行如下命令：
```
./configure
```
然后执行make命令：
```
make
```
然后进行安装：
```
make install
```
若安装过程中出现make : makeinfo : 命令未找到：
```
yum install texinfo
```
完成后，在gdb-8.2目录中找到gdb程序，复制到/usr/bin目录下:
```
cp gdb-8.2/gdb/gdb   /usr/bin
```
同时在目录中将生成的gdbserver程序也拷贝到/usr/bin目录下：
```
cp gdb-8.2/gdb/gdb/gdbserver  /usr/bin
```
5、完成








---
<small><font color= "gray">本文[Quenwaz](https://quenwaz.github.io)版权所有，转载请说明出处</font></small>