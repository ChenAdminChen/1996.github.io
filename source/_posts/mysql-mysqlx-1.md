---
title: mysqlx json function
date: 2017/11/22 09:40:45
reward: false
tags: 
    - mysql
    - mysqlx
categories: mysqlx
---

## mysqlx对json类型的基本操作

### 生成json格式的数据
#### 生成json格式的普通数据
``` bash
mysql> SELECT JSON_OBJECT('id', 87, 'name', 'carrot');
+-----------------------------------------+
| JSON_OBJECT('id', 87, 'name', 'carrot') |
+-----------------------------------------+
| {"id": 87, "name": "carrot"}            |
+-----------------------------------------+
```
#### 生成array型的数据
``` bash
mysql> SELECT JSON_ARRAY(1, "abc", NULL, TRUE, CURTIME());
+---------------------------------------------+
| JSON_ARRAY(1, "abc", NULL, TRUE, CURTIME()) |
+---------------------------------------------+
| [1, "abc", null, true, "11:30:24.000000"]   |
+---------------------------------------------+
```
#### 生成array包括json型的数据
``` bash
mysql> SELECT JSON_ARRAY(JSON_OBJECT('id', 87, 'name', 'carrot'), JSON_OBJECT('id', 87, 'name', 'carrot'));
+---------------------------------------------+
|  JSON_ARRAY(JSON_OBJECT('id', 87, 'name', 'carrot'), JSON_OBJECT('id', 87, 'name', 'carrot')) |
+---------------------------------------------+
| [{"id": 87, "name": "carrot"}, {"id": 87, "name": "carrot"}]   |
+---------------------------------------------+
```

### 修改json格式的操作

#### mysql表中的结构

``` bash
CREATE TABLE `consult` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `user_id` varchar(45) NOT NULL,
  `specialist_id` varchar(45) DEFAULT NULL,
  `public` int(11) DEFAULT NULL,
  `title` varchar(45) DEFAULT NULL,
  `content` varchar(45) DEFAULT NULL,
  `resouce` json DEFAULT NULL,   #格式：{}
  `reply` json DEFAULT NULL,   #格式：[{},{}]
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=2 DEFAULT CHARSET=utf8;
``` 

#### 修改普通格式的json数据

``` bash
#添加
update consult set doc = JSON_INSERT(resouce,'$.resouce',"http://img/imgs.jpg") where id = 1;
#修改
update consult set doc = JSON_REPLACE(resouce,'$.resouce',"http://img/imgs.jpg") where id = 1;
#删除
update consult set doc = JSON_remove(resouce,'$.resouce') where id = 1;
```
#### 修改带arrag的json数据
``` bash
#reply中具体格式
reply:
[
	{
		"time": "2017-11-21 16:49:42.000000",
		 "comment": "可以多运动",
		  "user_id": "456@yf.com",
		  "index":0  //该index的值是这条数据在数组中的下标，自行添加并管理
	}
]

# 对json里的数组进行操作
update consult set reply = NULL where id =1;
update consult set reply = '[{"time": "2017-11-21 16:49:42.000000", "comment": "可以多运动", "user_id": "456@yf.com","index":0}]' where id = 1;

#在json格式里的数组添加值
update consult set reply =  JSON_ARRAY_INSERT(reply,concat('$[',JSON_LENGTH(reply),']'),JSON_OBJECT('user_id','12355@yf.com','time',now(),'comment','但是感觉没有太多的作用','index',JSON_LENGTH(reply))) where id = 1;

#修改json格式里的数组信息
update consult set reply = JSON_REPLACE(reply,'$[2]',JSON_OBJECT('user_id','12355@yf.com','time',now(),'comment','测试修改','index',2)) where id =1;

```

#### 删除json内的数据
``` bash
#指定删除json里数组的某个值
update consult set reply = JSON_REMOVE(reply,'$[2]') where id = 1;

db.collection.update({'id': , 'reply.user_id': },{$set:{ }})

db.collection.update({'id':1 , 'reply.user_id："123@yf.com"   },{$set:{'reply.' }})
```

#### 获得json中array的长度

``` bash
select JSON_LENGTH(reply) from consult where id = 1;
```
