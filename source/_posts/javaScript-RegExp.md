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
   
## javaScript中给某个标签赋值操作

``` bash
# demo is tag id name
document.getElementById("demo").innerHTML = "content";
#demo is tag class name
document.getElementsByClassName("demo").innerHTML = "content";
# demo is tag name name
document.getElementsByName("demo").innerHTML = "content";
# input is tag name
document.getElementsByTagName("input").innerHTML = "content";
```
### getElementByName
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;该方法与 getElementById() 方法相似，但是它查询元素的 name 属性，而不是 id 属性。
另外，因为一个文档中的 name 属性可能不唯一（如 HTML 表单中的单选按钮通常具有相同的 name 属性），所有 getElementsByName() 方法返回的是元素的数组，而不是一个元素。

### getElementsByTagName
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;getElementsByTagName() 方法可返回带有指定标签名的对象的集合。
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;getElementsByTagName() 方法返回元素的顺序是它们在文档中的顺序。
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;如果把特殊字符串 "*" 传递给 getElementsByTagName() 方法，它将返回文档中所有元素的列表，元素排列的顺序就是它们在文档中的顺序。
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;传递给 getElementsByTagName() 方法的字符串可以不区分大小写。

## function object对象
``` bash
#person is object
var person = {
    age: 13,
    name: "test",
    sex: "男"
}
#第一种取值方法
var age1 = person.age; # age1 is 13
#第二种取值方法
var age2 = person["age"];  # age2 is 13
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;当变量声明时使用了“new”关键字是，则代表为object对象

## String的特殊方法
### slice()（python）
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;空格也算一个字符串
``` bash
var str = 'Apple, Banana, Kiwi';
#slice(start,end) 可以从后往前数
var pos = str.slice(-12,-9); # pos = Ban
var pos1 = str.slice(-12); # pos1 = Banana, Kiwi
``` 
### concat()(mysql)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;两个字符串拼接

``` bash
var text1 = "Apple";
var text2 = "Banana";
var text3 = text1.concat(" ",text2);  #Apple Banana
```
### charAt() & charCodeAt()
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;charAt从一个字符串中获得一个字符
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;charCodeAt从一个字符串中获得一个字符unicode

## number的特殊方法
### toExponential()
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;该方法是将number转换成string,参数为小数点后的几位
``` bash
var x = 9.656;
x.toExponential(2);     // returns 9.66e+0
x.toExponential(4);     // returns 9.6560e+0
x.toExponential(6);     // returns 9.656000e+0
```

### toFixed()
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;该方法是将number转换成string,参数为该数的小数点后的位数，像数学里的四舍五入，但它是指定小数点后的位数
``` bash
var x = 9.656;
x.toFixed(0);           // returns 10
x.toFixed(2);           // returns 9.66
x.toFixed(4);           // returns 9.6560
x.toFixed(6);           // returns 9.656000
```

### toPrecision()
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;该方法是将number转换成string,参数为该数的位数，像数学里的四舍五入，但它是指定数的位数
``` bash
var x = 9.656;
x.toPrecision();        // returns 9.656
x.toPrecision(2);       // returns 9.7
x.toPrecision(4);       // returns 9.656
x.toPrecision(6);       // returns 9.65600
```
### number中的类型转换
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;都是从第一位开始转换，若第一个字符串不是数字的则不能转换，返回NaN(Not-a-Number)
``` bash
Number("12")  # 12
parseFloat("12.5") #12.5
parseInt("12.5") #12

x = true;         //在javaScript中 true为1，其他的数据都为false
Number(x);        // returns 1
x = false;     
Number(x);        // returns 0
x = new Date();
Number(x);        // returns 1404568027739
x = "10"
Number(x);        // returns 10
x = "10 20"
Number(x);        // returns NaN
x = "10 test";
Number(x);       // returns 10
x = "test 10";
Number(x)       // returns NaN  
```
### number的特殊几个类型
| 属性 | 结果 |
|:-------|:-----:|
|MAX_VALUE|	Returns the largest number possible in JavaScript|
|MIN_VALUE|	Returns the smallest number possible in JavaScript|
|NEGATIVE_INFINITY|	Represents negative infinity (returned on overflow)|
|NaN| Represents a "Not-a-Number" value|
|POSITIVE_INFINITY|	Represents infinity (returned on overflow)|

## Array
### 三种方法判断是否是Array

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Array.isArray()判断某个变量是否是数组 true /false
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;使用关键判断instanceof (var instanceof Array)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;自定义方法判断

``` bash
function isArray(x) {
    return x.constructor.toString().indexOf("Array") > -1;
}
```
### 将Array的值转换成string
``` bash
// toString()
var fruits = ["Banana", "Orange", "Apple", "Mango"];
document.getElementById("demo").innerHTML = fruits.toString();

//join()
var fruits = ["Banana", "Orange", "Apple", "Mango"];
document.getElementById("demo").innerHTML = fruits.join();

```

### pop()与push()
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;javaScript中对array的操作像对队列一样，使用pop()与push()两种函数可以对数据进行操作
