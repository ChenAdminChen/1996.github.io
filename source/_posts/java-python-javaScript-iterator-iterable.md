---
title: java-python-javaScript-iterator-iterable学习
tags: 
    - java
    - python
    - javaScript
    - iterator
categories: iterator
---


## java

### iterator

### iterable
 
 
## python

### iterable/可迭代的
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;当对象中存在_iter_方法时，该对象就是iterable对象，若一般对象实现_iter_方法时，它将成为iterable，其在python中iterable的有以下：[列表、字典、元组]

``` bash
l = [1,2,3]
dir(l)            # _iter_
iter(l)           # <list_iterator at 0x69c8048>


In [12]: iter(123)
---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
<ipython-input-12-7ce543a15833> in <module>()
----> 1 iter(123)

TypeError: 'int' object is not iterable


In [21]: dict = {'Alice': '2341', 'Beth': '9102', 'Cecil': '3258'}

In [22]: iter(dict)
Out[22]: <dict_keyiterator at 0x59d81d8>     #返回一个键的可迭代对象

In [25]: next(dict)                          #字典不是一个迭代器，它只是一个迭代对象
---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
<ipython-input-25-9c216d0aadb1> in <module>()
----> 1 next(dict)

TypeError: 'dict' object is not an iterator


In [23]: l = [1,2,3]

In [24]: iter(l)
Out[24]: <list_iterator at 0x69ea710>       #返回一个值本身的可迭代对象

```

自定义迭代对象

``` bash
In [42]: class String(object):
    ...:
    ...:     def __init__(self, val):
    ...:
    ...:         self.val = val
    ...:
    ...:     def __str__(self):
    ...:
    ...:         return self.val
    ...:
    ...:     def __iter__(self):
    ...:
    ...:         print("This is __iter__ method of String class")
    ...:
    ...:         return iter(self.val)  #self.val is python string so iter() will return it's iterat
    ...:

In [43]: s = String("test")

In [44]: iter(s)
This is __iter__ method of String class
Out[44]: <str_iterator at 0x56c1400>
```

### iterator/迭代器
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;当对象中存在_iter_方法,并且存在_next_()方法时，它将是一个iterator,迭代器可用for lopp 进行获取值，可以调用next()方法进行获取值。
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;iterator对象存在一次性特点，当使用next()或_next_()获得值时，若对象不存在下一个值时，raise stopIteration 异常
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;iter()函数为一个可迭代对象返回一个迭代器,如果给iter函数传入一个迭代器，它就直接返回那个迭代器
``` bash
#迭代器 实现逻辑
class Iterator:

    def __init__(self, iterable)

        self.iterable = iterable

    def __iter__(self):  #iter should return self if called on iterator

        return self

    def next(self):  #Use __next__() in python 3.x

        if condition: #it should raise StopIteration exception if no next element is left to return

            raise StopIteration
```

## javaScript

### iterator

### iterable
