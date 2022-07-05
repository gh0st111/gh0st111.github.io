---
title: Web题目复现
date: 2021-10-19 23:10:33
tags:
categories: 新生赛
top_img: https://images.wallpaperscraft.com/image/single/silhouette_circle_glow_141558_1280x720.jpg
cover: https://images3.alphacoders.com/806/thumbbig-806997.webp
---

## http://172.17.135.34/challenges

### HTML source：

※做题时先看源码，会有意想不到的收获。

Step：查看网页源代码的方式有4种，分别是：1、鼠标右击会看到”查看源代码“，这个网页的源代码就出现在你眼前了；2、可以使用快捷Ctrl+U来查看源码；3、在地址栏前面加上view-source，如view-source：https://www.baidu.com ; 4、浏览器的设置菜单框中，找到“更多工具”，然后再找开发者工具，也可以查看网页源代码。

### Cookies：

※对开发工具和抓包工具的初步体验。

Step：利用f12,f15打开工具，查找flag。

### Location：

※需了解HTTP状态码：https://www.runoob.com/http/http-status-codes.html。

以及对burpsuite抓包的认识和利用。Burpsuite详细教程：https://pan.baidu.com/s/1lH2fDIxb3Ju-dMk6aCc12g提取码：q6t6

Step：用burpsuite抓包进行对请求信息的截取。

### Get_Post：

※：了解两种传参方式即get和post的区别

$_GET：用于在提交带有方法的 HTML 表单后收集表单数据

$_POST：用于在提交带有方法的 HTML 表单后收集表单数据

Step：利用hackbar，将get和post里的内容=if局中的内容即可

### Babyrce：

※1.对函数eval的理解：eval() 函数把字符串按照 PHP 代码来计算。该字符串必须是合法的 PHP 代码，且必须以分号结尾。如果没有在代码字符串中调用 return 语句，则返回 NULL。如果代码中存在解析错误，则 eval() 函数返回 false。2.对蚁剑工具的初步认识和使用。3对php一句话木马的认识：一句话木马就是只需要一行代码的木马，短短一行代码，就能做到和大马相当的功能。为了绕过waf的检测，一句话木马出现了无数中变形，但本质是不变的：木马的函数执行了我们发送的命令。

Step：使用蚁剑添加输入木马地址及密码，然后在根目录下查找flag文件即可。

### Ezinclude：

※了解error_reporting(0);函数：关闭错误报告。及include函数——include() 当使用该函数包含文件时，只有代码执行到 include() 函数时才将文件包含进来，发生错误时只给出一个警告，继续向下执行。强调的是其包含关系。

Step：首先看出是post传参，随后发现post被包含，只需查找目录的的flag的文件即可。

 

### Babyphp：

※php代码审计前奏之ctfshow之php特性：1.强弱类型的比较：可参考https://blog.csdn.net/qq_45086218/article/details/114113971及利用此特性进行数组绕过，从而获取我们想要的信息。2.isset函数特性：https://www.php.net/manual/zh/function.isset.php

Step：由题而知，此题利用get传参，且第一个语句中是强相等，而第二个语句重视弱相等。我们只需在数字后加入一个字符串即可绕过，获取flag。

### Babyphp2：

※对md5的认识：为一种加密算法，一般为单向，不可逆，即生成密文后不能反向解析出原文。从而利用md5漏洞进行绕过：[PHP中MD5函数漏洞总结 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/123235283)

Step：

### HTTP：

※对各种HTTP Headers类型的认识与理解，从而进行各种head的伪造，进而进入内网。

Step：首先伪造成本地127.0.0.1接着伪造Ginkgo浏览器，最后伪造refer即可。

### Babyupload：

※对getshell的认识，可以参考这篇文章：[GetShell的姿势总结 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/337855490)，将命令存进伪造的文件类型从而上传，绕过检查，实现控制。

Step：利用burpuite进行对文件类型的多次伪造。

**总结**：此次web类型题其主要目的在于促进我们对web的简单了解以及对php语法的初识，还有对基本工具burpsuite，hackbar等等的基本使用的锻炼。从web这一角映射出学习网安的框架以及各种困难，任重而道远，加油吧！

 

 















 

 

 