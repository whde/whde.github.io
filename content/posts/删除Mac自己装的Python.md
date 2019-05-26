---
typora-root-url: ../../static/
title: "删除Mac自己装的Python"
date: 2019-05-25T11:01:49+08:00
draft: true
keywords: ["Python","删除","Mac"]
description: "获取Xcode模拟器下载地址"
tags: ["Python", "Mac"]
categories: ["Mac"]
author: "Whde"
---

删除Mac自己装的Python，保留系统自带版本

```sh
// 删除框架
sudo rm -rf /Library/Frameworks/Python.framework/Versions/3.7

// 删除应用目录
sudo rm -rf '/Applications/Python 3.7'

// 删除指向python的链接
cd /usr/local/bin/
ls -l /usr/local/bin | grep '../Library/Frameworks/Python.framework/Versions/x.x' | awk '{print $9}' | tr -d @ | xargs rm

```





