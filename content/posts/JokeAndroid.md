---
typora-root-url: ../../static/
title: "Python爬取笑话的Android软件"
date: 2019-05-25T11:04:49+08:00
draft: true
keywords: ["Android","Mac","Python"]
description: "Python爬取www.jokeji.cn笑话, Android软件"
tags: ["Android", "Mac","笑话","爬虫"]
categories: ["Mac", "Android"]
author: "Whde"
---

### JokeAndroid

[github地址](https://github.com/whde/JokeAndroid)

[对应的服务器](../html/JokeServer.html)

使用了`Okhttp`网络模块

`fastjson`解析

```
implementation 'com.squareup.okhttp3:okhttp:3.11.0'
implementation group: 'com.alibaba', name: 'fastjson', version: '1.2.51'
```

`JokeSwipeRefreshLayout`实现了上下拉加载

效果图如下：

<figure class="half">
<img src="/../images/Screenshot_1544077808.png"  width=45%/>
<img src="/../images/Screenshot_1544077813.png"  width=45%/>
</figure>


