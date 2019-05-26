---
typora-root-url: ../../static/
title: "iOS代码统计"
date: 2019-05-25T11:03:49+08:00
draft: true
keywords: ["iOS代码统计","Xcode","Mac","cloc"]
description: "Cocoapods安装发布自己库"
tags: ["cloc", "Mac","Xcode","代码统计"]
categories: ["Mac"]
author: "Whde"
---

# iOS代码统计（空行、注释、代码）

### 下载工具

工具cloc地址：https://github.com/AlDanial/cloc
先去下载Release下最新版本

### 执行下面脚本：

```sh
# 脚本解释：
# /Users/Whde/Downloads/cloc-1.80/cloc 为下载的Cloc工具的路径
# /Users/Whde/Documents/Demo-IOS/trunk/Demo为项目的路径
perl  /Users/Whde/Downloads/cloc-1.80/cloc  /Users/Whde/Documents/Demo-IOS/trunk/Demo
```

### 得到结果如下：

```sh
➜  Demo perl  /Users/Whde/Downloads/cloc-1.80/cloc  /Users/Whde/Documents/Demo-IOS/trunk/Demo
    1537 text files.
    1507 unique files.
     183 files ignored.

github.com/AlDanial/cloc v 1.80  T=3.29 s (412.2 files/s, 41728.3 lines/s)
-------------------------------------------------------------------------------
Language                     files          blank        comment           code
-------------------------------------------------------------------------------
Objective C                    529          12616           8624          58317
C++                             62           2015           1287          13696
C/C++ Header                   621           6308          15081          10894
JSON                           111              0              0           2517
Markdown                        14            682              0           2142
Objective C++                    3            148             28            787
Bourne Shell                     8            172            286            769
C                                2             46             19            561
HTML                             1              8              0             62
D                                2              0              0              9
JavaScript                       1              0              5              2
-------------------------------------------------------------------------------
SUM:                          1354          21995          25330          89756
-------------------------------------------------------------------------------
```

