---
title: 常见 C 语言宏使用记录
categories: [
  "C"
]
tags: [
  "C"
]
date: 2016-07-30
image: "fast-lane.jpg"
---

### 1. 使用宏防止头文件被重复包含

    #ifndef COMMON_DEFINE_H
    #define COMMON_DEFINE_H
    // 头文件相关内容
    #endif

### 2. 使用宏来注释代码

    #if 0
    // 被注释的代码段，可以包含 //、/**/ 这样的注释代码
    #endif
    
### 3. 使用小括号防止宏错误

    #define MAX(a, b) (((a) > (b)) ? (a) : (b))
    #define MIN(a, b) (((a) < (b)) ? (a) : (b))
    
### 4. 字符、字符串转换

- 使用 ## 连接两个字符串
- 使用 # 转换为字符串（加双引号）
- 使用 #@ 转换为字符（加单引号）

示例代码：

    #define CON(x, y)  x##y
    #define STR(x) #x
    #define CHR(x) #@x

    CON(hi, 123) // hi123
    STR(helloworld) // "helloworld"
    CHR(1) // '1'
    
### 5. 使用局部作用域防止重定义

先看看如下示例代码：

    #define php_grpc_add_property_zval(res, name, val) \
    zval tmp; \
    tmp = *val; \
    add_property_zval(res, name, &tmp);
    
然后有如下宏使用：

    void func()
    {
        php_grpc_add_property_zval(res, "key", val1);
        php_grpc_add_property_zval(res, "value", val2);
    }
    
想想，会有什么问题呢？

会产生如下编译错误：`error: redefinition of 'tmp'`

那应该如何避免这种重定义错误呢？

使用这样的代码会解决问题吗？

    #define php_grpc_add_property_zval(res, name, val) { \
    zval tmp; \
    tmp = *val; \
    add_property_zval(res, name, &tmp); }
    
或者这样的代码：

    #define php_grpc_add_property_zval(res, name, val) do { \
    zval tmp; \
    tmp = *val; \
    add_property_zval(res, name, &tmp); \
    } while(0)
    
没错，使用 `{}` 或 `do {} while(0)` 的**局部作用域**即可解决重定义问题。

