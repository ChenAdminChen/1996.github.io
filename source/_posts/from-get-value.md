---
title: get from value
date: 2018.3.14 10:36:45
reward: false
tags: 
    - web
    - from
    - javaScript

---

## 代码如下

```bash

当前台需要向后台提交多条数据时，一般采用from表单中的name属性提交
    （1）$("#id").serialize();
获得数据的格式：naem=xiaoming&sex=1 键值对之间用&连接

    （2）$("#id").serializeArray();
获得返回的是Json对象的数量 格式：[objcet object],[objcet object]
    $.each(data,function(key,field)){
        alert(key); //为index
        alert(field.name); //键值
        alert(field.value);  //为内容值 
    }

    （3）var content= $("#id").serializeArray().reduce(function(m,o){m[o.name] = o.value; return m;}, {});
获得数据的格式：content[[objcet object]],content[[objcet object]]

```
