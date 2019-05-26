---
typora-root-url: ../../static/
title: "Python爬取笑话"
date: 2019-05-25T11:04:49+08:00
draft: true
keywords: ["Python爬取笑话","Mac","Python"]
description: "Python爬取www.jokeji.cn笑话"
tags: ["Python", "Mac","笑话","爬虫"]
categories: ["Mac"]
author: "Whde"
---

### Python爬取笑话

Python爬取笑话排行，将数据以json存储到文件中

我们爬取的地址是：http://www.jokeji.cn/hot.asp?action=brow

分析一下，我们需要爬取总页数，然后读取每页下面的笑话，笑话内容需要去详情页去爬取

##### 1、首先我们创建一个程序入口 

- 我们创建一个文件夹，文件夹里最后存数据文件进去
- spider(root_url) 方法开始爬取数据，这个方法我们自己实现

```python
# 程序入口
if __name__ == "__main__":
    # 创建文件夹，最后存数据到这个文件夹下面
    importlib.reload(sys)
    if os.path.isdir(root_folder):
        pass
    else:
        os.mkdir(root_folder)
    # 开始爬取数据
    spider(root_url)
    print('**** spider ****')
```

##### 2、开始爬取数据spider(root_url)

- 先去getpages(url)获取页数pages，后面会讲到
- page(pageurl)获取每页里面数据
- 数据爬完，存储到data.json文件中

```python
# 开始爬取数据
def spider(url):
    list1 = []
    i = 1
    # 去获取排行榜的页数
    pages = getpages(url)
    while i <= int(pages):
        # 拼接每一页的URL地址
        pageurl = 'http://www.jokeji.cn/hot.asp?action=brow&me_page='+str(i)
        print(pageurl)
        # 获取每页下面的内容
        list1 = list1+page(pageurl)
        i = i+1
        pass
    else:
        print('大于页数')

    # 将list存储到data.json中
    try:
        filename = root_folder + 'data.json'
        with open(filename, "wb") as f:
            stri = json.dumps(list1, encoding='UTF-8', ensure_ascii=False)
            print(stri)
            f.write(stri)
        pass
    except Exception as e:
        print('Error:', e)
        pass
    pass
```

##### 3、获取页数getpages(url)

找最后一页的逻辑(如图)，最后一页在这个`>>`按钮里面，我们看源码，可以看到598，598就是总的页数了，接下来就是怎么获取这个页数。我们找到Class="main_title"，然后找这main_title下面的所有td，我们所要的598就在倒数第二个td中，最后处理字符串，取出598。

![最后一页的逻辑](../images/WX20181205-164111@2x.png)

```python
# 获取排行榜的页数
def getpages(url):
    print(url)
    url = quote(url, safe=string.printable)
    headers = {
        'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/67.0.3396.99 Safari/537.36'
    }
    http = urllib3.PoolManager()
    web_data = http.request('GET', url, headers=headers).data
    # web_data = requests.get(url, headers=headers)
    # web_data.encoding = 'gb2312'
    soup = BeautifulSoup(web_data, 'html.parser', from_encoding='GBK')
    print(web_data)
    # 找总页数
    span = soup.find(class_='main_title')
    tds = span.findAll('td')
    td = tds[len(tds)-2]
    pages = td.select('a')[0].get('href').replace('hot.asp?action=brow&me_page=', '')
    print(pages)
    return pages
```

##### 4、page(pageurl)获取每页里面数据

getmeichannel(url)获取当前页下面的笑话列表

然后再detail(herf)去爬取每个笑话的详情

爬取逻辑看图：

![](../images/WX20181205-174145@2x.png)

```python
# 爬取每页下面的内容
def page(url):
    # 获取每页下面的笑话list
    channel_list = getmeichannel(url)
    list1 = []
    for tr in channel_list:
        dict1 = {}
        a = tr.find(class_='main_14')
        herf = 'http://www.jokeji.cn'+a.get('href')
        title = a.get_text()
        print(str(herf)+' --- '+str(title))
        dict1['herf'] = herf
        dict1['title'] = title
        dict1['date'] = tr.find(class_='date').get_text().replace('\r\n          ', '')
        # 获取当前笑话的详情，去详情页爬取数据
        dict1['detail'] = detail(herf)
        list1.append(dict1)
    return list1
```

##### 5、getmeichannel(url)获取当前页下面的笑话列表

```python
# 获取排行榜URL页下面的笑话list数据
def getmeichannel(url):
    url = quote(url, safe=string.printable)
    headers = {
        'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/67.0.3396.99 Safari/537.36'
    }
    http = urllib3.PoolManager()
    web_data = http.request('GET', url, headers=headers).data
    soup = BeautifulSoup(web_data, 'html.parser', from_encoding='GBK')
    channel = []
    tables = soup.findAll(height='30')
    for table in tables:
        try:
            for tr in table.findAll('tr'):
                channel.append(tr)
                pass
        except Exception as e:
            print('Error:', e)
            pass
    return channel
```

##### 6、获取详情数据detail(herf)

```python
# 爬取详情页数据
def detail(url):
    url = quote(url, safe=string.printable)
    headers = {
        'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/67.0.3396.99 Safari/537.36'
    }
    http = urllib3.PoolManager()
    r = http.request('GET', url, headers=headers)
    web_data = r.data
    soup = BeautifulSoup(web_data, 'html.parser', from_encoding='GBK')
    # 查找详情页数据
    font = soup.find(attrs={'id': 'text110'})
    try:
        return font.get_text()
        pass
    except Exception as e:
        print(str(e))
        return ''
        pass
```





源码地址https://github.com/whde/Joke.git
