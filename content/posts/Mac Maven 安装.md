---
typora-root-url: ../../static/
title: "Mac Maven 安装"
date: 2019-05-25T11:10:49+08:00
draft: true
keywords: ["Maven","Mac","Java", "Server"]
description: "Mac Maven 安装"
tags: ["Maven", "Mac","Java"]
categories: ["Mac"]
author: "Whde"
---

## **Mac Maven 安装** 

下载地址：http://maven.apache.org/download.cgi

选择这个版本下载：

![image-20181030105835411](static/images/image-20181030105835411.png)

下载已经存放到Downloads文件夹，先进入Downloads文件夹

```shell
cd Downloads
```

解压：

```shell
tar -xvf  apache-maven-3.5.4-bin.tar.gz
```

复制到对应文件下：

```shell
sudo mv -f apache-maven-3..5.4 /usr/local/
```

编辑 /etc/profile 文件：

```shell
cd ~
sudo vim /etc/profile
```

在/etc/profile文件末尾添加如下代码，然后保存文件

```shell
export MAVEN_HOME=/usr/local/apache-maven-3.5.4
export PATH=${PATH}:${MAVEN_HOME}/bin
```

并运行如下命令使环境变量生效：

```shell
source /etc/profile
```

查看是否生效，输入：

```shell
 mvn -v
```

显示如下信息，则说明 Maven 已经安装成功：

```sh
Apache Maven 3.5.4 (1edded0938998edf8bf061f1ceb3cfdeccf443fe; 2018-06-18T02:33:14+08:00)
Maven home: /usr/local/apache-maven-3.5.4
Java version: 11.0.1, vendor: Oracle Corporation, runtime: /Library/Java/JavaVirtualMachines/jdk-11.0.1.jdk/Contents/Home
Default locale: zh_CN_#Hans, platform encoding: UTF-8
OS name: "mac os x", version: "10.14", arch: "x86_64", family: "mac"
```

