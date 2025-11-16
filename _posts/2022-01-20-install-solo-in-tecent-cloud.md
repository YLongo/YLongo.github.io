---
layout: post
title: "Install Solo In Tecent Cloud"
date: 2022-01-20 00:00
tags: [deployment, docker]
---



1. 安装 rz/sz 
`yum -y install lrzsz`

2. 安装 mysql
`https://dev.mysql.com/doc/refman/8.0/en/linux-installation-yum-repo.html
`


3. 安装 JDK1.8
`yum search java|grep jdk
`
`yum install java-1.8.0-openjdk
`

4. 安装 javac
`yum install java-devel
`

5. 安装 tomcat8
    1. 获取
`wget http://mirrors.tuna.tsinghua.edu.cn/apache/tomcat/tomcat-8/v8.5.39/bin/apache-tomcat-8.5.39.tar.gz
`

    2. 解压
`tar -zxvf apache-tomcat-8.5.39.tar.gz -C /usr/local/
`


6. 安装 Maven
    1. 下载
`wget http://mirrors.tuna.tsinghua.edu.cn/apache/maven/maven-3/3.6.0/binaries/apache-maven-3.6.0-bin.tar.gz
`
    2. 解压
`tar -zxvf apache-maven-3.6.0-bin.tar.gz -C /usr/local/
`

    3. 添加环境变量
`export PATH=/usr/local/apache-maven-3.6.0/bin:$PATH
`

7. 安装 docker
`https://docs.docker.com/install/linux/docker-ce/centos/`

然后根据 [Solo]([https://github.com/b3log/solo](https://github.com/b3log/solo)) 的文档 Docker 一键部署（比通过源码搭建简单太多，我之前为什么要通过源码去折腾:joy:）。
