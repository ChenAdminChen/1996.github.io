---
title: mongodb 基本语法学习（一）
tags: mongodb
categories: mongodb
---


## mongodb 基本语法操作

### mongodb的数据库切换操作

``` bash

show dbs;   #查看mongodb数据库中存在的数据库
use datebase_name;   #使用指定的数据库
show collections;   #查看某个数据库下所有的collction

```

### 对collection操作

#### json的数据格式
 
``` bash

consult: 
{
    "_id": 111,  
    'user_id':'1234@163.com',

    'public': false,
    'time':'2017.11.01 12:25:33'

    'number':1
    'title':'如果长高',
    'content':'怎么才能长高？？？？',
    'resouces':[
        {
            'url':""
            'type':""
            'comment':""
        }
    ]
    'reply':[
        {
            'content':'test1'
            'time':""
            'user_id': '123@yf.com'
        },
        {
            'content':'test2'
            'time':
            'user_id': '222@yf.com'     
        }
    ]
}

```

#### find()操作
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;以下基于该json数据进行操作

``` bash
db.consult.find().pretty();   #查看consult下的数据并以json格式展示
db.consult.find({'_id':111});   #查找_id为111的数据
db.consult.find({'_id':111,'reply.user_id':'222@yf.com'});   #查找 _id: 111,reply数据组中user_id ： '222@yf.com'的数据

```

#### remove()操作

``` bash
db.consult.remove()   #删除consult内的数据
```

#### update()操作
``` bash
db.consult.update({'_id':111},{$set:{'public':false}});   #修改 _id : 111中字段public的值

#数组修改器push() 数组中存在则修改，若不存在则添加
db.conuslt.update({'_id':111,'reply.user_id':'222@yf.com'},{$push:{'reply.content':'测试'}});   #修改 '_id':111中reply中数组中'reply.user_id':'222@yf.com'的content值

#对json内的数值字段进行操作，$inc
db.consult.update({'_id':111},{$inc:{'number' : 1 }});   #将json内的number值加1
db.consult.update({'_id':111},{$inc:{'number ': -1 }});   #将json内的number值减1

#对json内普通字段进行操作，并能改变字段类型，$set
db.consult.update({'_id':111},{$set:{'number' : 3 }});   #修改number值
db.consult.update({'_id':111},{$set:{'number' : 'test1' }});  #将number字段类型数值型改为字符串型
```
