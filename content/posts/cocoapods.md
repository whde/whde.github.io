---
typora-root-url: ../../static/
title: "Cocoapods安装发布自己库"
date: 2019-05-25T10:59:49+08:00
draft: true
keywords: ["Cocoapods安装","Xcode","Mac","Pod"]
description: "Cocoapods安装发布自己库"
tags: ["Cocoapods", "Mac","Xcode"]
categories: ["Mac"]
author: "Whde"
---


<p><img src='http://upload-images.jianshu.io/upload_images/1623336-1482dba0c4f71e4d.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240' alt='示例图片' referrerPolicy='no-referrer'/></p>
<p>这里不啰嗦Cocoapods有什么用,直接上如何使用,关于有什么用,相信各大搜索引擎比我解释更全面;<p>

## Cocoapods安装

##### 1.Mac终端输入

```objective-c
sudo gem install cocoapods
```



##### 2.输入电脑密码即可开始安装,等待...界面出现
<img src="http://upload-images.jianshu.io/upload_images/1623336-a8fd4a8315bf431b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240"  alt='示例图片' referrerPolicy='no-referrer'/>



##### 3.继续终端输入

```objective-c
pod setup
```

等待界面出现
<img src="http://upload-images.jianshu.io/upload_images/1623336-b2c343af14d1fa05.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240"  alt='安装成功' referrerPolicy='no-referrer'/>



##### 4.终端输入以下代码,查看版本号

```objective-c
--version
```





## 写自己的库

##### 1.写完代码, 将自己的库上传到github,要生成一个Release版本

![进入Release仓库](http://upload-images.jianshu.io/upload_images/1623336-ad494032b26c9b31.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![创建新Release版本](http://upload-images.jianshu.io/upload_images/1623336-6fd535b6a5b305a1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![填写信息,发布Release版本](http://upload-images.jianshu.io/upload_images/1623336-65ef7cdc47d7a438.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![版本信息](http://upload-images.jianshu.io/upload_images/1623336-29e6ad8bd5aa8aa3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
接下来就看怎么将这个Release版本弄到Cocoapods上.

##### 2.创建.podspec文件

终端cd到项目文件夹下
![文件结构](http://upload-images.jianshu.io/upload_images/1623336-a75c551ef8c2eef2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![我的项目就cd到WhdeLocalized文件夹下](http://upload-images.jianshu.io/upload_images/1623336-85c2774a37da9435.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
终端输入代码创建.podspec文件,代码中Language对应项目名

```objective-c
pod spec create Language
```
用Xcode打开这个Language.podspec文件, 填写以下代码:
```objective-c
Pod::Spec.new do |s|
s.name          = "Language"
s.version       = "1.0.4"
s.summary       = "iOS Language."
s.homepage      = "https://github.com/whde/WhdeLocalized"
s.license       = 'MIT'
s.author        = { "Whde" => "460290973@qq.com" }
s.platform      = :ios, "7.0"
s.source        = { :git => "https://github.com/whde/WhdeLocalized.git", :tag => s.version.to_s }
s.source_files  = 'Language/Language/Language/*'
s.frameworks    = 'Foundation'
s.requires_arc  = true
s.description   = <<-DESC
It is a Language used on iOS, which implement by Objective-C.
DESC
end
```
key对应的信息
```objective-c
s.name(项目名称)
s.version(Release版本号,必须和Github上的Release版本号对于)
s.summary(对项目总结性的语言)
s.homepage(Github上项目的地址)
s.license(默认'MIT')
s.author(用户信息;自己的名字,自己的邮箱)
s.platform(支持的版本)
s.source(项目的git地址)
s.source_files(告诉别人,使用你的库,需要添加的文件在哪里)
s.frameworks(这项目需要添加的库)
s.requires_arc(是否支持ARC)
s.description   = <<-DESC
(更详细的描述)
DESC
end
```
##### 3.检查.podspec文件是否有问题

终端输入

```objective-c
pod spec lint Language.podspec
```
有什么问题, 会提示出来, 按照它的提示去修改, 不会改, 注意和给出的事例对比, 直到出现以下的结果

![这个结果表示.podspec文件没有问题](http://upload-images.jianshu.io/upload_images/1623336-a7be54c5829f7fb3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



##### 4.上传.podspec文件

终端输入

```objective-c
pod trunk push Language.podspec
```
![出现这个结果表示已经上传上去了](http://upload-images.jianshu.io/upload_images/1623336-58d44db01f8795f2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



##### 5.检查上传结果

终端输入

```objective-c
pod search Language
```
![上传结果及信息](http://upload-images.jianshu.io/upload_images/1623336-fdf90cf2d71b63d7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)





##### 6.使用

在这里就不详细说Cocoapods使用了, 附上代码

```objective-c
pod 'Language', '~> 1.0.4'
```
