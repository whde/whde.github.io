---
typora-root-url: ../../static/
title: "爬取笑话网站数据，存储到MySQL数据库"
date: 2019-05-25T11:08:49+08:00
draft: true
keywords: ["MySQL","Mac","Python"]
description: "Python爬取www.jokeji.cn笑话, 存储到MySQL数据库"
tags: ["MySQL", "Mac","笑话","爬虫"]
categories: ["Mac","Server"]
author: "Whde"
---

### 爬取笑话网站数据，存储到MySQL数据库

怎么爬取数据在[Python爬取笑话数据](../html/Joke.html)中有详细说明

本文基于这篇文章进行，将数据存储到MySQL

首先要连接MySQL，需要引入 `pymysql`

```python
import pymysql
```



接着连接MySQL，并创建我们的table，db=joke需要事先创建好

```python
def connectdb():
    db1 = pymysql.connect(
        host="localhost",
        user="root",
        passwd="666666",
        port=3306,
        db="joke")
    cursor = db1.cursor()
    cursor.execute("DROP TABLE IF EXISTS joke")
    sql = """CREATE TABLE joke (herf TEXT NOT NULL, title TEXT, date TEXT, detail TEXT)"""
    try:
        cursor.execute(sql)
        cursor.execute("""SET SQL_SAFE_UPDATES = 0;""")
        pass
    except Exception as e:
        print(str(e))
        pass
    return db1
```

接下来就是这么存到数据库中，我们存储连接(herf)，标题(title)，发布时间(date)，详情(detail)。如果存储失败，我们需要回滚操作

```python
def insetdb(db1, herf, title, date, detail):
    sql = "insert into joke(herf, title, date, detail) \
    values ('%s', '%s', '%s', '%s');" % \
       (herf, title, date, detail)
    try:
        cursor = db1.cursor()
        cursor.execute(sql)
        db1.commit()
        pass
    except Exception as e:
        print(str(e))
        db1.rollback()
        pass
```

在执行的最后，我们需要将数据库关闭

```python
db2.close()
```



[github](https://github.com/whde/JokeMysql)

[根据笑话数据写的服务端](../html/JokeServer.html)

[根据服务端写的安卓App](../html/JokeAndroid.html)

[根据服务端写的iOSApp](../html/JokeiOS.html)

