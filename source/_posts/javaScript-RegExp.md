---
title: javaScript Regular Expression 
date: 2018/1/10 09:36:45
tags: 
    - javaScript
    - RegExp
---

## Regular Expression

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;在javaScript中正则表达式也是一个Object,[学习地址](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions "javaScript regular expression")

### 创建一个RegExp
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;javaScript中创建一个RegExp，有两种方法：
``` bash
# 利用RegExp对象创建
var re = new RegExp('abc')

# 利用特别的标记创建 /RegExp/
var re = /abc/
```

### 特殊字符的用意

特殊字符   | 用法                                         | example
   *      | 出现0次或者多次                               | re = /a*bc/              :"abc" is true, "bc" is true
   +      | 出现1次或者多次                               | re = /a+bc/              :"abc" is true, "bc" is false
   ?      | 出现0次或者一次                          | re = /e?le?/         :"angle" is le, "angele" is ele, "angl" is l, "angeel" is el
   \      | 对于\后面的字符不进行转译，当作原来的意途用      | re = /\//                :查找包括/的字符串。
   ^      | 当它出现在表达式的最前面时，则表式紧跟它后面的字符必须出来在字符串的最前面  | re = /^A/   : "an A" is false, "A B" is true
   $      | 以什么结尾                                    | re = /t$/                 :"eater" is false, "eat" is true
