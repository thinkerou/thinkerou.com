---
title: PHP 的 empty 函数
categories: [
  "PHP"
]
tags: [
  "PHP"
]
date: 2015-11-08
image: "speakers.jpg"
---

## 问题是这样的

描述问题现象前，先上代码：

    // $ret['errno'] 的值由更新 update 数据库返回
    // update 操作会把 affected_rows 值返回
    if (empty($ret['errno'])) {
        // update 不成功则 insert
    }
    
根据代码看，问题已经很明显了：

> 如果 affected_rows 等于 0，$ret['errno'] 就等于 0，那么它会就是 empty 的吗？

> 熟悉 PHP 的你，肯定觉得是个小儿科的问题了，但是对于刚看 PHP 两天的我来说，这个问题还是值得记录下。

接下来就记录下，在 PHP 里什么内容会被认为是 empty 的？！

## empty 函数判断的是什么

函数 empty 检查一个变量是否为空，函数原型如下：

> bool empty(mixed $var)

> 当变量 $var 不存在或者它的值等同于 `false` 时，那么就会被认为是空的。

> 没有警告产生，即使变量不存在，意味着 empty() 本质上与下列语句等价：

> `!isset($var) || $var === false`

当 $var 存在且是一个`非空非零`的值时返回 `false`，否则返回`true`。

那么，在 PHP 里哪些东西会被认为是空的呢？如下：

> "" 空字符串

> 0 整数0

> 0.0 浮点数0

> "0" 字符串0

> null

> false

> array() 空数组

> $var 已声明但没有值的变量

再回到开始的问题，想当然的以为 $ret['errno']) 被设置为 0 后，empty 就会为 false，或许这跟 isset 函数搞混了。

> bool isset(mixed $var [, mixed $...])

> 检查变量是否设置，且不是 null

> 如果使用 unset 释放变量后，它将不再是 isset

> 如果使用 isset 测试一个被设置为 null 的变量，将返回 false

> 注意：null 字节（"\0"）并不等同于 null 常数

同时需要注意的是，另外一个函数 `is_null` ：

> bool is_null(mixed $var)

> 如果 $var 是 null 则返回 true，否则返回 false

## 参考资料

- [PHP 手册：empty](http://php.net/manual/en/function.empty.php)

- [stackoverflow: How to tell if a PHP array is empty?](http://stackoverflow.com/questions/2216052/how-to-tell-if-a-php-array-is-empty) 

- [stackoverflow: Checking if the string is empty](http://stackoverflow.com/questions/718986/checking-if-the-string-is-empty) 

- [PHP isset() vs empty() vs is_null()](https://www.virendrachandak.com/techtalk/php-isset-vs-empty-vs-is_null/)
