---
layout: post
title: PHP匿名函数和闭包
tag: php语法
date: 2018-05-25 16:58:05
---

匿名函数[官方文档][2]

本篇文章的代码测试使用的是[php在线测试工具][1]5.6

###匿名函数
	匿名函数就是没有定义函数名的函数，php从5.3版本开始支持匿名函数
```php
<?php

//普通函数
function test($foo){
    echo $foo;
};
//调用
test('hello world');

//匿名函数
$test = function($foo){
    echo $foo;
};		//定义时要加结束符
//调用
$test('hello world');
```

	匿名函数的作用是方便写回调函数和闭包。
	下面说说我个人简单理解的回调函数和闭包：

- 回调函数
回调函数是一个被被调用者调用的函数，也就是一个函数通过传参被另一个函数调用时，就是回调函数。

```php
<?php

//这个函数就是回调函数
function your_function(){
    echo 'hello world';
}

function test($func){
    if(is_callable($func)){
	    $func();
    }else{
	    return false;
    }
}

```

- 闭包
闭包的特点体现在作用域，闭包函数可以使用父级作用域的变量，并延长父级作用域变量的生命周期，php的闭包需要使用匿名函数来实现，使用了父级局部变量的的回调函数也是闭包函数。

```php
<?php

$foo = [1,2,3,4,5];

//普通写法
function your_function($value){
    return $value += 1;
}

print_r(array_map('your_function',$foo));


//匿名函数闭包写法
print_r(array_map(function($value){
    return $value += 1;
},$foo));
```

	与js不同，PHP函数内部不可以直接读取外部全局变量(需要用global)和父级局部变量(需要用use)

```php
<?php

$foo = [1,2,3,4,5];
$a = 1;
function test($arr){
    $b = 2;
    print_r(array_map(function($value)use($b){
        global $a;
        return $value + $a + $b;
    },$foo));
}

test($foo);
```



----


[1]: http://www.dooccn.com/php5.6/

[2]: http://www.php.net/manual/zh/functions.anonymous.php