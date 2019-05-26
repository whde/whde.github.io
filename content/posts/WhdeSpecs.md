---
typora-root-url: ../../static/
title: "创建一个类似CocoaPods的Spec"
date: 2019-05-25T11:12:49+08:00
draft: true
keywords: ["iOS","xcode", "Mac"]
description: "Mac Maven 安装"
tags: ["iOS","Xcode", "Mac"]
categories: ["iOS","Mac"]
author: "Whde"
---




# WhdeSpecs

创建一个类似CocoaPods的Spec Repo，然后将自己的组件放到上面，使用CocoaPods就可以帮我们集成这些组件, 本次将Specs放在Gitee上

1. ##### 创建自己的Specs

   去Gitee上创建一个项目，命名为：`WhdeSpecs`

   ```sh
   cd ~/.cocoapods/repos
   ```

   进入文件夹

   ```sh
   pod repo add WhdeSpecs https://gitee.com/Whde/WhdeSpecs.git
   ```

   成功之后终端显示

   ```sh
   Cloning spec repo 'WhdeSpecs' from 'https://gitee.com/Whde/WhdeSpecs.git'
   ```



2. ##### 按照链接中<a herf='cocoapods.html'>如何创建发布自己的库</a>介绍配置去配置项目和WhdeVersion.podspec

3. ##### 接下来就是如何提交到我们对应的地方，先进入组件文件夹，执行下面语句

   ```sh
   pod repo push WhdeSpecs WhdeVersion.podspec
   git push origin :refs/tags/2.0.0
   ```

4. ##### 成功之后回到~/.cocoapods/repos文件夹，查看是否已经有

   <img src='images/image-20180831113335538.png' alt='示例图片' referrerPolicy='no-referrer'/>

5. ##### 使用

   ```objective-c
   source 'https://gitee.com/Whde/WhdeSpecs.git'
   platform :ios, '8.0'
   inhibit_all_warnings!
   use_frameworks!
   
   target 'Demo' do
       pod 'WhdeVersion'
   end
   ```
