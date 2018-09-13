---
title: javaScript （二）
date: 2018.03.15 16:36:45
tags: 
    - javaScript
    - variable
---

## variable undefined

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;当变量声明未初始化时，则属于undefined,若变量为number类型时属于NaN,并且undefined为false.

``` bash
var a;
console.info(a); //a is undefined

var c = a + 2;
console.info(c);  //c is NaN

if (a === false){
    console.info('undefined equal to false ');
}

```

## const variable

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;const为常量，因此对于常量值时不可修改内容，但对于对象时，该对象将不再受到保护。

``` bash
const value = 9;

value = 7;   //is error ,value is const ,not be updated

const my_object = {'key': 'test'};

my_object.key = 'object';

console.info(my_object);    //{'key': 'object'}

```

### 字符类型转为数字类型

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;除了使用parseInt()、parseFloat()进行转换时，可用(+' ')进行转换。

``` bash
'1.1' + '1.1';    // '1.11.1'

(+'1.1') + (+'1.1');    // 2.2

```

### 数组中的删除与赋值为undefined的区别

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;当用delete删除数组中的某个元素后，in判断当前元素是否存在于数组中,返回一个false,但如果直接赋值undefined，再判断时，则返回true

``` bash
    var trees = ['test', 'test1', 'test2', 'test3'];

    delete trees[3];

    console.info(3 in trees);  //false

    trees[2] = undefined;
    console.info(2 in trees); // true

```

### sort()排序

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;sort()方法是按照字典顺序对元素进行排序，因此它假定元素都是字符串类型，并对其进行比较时，都是字符串类型的比较，对数字进行sort()时，可能会出现错误。

``` bash
# 对字符串进行比较
var names = ["David","Mike","Cynthia","Clayton","Bryan","Raymond"];

names.sort();

print(names); // Bryan,Clayton,Cynthia,David,Mike,Raymond

# 对数字进行对比,其无法得到正确的答案
var nums = [3,2,100,4,200]

nums.sort()

print(nums) //100,2,200,3,4

# 改进

function compare(num1, num2){
    return num1-num2;
}

var nums = [3,1,2,100,4,2900,2];

nums.sort(compare);

print(nums);  //[1, 2, 2, 3, 4, 100, 2900]
```
