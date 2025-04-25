---
date: '2025-04-20T19:00:00+08:00'
draft: false
title: linux常见操作
slug: linux-operation
---
### 概述
linux是一个生产环境常用的系统，和windows,macos都是一个档次的，它一般在服务器环境中使用，我们学习如何使用linux，了解如何在上面部署服务，是一件很有用，很快乐的事情。
### 安装java
java有两种，一个是甲骨文的，一个是openjdk的，前者不能免费使用，后者可以。我们在学习过程中推荐使用后者。openjdk在本篇文章编辑时已经更新到了24了，我们最好紧跟更新版本，使用最新的技术。  
linux安装java的方法就是下载可执行文件，把bin目录添加到环境变量。  
openjdk的下载页面地址在[https://jdk.java.net/](https://jdk.java.net/),比如24版本的下载页面地址是[https://jdk.java.net/24/](https://jdk.java.net/24/)。本文编辑时的linux amd64下载链接是[https://download.java.net/java/GA/jdk24.0.1/24a58e0e276943138bf3e963e6291ac2/9/GPL/openjdk-24.0.1_linux-x64_bin.tar.gz](https://download.java.net/java/GA/jdk24.0.1/24a58e0e276943138bf3e963e6291ac2/9/GPL/openjdk-24.0.1_linux-x64_bin.tar.gz)
### 安装python
linux系统本身就自带了python3，在命令行输入`python3 -V`，就可以看到版本号了，但是这个python是系统的python，最好不要动，一般情况也不允许动。我们可以使用venv这个库建立一个虚拟环境，作为我们使用的python环境。搭建虚拟环境命令是`python -m venv <虚拟环境位置>`，可能使用的python版本比较精简，没有venv，我们需要安装，一般会提示安装哪个包，一般是python3-venv，我们按需安装就好。最后把python的虚拟环境添加到环境变量，一般是当前目录和Script目录。  
可能没有pip，一方面，可以apt安装python3-pip，看能不能安装，如果不行，就使用pip官方的方法。[https://pip.pypa.io/en/stable/installation/](https://pip.pypa.io/en/stable/installation/)。这是pip官方安装方法的网址。我一般喜欢下载[https://bootstrap.pypa.io/get-pip.py](https://bootstrap.pypa.io/get-pip.py)get-pip.py，然后执行就好。
### 安装golang
一般情况下，linux的golang编写的程序在自己的本地虚拟机编译，然后把可执行文件转移到生产环境，所以我们需要学会怎么安装golang。[https://go.dev/dl/](https://go.dev/dl/)是golang的安装目录，我们下载对应的系统和cpu架构，下载解压就好，最好不要把目录设置成用户下的go文件夹，因为这个位置是放用户的golang的包的，不是放golang程序的。本文编辑时的linux amd64安装链接是[https://go.dev/dl/go1.24.2.linux-amd64.tar.gz](https://go.dev/dl/go1.24.2.linux-amd64.tar.gz)。下载解压后把bin目录添加到环境变量就好。
### 安装nodejs
和上面的一样，都是在官网下载可执行文件，解压到一个文件夹，然后添加环境变量。[https://nodejs.org/zh-cn/download](https://nodejs.org/zh-cn/download)是下载页面。[https://nodejs.org/dist/v22.14.0/node-v22.14.0-linux-x64.tar.xz](https://nodejs.org/dist/v22.14.0/node-v22.14.0-linux-x64.tar.xz)是本文编辑时的最新的linux amd64下载链接。
### 安装docker
我一般使用debian 12这个版本，所以我也只会这个版本，我们到官网文档就可以安装安装教程。[https://docs.docker.com/engine/install/debian/](https://docs.docker.com/engine/install/debian/)是官方的安装教程页面。我把代码贴到下面。  
卸载旧的docker
```bash
for pkg in docker.io docker-doc docker-compose podman-docker containerd runc; do sudo apt-get remove $pkg; done
```
添加安装源
```bash
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/debian \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```
安装
```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
### 安装mysql
mysql是最流行的数据库。我们动态网站很多情况都需要它。mysql最新版本没有在apt源，我们要安装mysql的源。[https://repo.mysql.com/](https://repo.mysql.com/)。这是源的下载页面，目前最新的是[https://repo.mysql.com/mysql-apt-config_0.8.33-1_all.deb](https://repo.mysql.com/mysql-apt-config_0.8.33-1_all.deb)。我们使用dpkg工具安装就好，一般情况会在终端弹出一个页面，我们点击第三个退出就好。可能需要安装依赖，lsb-release和gnupg。然后`apt update`就可以安装mysql-server了。
### 安装php
php是很顽固的语言，只能说除了写网站就一点用都没有了。一般apt不是最新的源，官网也只提供源代码，需要自己编译，我们添加第三方源来进行安装。
```bash
# 下载sury的gpg
wget https://packages.sury.org/php/apt.gpg -O sury.gpg
# 把gpg放到apt的keyrings目录
sudo mv sury.gpg /etc/apt/keyrings/
```
然后在/etc/apt/sources.list.d/sury.list添加下面这个内容，`deb [signed-by=/etc/apt/keyrings/sury.gpg] https://packages.sury.org/php/ bookworm main`，我是debian 12，其他系统和版本使用对应的写法就好了。添加源`apt update`就可以安装不同版本的php了。  
`update-alternatives --list php`可以列出目前有什么php版本。`update-alternatives --set php /usr/bin/php8.4`可以切换目前使用的版本。