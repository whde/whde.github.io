---
typora-root-url: ../../static/
title: "获取iOS 设备的 UDID"
date: 2019-05-25T11:11:49+08:00
draft: true
keywords: ["iOS","UDID","Xcode", "Safari"]
description: "Mac Maven 安装"
tags: ["iOS","UDID"]
categories: ["iOS"]
author: "Whde"
---

### iOS 设备的 UDID
**<font color="#006600">什么是 UDID？</font>**
UDID 是由子母和数字组成的 40 个字符串的序号，用来区别每一个唯一的 iOS 设备，包括 iPhones, iPads, 以及 iPod Touches，这些编码看起来是随机的，实际上是跟硬件设备特点相联系的。

**<font color="#006600">如何获取 iOS 设备 UDID？</font>**

 <img style="width: 187px; cursor: pointer;" alt="" src="https://whde.gitee.io/images/651">

在 iOS 设备上打开下面的地址，即可方便的获取到当前设备的 UDID。

<span style="color:#006600;">https://www.pgyer.com/udid</span>

注意：请根据网页的提示，安装蒲公英提供的描述文件。*<font color="#006600">如果手机设置了锁屏密码，则需要根据提示输入锁屏密码。</font>*

  <img style="width: 187px; cursor: pointer;" alt="" src="https://whde.gitee.io/images/652">

 <img style="width: 187px; cursor: pointer;" alt="" src="https://whde.gitee.io/images/650">

 <img style="width: 187px; cursor: pointer;" alt="" src="https://whde.gitee.io/images/653">

<span style="color:#df402a;background-color:#FAE220;">复制UDID，给开发工程师</span>



**<font color="#006600">PS：</font>**
​    这个过程安装了一个蒲公英的描述文件，在拿到UDID之后，我们可以在手机上删掉它，描述文件路径：
​    <span style="color:#df402a;background-color:#FAE220;">设置->通用->描述文件->蒲公英</span>
