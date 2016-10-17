---
layout: post  
title: Linux中安装java
categories: [java]  
tags: [Java]  
description: 「如何在Linux中安装java」   
---

## 下载
到[这里](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)下载适合你操作系统的文件，比如jdk-8u101-linux-x64.tar.gz

## 安装
将下载好的文件放到任意目录，比如/home/user/下，用如下命令解压：

> tar zxvf jdk-8u101-linux-x64.tar.gz -C 目标文件夹

如果是解压到当前文件夹可以直接用

> tar zxvf jdk-8u101-linux-x64.tar.gz

解压完成后，需要设置环境变量。假设现在的目录结构是

> /home/user/jdk1.8.0_101/

jdk1.8.0_101/ 就是jdk-8u101-linux-x64.tar.gz解压生成的文件夹。

分别打开位于/home/user/ 目录下的.profile文件和.bashrc文件，分别在它们末尾添加以下列语句并保存

> export JAVA_HOME=/home/user/jdk1.8.0_101
> export JRE_HOME=${JAVA_HOME}/jre
> export CLASSPATH=.:${JAVA_HMOE}/lib:${JRE_HOME}/lib
> export PATH=${JAVA_HOME}/bin:$PATH

这样就完成了安装。注意要重新登录才能生效。

## 验证
重新登录之后，用

> java -version

命令可以查看及验证安装好的java版本
